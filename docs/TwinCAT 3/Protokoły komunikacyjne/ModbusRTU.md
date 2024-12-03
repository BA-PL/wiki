---
title: ModbusRTU
parent: Protokoły komunikacyjne
nav_order: 3
layout: page
---


# Modbus RTU
{: .no_toc }

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


![modbus22](https://ba-pl.github.io/wiki/assets/images/modbus22.png "modbus22")
![modbus23](https://ba-pl.github.io/wiki/assets/images/modbus23.png "modbus23")
![modbus24](https://ba-pl.github.io/wiki/assets/images/modbus24.png "modbus24")
![modbus25](https://ba-pl.github.io/wiki/assets/images/modbus25.png "modbus25")
![modbus26](https://ba-pl.github.io/wiki/assets/images/modbus26.png "modbus26")
![modbus27](https://ba-pl.github.io/wiki/assets/images/modbus27.png "modbus27")
![modbus28](https://ba-pl.github.io/wiki/assets/images/modbus28.png "modbus28")
![modbus29](https://ba-pl.github.io/wiki/assets/images/modbus29.png "modbus29")
![modbus30](https://ba-pl.github.io/wiki/assets/images/modbus30.png "modbus30")
![modbus31](https://ba-pl.github.io/wiki/assets/images/modbus31.png "modbus31")
![modbus32](https://ba-pl.github.io/wiki/assets/images/modbus32.png "modbus32")
![modbus33](https://ba-pl.github.io/wiki/assets/images/modbus33.png "modbus33")
![modbus34](https://ba-pl.github.io/wiki/assets/images/modbus34.png "modbus34")
![modbus35](https://ba-pl.github.io/wiki/assets/images/modbus35.png "modbus35")
![modbus36](https://ba-pl.github.io/wiki/assets/images/modbus36.png "modbus36")
![modbus37](https://ba-pl.github.io/wiki/assets/images/modbus37.png "modbus37")
![modbus38](https://ba-pl.github.io/wiki/assets/images/modbus38.png "modbus38")
![modbus39](https://ba-pl.github.io/wiki/assets/images/modbus39.png "modbus39")
![modbus40](https://ba-pl.github.io/wiki/assets/images/modbus40.png "modbus40")
![modbus41](https://ba-pl.github.io/wiki/assets/images/modbus41.png "modbus41")
![modbus42](https://ba-pl.github.io/wiki/assets/images/modbus42.png "modbus42")
![modbus43](https://ba-pl.github.io/wiki/assets/images/modbus43.png "modbus43")
![modbus44](https://ba-pl.github.io/wiki/assets/images/modbus44.png "modbus44")
![modbus45](https://ba-pl.github.io/wiki/assets/images/modbus45.png "modbus45")
![modbus46](https://ba-pl.github.io/wiki/assets/images/modbus46.png "modbus46")
![modbus47](https://ba-pl.github.io/wiki/assets/images/modbus47.png "modbus47")
![modbus48](https://ba-pl.github.io/wiki/assets/images/modbus48.png "modbus48")
![modbus49](https://ba-pl.github.io/wiki/assets/images/modbus49.png "modbus49")
![modbus50](https://ba-pl.github.io/wiki/assets/images/modbus50.png "modbus50")
![modbus51](https://ba-pl.github.io/wiki/assets/images/modbus51.png "modbus51")
![modbus52](https://ba-pl.github.io/wiki/assets/images/modbus52.png "modbus52")
![modbus53](https://ba-pl.github.io/wiki/assets/images/modbus53.png "modbus53")
![modbus54](https://ba-pl.github.io/wiki/assets/images/modbus54.png "modbus54")
![modbus55](https://ba-pl.github.io/wiki/assets/images/modbus55.png "modbus55")
![modbus56](https://ba-pl.github.io/wiki/assets/images/modbus56.png "modbus56")
![modbus57](https://ba-pl.github.io/wiki/assets/images/modbus57.png "modbus57")
![modbus58](https://ba-pl.github.io/wiki/assets/images/modbus58.png "modbus58")
![modbus59](https://ba-pl.github.io/wiki/assets/images/modbus59.png "modbus59")
![modbus60](https://ba-pl.github.io/wiki/assets/images/modbus60.png "modbus60")
![modbus61](https://ba-pl.github.io/wiki/assets/images/modbus61.png "modbus61")
![modbus62](https://ba-pl.github.io/wiki/assets/images/modbus62.png "modbus62")
![modbus63](https://ba-pl.github.io/wiki/assets/images/modbus63.png "modbus63")
![modbus64](https://ba-pl.github.io/wiki/assets/images/modbus64.png "modbus64")
![modbus65](https://ba-pl.github.io/wiki/assets/images/modbus65.png "modbus65")
![modbus66](https://ba-pl.github.io/wiki/assets/images/modbus66.png "modbus66")
![modbus67](https://ba-pl.github.io/wiki/assets/images/modbus67.png "modbus67")
![modbus68](https://ba-pl.github.io/wiki/assets/images/modbus68.png "modbus68")
![modbus69](https://ba-pl.github.io/wiki/assets/images/modbus69.png "modbus69")
![modbus70](https://ba-pl.github.io/wiki/assets/images/modbus70.png "modbus70")
![modbus71](https://ba-pl.github.io/wiki/assets/images/modbus71.png "modbus71")
![modbus72](https://ba-pl.github.io/wiki/assets/images/modbus72.png "modbus72")
![modbus73](https://ba-pl.github.io/wiki/assets/images/modbus73.png "modbus73")
![modbus74](https://ba-pl.github.io/wiki/assets/images/modbus74.png "modbus74")
![modbus75](https://ba-pl.github.io/wiki/assets/images/modbus75.png "modbus75")
![modbus76](https://ba-pl.github.io/wiki/assets/images/modbus76.png "modbus76")
![modbus77](https://ba-pl.github.io/wiki/assets/images/modbus77.png "modbus77")
![modbus78](https://ba-pl.github.io/wiki/assets/images/modbus78.png "modbus78")
![modbus79](https://ba-pl.github.io/wiki/assets/images/modbus79.png "modbus79")
![modbus80](https://ba-pl.github.io/wiki/assets/images/modbus80.png "modbus80")
![modbus81](https://ba-pl.github.io/wiki/assets/images/modbus81.png "modbus81")
![modbus82](https://ba-pl.github.io/wiki/assets/images/modbus82.png "modbus82")
![modbus83](https://ba-pl.github.io/wiki/assets/images/modbus83.png "modbus83")

# Przykład aplikacji 

Przykładową aplikację możesz pobrać tutaj:
<br>
<br>
[Download ADS Sample](https://github.com/BA-PL/ADS/archive/refs/heads/main.zip){: .btn .btn-red }


