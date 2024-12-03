---
title: IoT Communicator
parent: Protokoły komunikacyjne
nav_order: 2
layout: page
---

# Wprowadzenie 
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Wykorzystana konfiguracja

W celu opracowania instrukcji posłużono się następującą konfiguracją sprzętową:
- Sterownik C6015-0010 z systemem operacyjnym Windows 10 oraz TwinCAT 3.1.4024.53
- Telefon komórkowy z dostępem do Internetu oraz pobraną aplikacją TwinCAT IoT
- Komputer z uruchomionym brokerem MQTT

## Opis technologii 

Biblioteka Tc3_IotCommunicator pozwala na wymianę danych pomiędzy programem PLC a brokerem poprzez
protokół komunikacyjny MQTT. Rozwiązanie takie pozwala np. na zdalne odczytywanie zmiennych statusowych
procesu (subscribe mode) lub zadawanie parametrów (publish mode) bez konieczności fizycznego przebywania w
pobliżu sterownika czy komputera, z którego sterownik był programowany. W przykładzie przedstawiona zostanie
komunikacja dla dwóch pokojów, z czego jeden będzie zabezpieczony nazwą użytkownika i hasłem, a drugi nie.

![iot_comm1](https://ba-pl.github.io/wiki/assets/images/iot_comm1.png "iot_comm1")

# Przygotowanie sterownika oraz program PLC
## Uruchomienie brokera MQTT

W tej instrukcji instalacja brokera MQTT zostanie przedstawiona w sposób skrócony. W celu otrzymania
dokładnej instrukcji prosimy o kontakt poprzez skrzynkę mailową support@beckhoff.pl.
<br>
W pierwszej kolejności należy zainstalować broker MQTT https://mosquitto.org/download/ (w naszym
przypadku będzie to Eclipse Mosquitto), a następnie odblokować port firewall 1883 na urządzeniu serwera oraz
klienta.
<br>
Dla wersji 2.0.0 oraz nowszych, wymagana jest zmiana domyślnej konfiguracji, aby zezwolić na dostęp innych
urządzeń do brokera. W tym celu należy edytować plik konfiguracyjny, znajdujący się w folderze instalacyjnym, o
nazwie **mosquitto.conf**. Potrzebne parametry:
- listener 1883 0.0.0.0 – deklarujemy port oraz adres po których będzie przebiegała komunikacja (0.0.0.0
oznacza dostęp z dowolnego adresu)
- allow_anonymous true – zezwalamy na połączenia z innych urządzeń

![iot_comm2](https://ba-pl.github.io/wiki/assets/images/iot_comm2.png "iot_comm2")

Aby załadować nową konfigurację, należy zapisać edytowany plik oraz w linii poleceń cmd uruchomić broker:
<br>
**mosquitto -v -c mosquitto.config**

![iot_comm3](https://ba-pl.github.io/wiki/assets/images/iot_comm3.png "iot_comm3")

Należy pamiętać, aby broker był cały czas uruchomiony podczas komunikacji!
<br>
Pełna dokumentacja pliku konfiguracyjnego dostępna na stronie https://mosquitto.org/man/mosquitto-conf-5

## Przygotowanie sterownika

W pierwszej kolejności należy połączyć się ze sterownikiem, a następnie odblokować w firewall port 1883 dla
połączeń przychodzących i wychodzących. Dodatkowa instalacja biblioteki nie jest konieczna, ponieważ biblioteka
Tc3_IotCommunicator jest domyślnie zainstalowana w wersji TwinCAT 3.1.4022 oraz wyższych. W przypadku
uruchamiania brokera MQTT na urządzeniu posiadającym adres IP przydzielany przy pomocy serwera DHCP należy
się upewnić, czy urządzenie mobilne, sterownik i urządzenie na którym uruchomiony jest broker znajdują się w tej
samej podsieci.

## Przygotowanie struktury danych

Należy uruchomić środowisko TwinCAT XAE, a następnie dołączyć do programu PLC biblioteki
Tc3_IotCommunicator oraz Tc3_Module. W następnej kolejności należy utworzyć strukturę danych, która
przesyłana będzie przy pomocy protokołu MQTT, np. jak poniżej:

![iot_comm4](https://ba-pl.github.io/wiki/assets/images/iot_comm4.png "iot_comm4")

Atrybut iot.DisplayName odpowiada za wyświetlanie nazwy zmiennej w urządzeniu klienta, atrybut ReadOnly
pozwala na ustawienie braku możliwości zmiany wartości zmiennej, atrybut Unit określa jednostkę w jakiej
wyświetlana jest wartość, a atrybut MinValue i MaxValue pozwala określić zakres wyświetlania zmiennej w
urządzeniu klienta.

## Przygotowanie funkcji do komunikacji 

W celu zapewnienia komunikacji przy pomocy protokołu MQTT używa się bloku funkcyjnego
FB_IotCommunicator dostępnego w bibliotece Tc3_IotCommunicator. Blok ten posiada następujące wejścia:
- sHostName – adres IP lub Hostname brokera MQTT
- nPort – port wykorzystywany do komunikacji (w naszym przypadku port 1883)
- sClientID – opcjonalne wejście dla ustawienia unikalnej nazwy klienta
- sMainTopic – nazwa głównego tematu
- sDeviceName – nazwa pokoju
- sUser – nazwa użytkownika (gdy skonfigurowane w brokerze)
- sPassword – hasło (gdy skonfigurowane w brokerze)
- stTLS – struktura dla komunikacji zabezpieczanej przy pomocy TLS
- bRetain – zmienna dla ustalenia, czy broker ma przechowywać poprzednie wiadomości
- eQoS – zmienna dla „Quality of Service”
<br>
<br>
Wyjścia:
- bError – gdy wystąpi błąd
- hrErrorCode – kod błędu
- eConnectionState – stan komunikacji między klientem I brokerem
- bConnected – TRUE jeśli jest poprawna komunikacja między klientem i brokerem
- fbCommand – wyjscie do ewaluacji otrzymanych danych („komend”)
<br>
<br>
Oraz metody:
- Execute – wywoływana cyklicznie dla utrzymania komunikacji
- SendData – metoda do wysłania danych do brokera
- SendMessage – metoda do wysłania wiadomości do brokera

## Komunikacja bez podawania użytkownika i hasła

W celu zapewnienia podstawowej komunikacji (bez zabezpieczenia przy pomocy podawania nazwy
użytkownika i hasła), konfiguracja wygląda jak na obrazku poniżej (deklarujemy tylko 4 zmienne wejściowe):

![iot_comm5](https://ba-pl.github.io/wiki/assets/images/iot_comm5.png "iot_comm5")

W aplikacji mobilnej w zakładce Settings odznaczamy opcję „Authentication”:

![iot_comm6](https://ba-pl.github.io/wiki/assets/images/iot_comm6.png "iot_comm6")

Należy pamiętać, że broker MQTT również powinien być uruchomiony bez opcji zakładającej nazwę
użytkownika i hasło dla komunikacji.

## Komunikacja z podawaniem nazwy użytkownika i hasła 

W celu zapewnienia komunikacji bezpieczniejszej (zabezpieczonej nazwą użytkownika i hasłem) poza
zmiennymi zadeklarowanymi w przykładzie powyżej należy zadeklarować również w bloku zmienne sUser oraz
sPassword, jak na przykładzie poniżej (muszą być one zgodne z użytkownikiem i hasłem skonfigurowanymi dla
brokera):

![iot_comm7](https://ba-pl.github.io/wiki/assets/images/iot_comm7.png "iot_comm7")

W aplikacji mobilnej należy zaznaczyć opcję „Authentication”, a następnie wpisać nazwę użytkownika i hasło w
oknie, które się pojawi:

![iot_comm8](https://ba-pl.github.io/wiki/assets/images/iot_comm8.png "iot_comm8")

Należy pamiętać, że broker MQTT również powinien być uruchomiony z opcją zakładającą nazwę użytkownika
i hasło dla komunikacji.

## Przygotowanie struktury programu 

W celu zapewnienia cyklicznej komunikacji klienta z brokerem należy co cykl wywoływać metodę Execute dla
funkcji FB_IotCommunicator:

![iot_comm9](https://ba-pl.github.io/wiki/assets/images/iot_comm9.png "iot_comm9")

Przesyłanie danych realizowane będzie co 500 ms przy pomocy timera TON, w przypadku gdy wyjście
bConnected bloku będzie w stanie TRUE (komunikacja będzie poprawna):

![iot_comm10](https://ba-pl.github.io/wiki/assets/images/iot_comm10.png "iot_comm10")

Powyższy fragment kodu służy do odczytu informacji z programu PLC np. za pomocą urządzenia mobilnego. Aby
możliwe było nadpisywanie wartości zmiennych z poziomu urządzenia klienta należy zaimplementować fragment
kodu jak poniżej:

![iot_comm11](https://ba-pl.github.io/wiki/assets/images/iot_comm11.png "iot_comm11")

Kod ten pozwala na nadpisanie wartości zmiennej w przypadku gdy dostępna jest możliwość wysłania nowej
komendy do brokera (fbIoT.fbCommand.bAvailable). 

# Aplikacja mobilna

W celu odczytu i zapisywania danych w programie PLC z poziomu telefonu komórkowego należy pobrać i
zainstalować aplikację TwinCAT IoT (dostępna w sklepie Google Play oraz AppStore). Następnie w aplikacji należy
przejść do zakładki Settings, gdzie dokonujemy konfiguracji urządzenia klienta. W polu Broker Address wpisujemy
adres IP lub Hostname brokera, w polu Port wpisujemy port komunikacyjny, w polu Client ID ID klienta (o ile
zadeklarowane w programie PLC), w polu Topic nazwę tematu, a opcję Authentication zaznaczamy w zależności od
rodzaju komunikacji (opisane wcześniej). Pole Encryption pozwala na wybranie kodowania, o ile zostało
zaimplementowane w brokerze.

![iot_comm12](https://ba-pl.github.io/wiki/assets/images/iot_comm12.png "iot_comm12")

## Uruchomienie aplikacji mobilnej 

Po uruchomieniu programu PLC na sterowniku oraz aplikacji mobilnej otrzymujemy możliwość odczytywania i
zmiany wartości parametrów programu PLC przy pomocy aplikacji mobilnej, jak przedstawiono to na poniższych
obrazkach.

### Uruchomienie bez nazwy użytkownika i hasła 

W pierwszej kolejności uruchomiono komunikację niechronioną nazwą użytkownika i hasłem. Widok działającej
aplikacji przedstawiono na obrazkach poniżej:

![iot_comm13](https://ba-pl.github.io/wiki/assets/images/iot_comm13.png "iot_comm13")

Jak można zauważyć, oba pokoje są w trybie online, mimo że drugi z nich został zadeklarowany jako chroniony
nazwą użytkownika i hasłem. Spowodowane jest to uruchomieniem brokera bez opcji logowania użytkowników.

### Uruchomienie chronione nazwą użytkownika i hasłem 

Następnie uruchomiono komunikację wymagającą zalogowania się przy pomocy nazwy użytkownika i hasła.
Konfiguracja i widok działającej aplikacji przedstawiono na obrazkach poniżej:

![iot_comm14](https://ba-pl.github.io/wiki/assets/images/iot_comm14.png "iot_comm14")

Jak można zauważyć, w zakładce Devices widoczny jest tylko Room 2. Spowodowane jest to uruchomieniem
brokera z opcją logowania użytkowników, a niezaimplementowaniem tej opcji przy deklaracji bloku funkcyjnego do
komunikacji dla pokoju 1.

## Dodatkowe funkcje 

### Nadpisywanie wartości 

Opcja nadpisywania wartości dostępna jest po kliknięciu na daną wartość, a następnie pojawi się możliwość
wpisania żądanej wartości.

![iot_comm15](https://ba-pl.github.io/wiki/assets/images/iot_comm15.png "iot_comm15")

### Toggle booleans

Opcja „Toggle booleans” pozwala na zmianę wartości zmiennej bool na przeciwną od razu po kliknięciu na nią.
W przypadku odznaczenia opcji pojawia się okno wyboru wartości zmiennej.

![iot_comm16](https://ba-pl.github.io/wiki/assets/images/iot_comm16.png "iot_comm16")

### Live graph 

Opcja „Live graph” pozwala na monitorowanie wartości zmiennych na wykresie. Aby ją uruchomić, należy
kliknąć ikonę w prawym górnym rogu ekranu, a nastepnie wybrać zmienne które chcemy monitorować i w tym
samym miejscu uruchomić rysowanie wykresu.

![iot_comm17](https://ba-pl.github.io/wiki/assets/images/iot_comm17.png "iot_comm17")

# Przykładowa aplikacja

Przykładową aplikację możesz pobrać tutaj:
<br>
[IoT Sample](https://github.com/BA-PL/IoTCommunicator/archive/refs/heads/main.zip){: .btn .btn-red }



