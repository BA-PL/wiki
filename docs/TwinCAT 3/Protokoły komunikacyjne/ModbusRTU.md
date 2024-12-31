---
title: ModbusRTU
parent: Protokoły komunikacyjne
nav_order: 3
layout: page
---


# Modbus RTU
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Rozszerzenie TF6255 wbudowane w środowisko TC3 XAE pozwala na komunikację poprzez protokół Modbus
RTU. Każde urządzenie wykorzystujące system TC3 XAR oraz posiadające wyprowadzenie do magistrali RS232,
RS422 lub RS485 może zostać Masterem bądź też Slavem w sieci Modbusa RTU. Programista ma możliwość
dołączenia urządzenia z TC3 do magistrali komunikacyjnej poprzez następujące komponenty hardware’owe:
- Poprzez port szeregowy na maszynie PC, IPC lub też na sterowniku CX
- Terminale magistrali K-BUS z serii KL60xx
- Terminale magistrali E-BUS z serii EL60xx

![modbus1](https://ba-pl.github.io/wiki/assets/images/modbus1.png "modbus1")

Niezależnie od wybranej konfiguracji sprzętowej do komunikacji po protokole Modbus RTU, funkcjonalność
pozostaje identyczna.
<br>
W zależności od danego wyprowadzenia do komunikacji po protokole Modbus RTU możemy posiadać różne
rozmiary buforów danych ( domyślnie mogą to być 5 bajt, 22 bajt lub też 64 bajt), maksymalną szybkość transmisji
jak i inne dodatkowe cechy charakterystyczne dla magistrali RS232, RS422 lub też RS485.

# Biblioteka Tc2_ModbusRTU
Biblioteka TC2_Modbus_RTU jest kluczowym elementem konfiguracji komunikacji poprzez protokół Modbus
RTU. Samą bibliotekę można dodać do projektu PLC poprzez zakładkę References:

![modbus2](https://ba-pl.github.io/wiki/assets/images/modbus2.png "modbus2")

## Wybór bloku funkcyjnego do obsługi Modbus RTU 
Przeglądając zawartość biblioteki można zauważyć następujące bloki funkcyjne :

![modbus3](https://ba-pl.github.io/wiki/assets/images/modbus3.png "modbus3")

W zależności od wymaganego bufora danych oraz posiadanej konfiguracji sprzętowej należy wybrać
odpowiedni blok funkcyjny. Zasada wyboru boku zawsze przebiega w dowolnej kolejności przez następujące 2
etapy:
<br>
1. Czy wyprowadzenie do magistrali protokołu Modbus RTU zawiera 5 bajtów, 22 bajty czy 64 bajty bufora
danych?
2. Czy dany port ma pracować w trybie Master czy Slave ?

### Jak sprawdzić rozmiar bufora danych? 
Wchodząc na stronę beckhoff.com oraz wyszukując dany moduł do magistrali komunikacyjnej
RS232/422/485 możemy odczytać liczbę bajtów które możemy wykorzystać w TwinCAT 3 do buforowania
danych. Na jej podstawie będziemy wybierali blok funkcyjny który zostanie wykorzystany w programie PLC.

![modbus4](https://ba-pl.github.io/wiki/assets/images/modbus4.png "modbus4")

Rozmiar proces data możemy również odczytać bezpośrednio z modułu w konfiguracji I/O projektu PLC.

![modbus5](https://ba-pl.github.io/wiki/assets/images/modbus5.png "modbus5")

Domyślnie wbudowane porty COM posiadają 64 bajty do komunikacji oraz wszystkie moduły z serii EL60xx
posiadają 22 Bajty.

## Opis bloku Modbus RTU Master
Niezależnie od wybranego bloku funkcyjnego funkcjonalność oraz wyprowadzenia pozostają identyczne.

![modbus6](https://ba-pl.github.io/wiki/assets/images/modbus6.png "modbus6")

Sama funkcjonalność bloków funkcyjnych ModbusRTUMaster opiera się na wywoływaniu
sparametryzowanych akcji pozwalających na nadawanie komend oraz analizowaniu informacji zwrotnej w
zależności od rodzaju komendy. Elementami wymagającymi parametryzacji są :

![modbus7](https://ba-pl.github.io/wiki/assets/images/modbus7.png "modbus7")

Niezależnie od rodzaju zastosowanego bloku funkcyjnego ModbusRTUMaster każdy blok posiada identycznie
wyzwalane akcje pozwalające na komunikację. Zostały one utworzone zgodnie z ideą podziału funkcji w
dokumentacji protokołu Modbus RTU. Podział Akcji wygląda następująco :

![modbus8](https://ba-pl.github.io/wiki/assets/images/modbus8.png "modbus8")

Przykładowe wywołanie bloku odczytu wejść

![modbus9](https://ba-pl.github.io/wiki/assets/images/modbus9.png "modbus9")

## Opis bloku Modbus RTU Slave

![modbus10](https://ba-pl.github.io/wiki/assets/images/modbus10.png "modbus10")

Bloki Funkcyjne ModbusRTUSlave_xxx pozwalają na utworzenie slave’a Modbusowego poprzez zmienne
wskazujące obszary wejść, wyjść jak i obszary pamięci. Jednocześnie na jednym wyprowadzeniu do magistrali RS –
owej może istnieć jeden slave Modbusowy. Blok Funkcyjny MobusRTUSlave_xxx w celu prawidłowego
funkcjonowania musi być wywoływany w każdym cyklu. Sam blok funkcyjny jest pasywny i nie zużywa czasu
procesora aż do momentu przyjścia komendy zewnętrznej. Blok funkcyjny posiada następujące wyprowadzenia:

![modbus11](https://ba-pl.github.io/wiki/assets/images/modbus11.png "modbus11")

Przykładowa implementacja

![modbus12](https://ba-pl.github.io/wiki/assets/images/modbus12.png "modbus12")

## Obszary Pamięci
W komunikacji po protokole Modbus RTU istotne jest właściwe adresowanie wejścia komunikacyjnego MBAdd
w celu poprawnego określenia adresu przeznaczenia używanego przez daną akcję odczytu/zapisu. Uwaga:
niezależnie od wybranego sposobu deklaracji zajmowanego obszaru pamięci, nie będzie to miało wpływu na
funkcjonowanie komunikacji przy wykorzystaniu Modbusa RTU.

### Adresacja obszaru wejść
Definicja wejść w TwinCAT 3 nieco różni się od standardowej definicji wejść. Programista ma możliwość
dokładnego określenia pozycji zadeklarowanej zmiennej w obszarze pamięci wejść:

![modbus13](https://ba-pl.github.io/wiki/assets/images/modbus13.png "modbus13")

Jak i również ma możliwość utworzenia zmiennej w pamięci stosując automatyczne przypisanie adresu w
obszarze pamięci:

![modbus14](https://ba-pl.github.io/wiki/assets/images/modbus14.png "modbus14")

Dostęp do odczytu wejść cyfrowych możemy użyć komendy .ReadInputStatus w bloczku mastera Modbusa
RTU. W przypadku chęci odczytu wartości wejściowych analogowych można zastosować komendę .ReadInputRegs.

![modbus15](https://ba-pl.github.io/wiki/assets/images/modbus15.png "modbus15")

Przykładowo w celu odczytu danego rejestru przy użyciu komendy .ReadInputRegs użyjemy adresowania po
zmiennych typu WORD używając odpowiednio dla każdego adresu 16#0, 16#1, 16#2 itd.
W przypadku chęci odczytu pojedynczych wejść cyfrowych będziemy używali adresowania bitowego w
przestrzeni wordów adresowanego odpowiednio: 16#00, 16#01, 16#02 … 16#0F, 16#10, 16#11 itd.

### Adresacja obszaru wyjść
Adekwatnie do deklaracji obszaru wejść, deklarowanie obszaru wyjść posiada identyczne możliwości co do
określenia docelowego obszaru pamięci wyjść. Pozwala na bezpośrednie przypisanie adresu wyjść do danej
zmiennej na lokalnym sterowniku :

![modbus16](https://ba-pl.github.io/wiki/assets/images/modbus16.png "modbus16")

Jak i programista ma możliwość użycia automatycznego przypisania obszaru pamięci

![modbus17](https://ba-pl.github.io/wiki/assets/images/modbus17.png "modbus17")

W przypadku chęci odczytu wyjść cyfrowych programista ma możliwość użycia akcji .ReadCoils lub też
.ReadHoldingRegisters. Programista ma również możliwość wysterowania wyjść poprzez akcje .WriteSingleCoil,
.WriteSingleRegister, .WriteMultipleCoils lub też .WriteMultipleRegisters. 

![modbus18](https://ba-pl.github.io/wiki/assets/images/modbus18.png "modbus18")

### Adresacja obszaru pamięci

Również w przypadku deklarowania obszaru pamięci programista ma możliwość wybrania docelowego obszaru
pamięci wewnętrznej poprzez użycie następującej deklaracji :

![modbus19](https://ba-pl.github.io/wiki/assets/images/modbus19.png "modbus19")

Lub też użycie automatycznego przypisania obszaru pamięci:

![modbus20](https://ba-pl.github.io/wiki/assets/images/modbus20.png "modbus20")

W celu odczytu danego obszaru pamięci można użyć akcji .ReadRegs. W celu zapisania obszarów pamięci
można użyć z kolei akcji .WriteSingleRegister lub .WriteRegs.

![modbus21](https://ba-pl.github.io/wiki/assets/images/modbus21.png "modbus21")

Obszar pamięci posiada stałe przesunięcie o wartości 16#4000.

# Konfiguracja sprzętowa
## COM Port
W przypadku jeżeli urządzenie posiada port COM który będziemy wykorzystywać ( np. CX9020 z opcją N031)
mamy możliwość użycia tego portu w programie PLC jako interfejs magistrali np. RS485. W celu konfiguracji danego
portu w pierwszej kolejności należy wyprać PPM na elemencie I/O / Devices i dodać element Serial Communication
Port.

![modbus22](https://ba-pl.github.io/wiki/assets/images/modbus22.png "modbus22")

Pierwszym elementem, na który warto zwrócić uwagę jest ustawienie portu komunikacyjnego jako COM 1. Jest
to domyślny adres pierwszego portu szeregowego dla każdego urządzenia z serii CX. Ustawienie można sprawdzić
poprzez zakładkę Serial Port w danym obiekcie COM Port.

![modbus23](https://ba-pl.github.io/wiki/assets/images/modbus23.png "modbus23")

Następnie główną konfigurację danego portu można przeprowadzić w zakładce Communication Properties. Ze
względu na użycie portu jako interfejsu komunikacyjnego do Modbusa należy zmienić tryb COM Port Mode na
KL6xx1 Mode. Poniżej została przedstawiona przykładowa konfiguracja na sterowniku CX9020 z portem RS485 dla
ramki Modbusa RTU o parametrach:

![modbus24](https://ba-pl.github.io/wiki/assets/images/modbus24.png "modbus24")

## Konfiguracja KL60XX na przykładzie KL6021 – 22 bajt
### Konfiguracja za pomocą programu KS2000
Przed przystąpieniem do konfiguracji modułu KL60xx należy mieć zainstalowany dodatkowo płatny program
KS2000. Pozwala on na konfigurację modułów K-Busowych. Następnie należy wykonać skanowanie istniejącego
modułu w następującej kolejności :
<br>
1. Zrestartować sterownik docelowy w tryb Config
2. Zeskanować konfigurację magistrali K-BUS bez włączania trybu Toggle Free Run State
3. Przejść do programu KS2000
4. Wybieramy sterownik docelowy z którym się łączymy

![modbus25](https://ba-pl.github.io/wiki/assets/images/modbus25.png "modbus25")

5. Logujemy się za pomocą programu KS2000 do sterownika

![modbus26](https://ba-pl.github.io/wiki/assets/images/modbus26.png "modbus26")

6. Wchodzimy w zakładkę Settings wybranego modułu komunikacyjnego

![modbus27](https://ba-pl.github.io/wiki/assets/images/modbus27.png "modbus27")

7. Konfigurujemy parametry komunikacji. Przykładowo:

![modbus28](https://ba-pl.github.io/wiki/assets/images/modbus28.png "modbus28")

8. Klikamy przycisk Apply
9. Zlinkować zmienne z I/O karty KL6021 adekwatnie według instrukcji opisanej w dalszej części 

### Konfiguracja poprzez blok funkcyjny KL6Configugation

![modbus29](https://ba-pl.github.io/wiki/assets/images/modbus29.png "modbus29")

Drugą możliwością konfiguracji karty jest użycie bloku funkcyjnego KL6Configuration. Jest to blok pozwalający
na określenie parametrów nastaw komunikacji wykorzystujących magistralę szeregową. W celu określenia który
port należy utworzyć zmienne będące buforem komunikacyjnym posiadający możliwość podlinkowana do
zmiennych komunikacyjnych karty KL6xxx. Zmiennymi buforowymi pozwalającymi na linkowanie są:

![modbus30](https://ba-pl.github.io/wiki/assets/images/modbus30.png "modbus30")

Poniżej zaprezentowano przykładową parametryzację:

![modbus31](https://ba-pl.github.io/wiki/assets/images/modbus31.png "modbus31")

Zmienne stKL6x22bCommBuff zostały odpowiednio zlinkowane zgodnie z opisem umieszczonym w dalszej części. 

## EL60xx 
W przypadku kart z serii EL60xx mamy do dyspozycji rozmiar bufora danych w wielkości 22 bajtów niezależnie
od wybranej karty jak i możliwość konfiguracji parametrów komunikacyjnych bez użycia oprogramowania KS2000.
Samą konfigurację karty możemy przeprowadzić poprzez konfigurację po protokole CAN over Ethercat.
Komunikacja CAN over Ethercat pozwala na wprowadzenie parametrów komunikacyjnych bezpośrednio w
przestrzeni rejestrów karty docelowej. Niezależnie od rozwiązania możemy konfigurować parametry na
przykładowe 2 sposoby:
- konfiguracja poprzez zakładkę CoE – Online
- konfiguracja poprzez zakładkę CoE – Startup

### Konfiguracja poprzez zakładkę CoE – Online

![modbus32](https://ba-pl.github.io/wiki/assets/images/modbus32.png "modbus32")

Część kart wymieniających informację poprzez magistralę E-BUS posiada możliwość konfiguracji
parametrów nastaw bezpośrednio na samej karcie, niezależnie od konfiguracji sprzętowej. Parametryzację samej
karty w trybie Online można wykonać poprzez zakładkę CoE-Online.
<br>
Unikatowe parametry konfiguracyjne charakterystyczne dla danej karty w większości przypadków znajdują
się pod adresem 8000.

![modbus33](https://ba-pl.github.io/wiki/assets/images/modbus33.png "modbus33")

Zmienne z flagą RW – Read/Write programista ma możliwość nadpisania i zmiany parametrów. Zmianę
danego parametru z listy można wykonać poprzez dwukrotne kliknięcie danego parametru LPM. W przypadku
zmiennych przyjmujących wartości nieokreślone żadnym standardem (np. Enable Half Duplex) otworzy się nam
okno posiadający standardowy szablon wprowadzenia nowej wartości. Nową wartość można wprowadzić w
dowolnym formacie numerycznych i zapisać poprzez kliknięcie przycisku OK.

![modbus34](https://ba-pl.github.io/wiki/assets/images/modbus34.png "modbus34")

W przypadku zmiennych posiadających jakieś bliżej określone standardowe wartości (np.Baudrate)
użytkownik ma możliwość wyboru danej wartości z rozwijanego paska wyboru, ułatwiając wprowadzanie.

![modbus35](https://ba-pl.github.io/wiki/assets/images/modbus35.png "modbus35")

### Konfiguracja poprzez zakładkę CoE – Startup
W przypadku konfiguracji poprzez zakładkę CoE – Online edytujemy bezpośrednio wartości na karcie docelowej
w trybie online. Wadą tego rozwiązania jest możliwość utracenia konfiguracji w przypadku uszkodzenia danej karty
podczas działania. W momencie wymiany karty poprzednia konfiguracja zostaje utracona.
<br>
Dlatego też w tym przykładzie zostanie pokazana konfiguracja poprzez zakładkę CoE – Startup . W przypadku
konfiguracji poprzez zakładkę CoE – Startup parametry konfiguracyjne zostają przepisane z pamięci sterownika i
wprowadzone na karcie docelowej w trakcie przejścia w tryb Run.

![modbus36](https://ba-pl.github.io/wiki/assets/images/modbus36.png "modbus36")

W celu wprowadzenia nowej konfiguracji danego parametru należy w zakładce Startup kliknąć przycisk New…

![modbus37](https://ba-pl.github.io/wiki/assets/images/modbus37.png "modbus37")

W nowo otwartym oknie rozwijamy zakładkę COM Settings z indeksem 8000. Zawarte są w niej wszystkie
ustawienia parametrów komunikacyjnych.

![modbus38](https://ba-pl.github.io/wiki/assets/images/modbus38.png "modbus38")

Przykładowo klikając dwukrotnie LPM na parametrze Baudrate jesteśmy w stanie edytować wartość prędkości
komunikacji poprzez rozsuwany pasek z enumeracyjnymi wartościami prędkości komunikacji. Następnie nowo
wybraną wartość prędkości komunikacji zatwierdzamy przyciskiem OK.

![modbus39](https://ba-pl.github.io/wiki/assets/images/modbus39.png "modbus39")

Po wprowadzeniu nowej wartości należy, zaznaczając zmieniony przez nas parametr, kliknąć przycisk OK.
Zmiana danego parametru zostanie wówczas zapisana w oknie Startup i zostanie wczytana na kartę w
momencie przejścia karty ze stanu Pre-OP do Safe-OP.

![modbus40](https://ba-pl.github.io/wiki/assets/images/modbus40.png "modbus40")

Programista ma również możliwość szybkiego przepisania wartości zmiennych z zakładki CoE – Online do
zakładki Startup poprzez wybranie zmienionego parametru i kliknięcie przycisku Add to startup…

![modbus41](https://ba-pl.github.io/wiki/assets/images/modbus41.png "modbus41")

Wybrana konfiguracja danej zmiennej, wraz z określoną jej wartoscią zostanie przeniesona na koniec Startup
listy.

![modbus42](https://ba-pl.github.io/wiki/assets/images/modbus42.png "modbus42")

Po takim skonfigurowaniu parametrów komunikacyjnych karty należy już tylko utworzyć połączenie pomiędzy
programem PLC a wyprowadzeniami utworzonymi w karcie EL60xx zgodnie z kolejnym rozdziałem.

## Przykładowe linkowanie zmiennych procesu komunikacyjnego 

![modbus43](https://ba-pl.github.io/wiki/assets/images/modbus43.png "modbus43")

Posiadając utworzony w programie blok ModbusRtuMaster_xxx lub ModbusRtuSlave_xxx mamy możliwość
podlinkowania Process Image z programu PLC do COM Port’u. Połączenie należy utworzyć w sposób następujący :
<br>
1. Połączenie elementu Input/Status do elementu InData/SerStatus

![modbus44](https://ba-pl.github.io/wiki/assets/images/modbus44.png "modbus44")

2. Połączenie elementów od Input/Data1 do Input/Data64 za pomocą skrótu Change multi-link do elementu
InData/D

![modbus45](https://ba-pl.github.io/wiki/assets/images/modbus45.png "modbus45")

3. Następnie należy przypisać element Outputs/Ctrl do OutData/SerCtrl

![modbus46](https://ba-pl.github.io/wiki/assets/images/modbus46.png "modbus46")

4. Połączenie zmiennych od Outputs/Data1 do Outputs/Data64 poprzez Change multi-link do OutData/D

![modbus47](https://ba-pl.github.io/wiki/assets/images/modbus47.png "modbus47")

Po utworzeniu połączeń, a następnie po prze aktywowaniu nowej konfiguracji strona hardware’owa jest
przygotowana do działania. W przypadku jeżeli w projekcie PLC jest utworzona zmienna typu
ModbusRtuMaster_PcCom lub ModbusRtuSlave_PcCom, lecz nie widać jej w elemencie do podlinkowania należy
przebudować program PLC. Niezależnie czy wykorzystujemy karty z serii KL6xxx czy karty EL6xxx linkowanie
zmiennych komunikacyjnych wygląda identycznie.

# Tips & Tricks 
## Wyprowadzenia portu DB9 i przykład połączenia
W zależności od sterownika możemy posiadać różne konfiguracje wyprowadzeń na porcie DB9. W przypadku
sterowników serii CX9020 z rozszerzeniem na port COM wyprowadzenie na RS485 prezentuje się następująco

![modbus81](https://ba-pl.github.io/wiki/assets/images/modbus81.png "modbus81")

W przypadku sterowników CX8180 oraz CX7080 wyprowadzenie DB9 jest przygotowane do konfiguracji
magistrali RS232 lub RS485 w Half-Duplex’ie :

![modbus82](https://ba-pl.github.io/wiki/assets/images/modbus82.png "modbus82")

![modbus83](https://ba-pl.github.io/wiki/assets/images/modbus83.png "modbus83")

# Przykład aplikacji 

Przykładową aplikację możesz pobrać tutaj:
<br>
<br>
[Download Modbus Sample](https://github.com/BA-PL/Tx6255-Modbus-RTU/archive/refs/heads/main.zip){: .btn .btn-red }


