---
title: Serial Communication
parent: Protokoły komunikacyjne
nav_order: 4
layout: page
---


# Serial Communication
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Wprowadzenie 
Trzy powszechnie stosowane warstwy fizyczne komunikacji szeregowej to RS232, RS422 oraz RS485. Na każdej z nich można uruchomić dowolną warstwę aplikacyjną – protokół komunikacyjny. Najczęściej spotykanym protokołem komunikacyjnym komunikacji szeregowej jest Modbus RTU. Jednak czasem spotykamy urządzenia, które tego standardu komunikacyjnego nie obsługują i zmuszeni jesteśmy dostosować się do protokołu komunikacyjnego danego producenta. Przykładami takich urządzeń są falowniki, centrale alarmowe, stacje pogodowe. Musimy zgodnie z dokumentacją napisać własną warstwę aplikacyjną. Do tego służy biblioteka Serial Communication.
<br>
Biblioteka umożliwia przesyłanie pojedynczych bajtów, ciągów znaków (zmiennych STRING) i dowolnych danych (przekazywanych poprzez wskaźniki).
W przykładzie, prezentowanym w niniejszej dokumentacji, będzie modelowana komunikacja typu master - slave. Pokazane będę dwa sposoby wymiany danych – poprzez zmienne typu STRING i poprzez ciągi danych (umieszczone w tablicach zmiennych typu BYTE).
<br>
Dokumentacja ta nie zastępuje informacji zawartych w dokumentacji do urządzeń i biblioteki, stanowi tylko krótki opis przykładów. Przykłady i opis zostały stworzone w oparciu TwinCAT 3.
<br>
Spis użytych urządzeń:
- komputer z zainstalowanym oprogramowaniem TwinCAT 3 w wersji 4024.7 i środowiskiem TwinCAT XAE Shell oraz zainstalowaną biblioteką TF6340 Serial Communication
- sterownik CX9020 z opcją N031 (port RS485) oraz systemem operacyjnym Windows Embedded Compact 7 oraz TwinCAT 3 w wersji 4024.7

# Wymagania programowe
Potrzebne nam będzie oprogramowanie TwinCAT 3 oraz biblioteka TF6340 Serial Communication
Bibliotekę dodajemy w zakładce References, klikając PPM i wybierając Add library:

![sm1](https://ba-pl.github.io/wiki/assets/images/sm1.png "sm1")

a następnie wybierając (poprzez dwukrotne kliknięcie LPM) z listy żądaną bibliotekę (w tym przypadku jest to biblioteka Tc2_SerialCom):

![sm2](https://ba-pl.github.io/wiki/assets/images/sm2.png "sm2")

# Program PLC
## SerialLineControl
Ten blok funkcyjny jest częścią wspólną każdej aplikacji. SerialLineControl służy przesyłaniu danych z programu PLC do fizycznego buforu portu COM. Dzięki takiemu podejściu bloki wysyłania i odbierania danych są takie same, niezależnie od tego jaki rodzaj portu COM jest fizycznie używany. Rodzaj portu określa zmienna Mode typu ComSerialLineMode_t. Najczęściej wybierane są wartości SERIALLINEMODE_KL6_22B_STANDARD (dla modułów w trybie 22B) lub SERIALLINEMODE_PC_COM_PORT (dla portów COM).

![sm3](https://ba-pl.github.io/wiki/assets/images/sm3.png "sm3")

Po wybraniu odpowiedniego typu portu, trzeba podłączyć zmienne zlinkowane z wejściami i wyjściami portu COM. Zmienne te podpinamy pod 3 wejścia: pComIn, pComOut, SizeComIn. pComIn i pComOut to pointery wskazujące na adres w pamięci, SizeComIn pokazuje jaki jest rozmiar tej zmiennej. Takie rozwiązanie pozwala na wykorzystanie jednego, uniwersalnego bloku funkcyjnego do zmiennych różnych rozmiarów. Zaletą takiego podejścia jest elastyczność. Jeśli w przyszłości będziemy chcieli wykorzystać nasz kod w innej aplikacji, w której jest port COM innego typu, to zmiany ograniczają się do minimum. Przykład przejścia z różnic między trybem 22B a PC COM:

![sm4](https://ba-pl.github.io/wiki/assets/images/sm4.png "sm4")

Zmienne RxBuffer i TxBuffer (obie typu ComBuffer) są to bufory danych, łączone z blokami funkcyjnymi komend wysyłania i odbierania.
Dodatkowe informacje o tym bloku znajdują się w dalszej części dokumentacji.

## Wymiana danych – rola urządzenia Master i Slave
Program szablonowy realizuje komunikację RS typu pytanie – odpowiedź. Master na rozkaz wysyła zapytanie i oczekuje na odpowiedź. Odpowiedź jest analizowana i wracamy do początku. Slave czeka na zapytanie, gdy je otrzyma rozpoczyna analizę i wysyła odpowiednią odpowiedź. Po wszystkim jest powrót do oczekiwania. W przypadku wystąpienia błędu jest on zliczony i zapamiętany. Po określonym czasie następuje restart cyklu i powrót do kroku początkowego.
<br>
Kod szablonowych programów napisany jest w języku Structured Text (ST), dla zwiększenia czytelności kroki zostały umieszczone w instrukcji CASE. Blok odbierania danych (ReceiveString lub ReceiveData) znajduje się poza instrukcją CASE, ponieważ odbiór musi być aktywny cały czas. Blok wysyłania danych (SendString lub SendData) wykonywany jest jedynie w kroku, w którym dane są wysyłane. Kody podprogramów wymiany danych i zmiennych typu STRING są do siebie podobne.
<br>
Komunikację za pomocą zmiennych typu STRING wykorzystuje się np. do komunikacji z modemami GSM, gdzie do i z urządzenia wysyłane są komendy AT (Hayes AT).
<br>
Komunikację za pomocą dowolnego typu danych wykorzystuje się najczęściej do stworzenia komunikacji z falownikiem czy stacją pogodową, gdzie producent opisuje poszczególne bajty ramki komunikacyjnej pytania i odpowiedzi, np.:

![sm5](https://ba-pl.github.io/wiki/assets/images/sm5.png "sm5")

## Strona Master 
CASE iState OF
<br>
0: Oczekujemy na sygnał do wysłania komendy – zmienna bRead. Gdy go otrzymamy to czyszczona jest zmienna odczytana InData i wstawiana jest wartość do zmiennej wysyłanej OutData. W przykładzie zmienna bRead przyjmuje wartość TRUE co 1s, ponieważ podpięta jest do wyjścia timera fbTimer (linia 15). Można to oczywiście zmienić.
<br>
10: Wysłanie ramki – tu nie należy nic modyfikować. Jeśli wszystko zakończy się sukcesem, to idziemy do kroku 20. Gdyby operacja zakończyła się niepowodzeniem, to idziemy do kroku 10000.
<br>
20: W tym kroku czekamy na odpowiedź urządzenia Slave. Odczytane dane przepisujemy do zmiennej InData. Gdyby operacja zakończyła się niepowodzeniem, to idziemy do kroku 10000.
<br>
30: Jest to krok analizy danych – należy zmodyfikować go wedle uznania.
<br>
10000: Krok obsługi błędu. Tu również można dokonać modyfikacji. W przykładzie po 5s jest wykonywany reset – zmienne wejścia, wyjścia i kod błędu są zerowane, wracamy do kroku pierwszego.

## Strona Slave
**Uwaga!** W typowej aplikacji odczytującej dane strona slave jest już w urządzeniu wykonana, do nas należy przygotowanie odpowiedniego zapytania ze strony Master. Przykład nasz pokazuje przykładową implementację obu stron.
<br>
CASE iSate OF:
<br>
0: Nasłuch – oczekujemy na dane. Gdy go otrzymamy to wpisujemy dane do zmiennej InData, dane wyjściowe są czyszczone i przechodzimy do kroku 10. Gdyby operacja zakończyła się niepowodzeniem, to idziemy do kroku 10000.
<br>
10: Dekodowanie danych – tu sprawdzamy czy zapytanie było kierowane do nas, czy zgadza się suma kontrolna CRC, jaki rodzaj zapytania dostaliśmy itp. w zależności od tego jakie zapytanie otrzymaliśmy szykujemy odpowiednią odpowiedź. Jeśli zapytanie nie było kierowane do nas, to wracamy do nasłuchu. Tą część należy zmodyfikować zgodnie z aplikacją.
<br>
20: Wysłanie wcześniej przygotowanej ramki – tu nie należy nic modyfikować. Jeśli wszystko zakończy się sukcesem, to idziemy do kroku 0 (do nasłuchu). Gdyby operacja zakończyła się niepowodzeniem, to idziemy do kroku 10000.
<br>
10000: Krok obsługi błędu. Tu również można dokonać modyfikacji. W przykładzie po 5s jest wykonywany reset – zmienne wejścia, wyjścia i kod błędu są zerowane, wracamy do kroku pierwszego.

# Konfiguracja
## Wbudowany port COM

Konfiguracja wbudowanego portu COM odbywa się ręcznie. W pierwszej kolejności dodajemy port COM. Robimy to klikając w drzewku projektu, prawym przyciskiem myszy, na zakładce I/O->Devices i wybieramy Add New Item:

![sm6](https://ba-pl.github.io/wiki/assets/images/sm6.png "sm6")

Następnie z listy, która się pojawi, rozwijamy zakładkę Miscellaneous i wybieramy Serial Communication Port.

![sm7](https://ba-pl.github.io/wiki/assets/images/sm7.png "sm7")

Następnie wybieramy z drzewka projektu dodany w ten sposób port COM i przeprowadzamy jego konfigurację. W zakładce Serial Port wybieramy, który fizyczny port chcemy używać:

![sm8](https://ba-pl.github.io/wiki/assets/images/sm8.png "sm8")

W zakładce Communication Properties ustawiamy parametry transmisji. Ważne jest zaznaczenia pola KL6xx Mode i ustawienie parametreów transmisji – w obu urządzeniach muszą być takie same:

![sm9](https://ba-pl.github.io/wiki/assets/images/sm9.png "sm9")

## Moduł magistrali K-Bus
W przypadku komunikacji z wykorzystaniem modułu komunikacji szeregowej magistrali K-Bus (w przykładzie jest to moduł KL6021) najpierw należy wyszukać moduł na magistrali.

![sm10](https://ba-pl.github.io/wiki/assets/images/sm10.png "sm10")

Następnie najłatwiej parametryzację wykonać w programie KS2000 (jedna zakładka konfiguruje moduł, druga parametry transmisji):

![sm11](https://ba-pl.github.io/wiki/assets/images/sm11.png "sm11")


![sm12](https://ba-pl.github.io/wiki/assets/images/sm12.png "sm12")

Informacje na temat programu KS2000 można znaleźć w dokumentacji pod adresem https://infosys.beckhoff.com/content/1033/kl304x_kl305x/1142398859.html
<br>
Opcjonalnie moduł taki można konfigurować za pomocą bloku funkcyjnego KL6configuration z biblioteki Tc2_SerialCom.

## Moduł magistrali E-Bus
W przypadku komunikacji z wykorzystaniem modułu komunikacji szeregowej magistrali E-Bus (w przykładzie jest to moduł (EL6021) najpierw należy wyszukać moduł na magistrali za pomocą programu TwinCAT System Manager.

![sm13](https://ba-pl.github.io/wiki/assets/images/sm13.png "sm13")

Następnie konfigurujemy go na zakładce CoE – Online:

![sm14](https://ba-pl.github.io/wiki/assets/images/sm14.png "sm14")

Powyższe parametry są wpisywane do modułu. Można skonfigurować system w ten sposób, że będą wgrywane zawsze przy starcie TwinCATa. Wystarczy odpowiednie parametry dodać na zakładce Startup i aktywować konfigurację. Upraszcza to znacznie procedurę wymiany modułu w przyszłości.

# Linkowanie zmiennych 
W przypadku każdego z czterech omówionych programów (dwa rodzaje master’a i dwa rodzaje slave’a) konieczne jest zlinkowanie zmiennych w konfiguracji sterownika. Robimy to w ten sam sposób dla każdego programu. W drzewku projektu wybieramy port szeregowy, który wcześniej skonfigurowaliśmy. Następnie klikamy zakładkę Inputs i zaznaczamy wszystkie zmienne (w przypadku portu PC COM oprócz ExtVoltageOk):

![sm15](https://ba-pl.github.io/wiki/assets/images/sm15.png "sm15")

Następnie, klikamy zaznaczenie prawym przyciskiem myszy i wybieramy opcję Change Multilink. W oknie, które się pojawi będziemy mieli do wyboru zmienną odpowiedniego typu:

![sm16](https://ba-pl.github.io/wiki/assets/images/sm16.png "sm16")

Podobnie robimy w przypadku danych wyjściowych. Klikamy zakładkę Outputs, tym razem zaznaczamy jednak wszystkie zmienne. Po kliknięciu Change Multilink pojawi się możliwość zlinkowania zmiennej. Na koniec trzeba aktywować konfigurację.

# Task 
W przypadku modułów pracujących w trybie 5 i 22 bajtowym może być konieczne wywołanie bloku
SerialLineControl w szybszym tasku. Jest to wyjaśnione na przykładzie, przedstawionym na poniższym rysunku.

![sm17](https://ba-pl.github.io/wiki/assets/images/sm17.png "sm17")

Załóżmy taką sytuację:
<br>
1. Task PLC ustawiony na sterowniku wynosi 10 ms.
<br>
2. Moduł komunikacyjny to KL6041, wewnętrzny bufor odczytu 1024 bajty, zapisu 128 bajtów.
<br>
3. Urządzenia slave wysyłają do modułu co 250 ms jednorazowo ramkę składającą się z 220 bajtów
danych.
<br>
4. Moduł jest ustawiony w trybie 22 B – oznacza to, że proces image wejść i wyjść wynoszą po 22 bajty –
tyle danych jest przesyłanych w jednym cyklu.
<br>
5. W jednym cyklu między modułem a programem PLC może być wymienionych 22 B danych – wynika to
z proces image modułu.
<br>
Prześledźmy proces odczytu danych. Wewnątrz modułu jest bufor odczytu do którego trafiają dane z
urządzenia slave. W przypadku KL6041 ma on pojemność 1024 bajtów. Nasze urządzenie slave „wrzuca” do niego
220 bajtów danych. Sterownik odczytuje z tego buforu 22 bajty. Po potwierdzeniu odczytu dane są zdejmowane z
buforu (zastosowana jest kolejka FIFO). Proces pobrania danych zajmuje trzy cykle PLC. W naszym przypadku task
PLC wynosi 10 ms, więc pobranie z potwierdzeniem 22 bajtów wynosi 30 ms. Zatem pobranie 220 bajtów zajmie
10x30ms = 300 ms. Dane z urządzenia slave są wysyłane co 250 ms, więc nie zdążymy oczyścić buforu przed
przyjęciem nowych danych. Na początku niż złego się nie stanie, ponieważ jest miejsce w buforze, a odczyt przez
PLC jest realizowany jako FIFO. Ale po pewnym czasie, bufor się przepełni i utracimy część danych. Nie możemy
zwiększyć buforu wewnątrz modułu, nie możemy zwiększyć też ilości danych przesyłanych między modułem a
programem PLC, jedyne co możemy zmienić to częstotliwość wykonywania programu PLC, czyli task. Jeśli ustawimy
go na 2 ms, to czas odczytu danych z bufora zostanie zmniejszony do 60 ms. Szybszy task musi mieć najwyższy
priorytet i wystarczy że wywołamy w nim blok SerialLineControl. Blok ten pobierze dane z modułu i umieści w swoim wewnętrznym buforze (zmienna ComBuffer, rozmiar 300 bajtów, jednakowy dla odbierania i wysyłania). 
Bufor ten jest współdzielony przez bloki funkcyjne ReceiveString (lub ReceiveData) i SendString (lub SendData), wywoływane w wolniejszym task’u.

# Przykład aplikacji 

Przykładową aplikację możesz pobrać tutaj:
<br>
<br>
[Download SerialComm Sample](link){: .btn .btn-red }


