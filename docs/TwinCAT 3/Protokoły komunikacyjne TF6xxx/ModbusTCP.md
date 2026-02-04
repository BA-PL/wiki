---
title: ModbusTCP
parent: Protokoły komunikacyjne TF6xxx
nav_order: 4
layout: page
---

# Modbus TCP
{: .no_toc }
<h6> Data modyfikacji: 27.10.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}


# Wprowadzenie

## Informacje podstawowe

![mtcp1](https://ba-pl.github.io/wiki/assets/images/ModbusTCP/mtcp1.png "mtcp1")

Protokół Modbus TCP (licencja TF6250) jest oparty o warstwę TCP/IP, może więc zostać uruchomiony na dowolnym porcie Ethernet zawartym w sterowniku (od Q1 2026 możliwość uruchomienia na dedykowanym module EL6251). Pozwala on na komunikację typu serwer/klient z wieloma urządzeniami z branży automatyki, które udostępniają swoje dane procesowe. Jest protokołem uniwersalnym, wspieranym przez wielu producentów. 
W Beckhoff można go uruchomić na wszystkich sterownikach (już od Performance Level 10 - CX7xxx).

![mtcp2](https://ba-pl.github.io/wiki/assets/images/ModbusTCP/mtcp2.png "mtcp2")

# Instalacja

## Dla Windows CE

Instalacja następuje poprzez instalację dodatku na komputerze inżynierskim (opisane niżej) i przeniesienie pliku CAB na sterownik, co opisane jest [w Infosys](https://infosys.beckhoff.com/content/1033/tf6250_tc3_modbus_tcp/705884939.html).

## Dla TwinCAT/BSD

Instalacja odbywa się poprzez command line z poziomu sterownika. Opisane jest to [w Infosys](https://infosys.beckhoff.com/content/1033/tf6250_tc3_modbus_tcp/11519180811.html)

## Dla pełnych systemów operacyjnych

Procedura instalacji dodatków opisana jest oddzielnie [dla wersji 4026](https://ba-pl.github.io/wiki/docs/TwinCAT%203/Instalacja/Instalacja/#instalacja-twincat-i-funkcji) oraz [dla wersji 4024](https://ba-pl.github.io/wiki/docs/TwinCAT%203/Instalacja/Instalacja%204024/#instalacja-bibliotek-oraz-dodatkowych-narz%C4%99dzi).
Należy postępować zgodnie z instrukcją, instalując dodatek TF6250 Modbus TCP.

# Konfiguracja

## Odblokowanie portu w Firewall

Modbus TCP domyślnie komunikuje się po porcie TCP 502. Należy więc pamiętać o odblokowaniu odpowiedniego portu w firewall **na sterowniku**.

* Dla Windows CE wykonuje się to w Control Panel -> CX Configuration -> Firewall:

![mtcp3](https://ba-pl.github.io/wiki/assets/images/ModbusTCP/mtcp3.png "mtcp3")

* Dla pełnych systemów Windows wykonuje się to w ustawieniach Firewall z poziomu systemu operacyjnego:

![mtcp4](https://ba-pl.github.io/wiki/assets/images/ModbusTCP/mtcp4.png "mtcp4")

* Dla TwinCAT/BSD odblokowanie portu w Firewall wykonuje się z poziomu Command Line. Procedura opisana jest [w Infosys](https://infosys.beckhoff.com/content/1033/twincat_bsd/6424551179.html)

# Funkcjonalność serwera

Dla funkcjonalności Modbus Server należy zainstalować serwer Mobus TCP (opisane wyżej) oraz zadeklarować odpowiednie zmienne w PLC. Ważne jest, aby deklaracja była dokładnie taka, jak opisana [w Infosys](https://infosys.beckhoff.com/content/1033/tf6250_tc3_modbus_tcp/192743435.html) - z dokładnością co do nazwy listy zmiennych globalnych, na których znajdują się odpowiednie tablice - inaczej funkcjonalność serwera nie będzie działać poprawnie.

![gvl](https://ba-pl.github.io/wiki/assets/images/ModbusTCP/gvl.png "gvl")

Wówczas odpowiednio zadekladrowane tablice będą odpowiadać poniższym obszarom (warto zwrócić uwagę na PLC memory area, czyli deklarację %M, która mapowana jest domyślnie na adresie 16#3000):

![mtcp6](https://ba-pl.github.io/wiki/assets/images/ModbusTCP/mtcp6.png "mtcp6")

# Funkcjonalność client

W celu uruchomienia funkcjonalności Modbus Client, podobnie jak w przypadku funkcjonalności Server, musimy zainstalować na sterowniku dodatek TF6250, natomiast w środowisku inżynierskim należy dodać bibliotekę Tc2_ModbusSrv. Pozwala ona na wywołanie wszystkich funkcji Modbusowych (lista poniżej):

![mtcp7](https://ba-pl.github.io/wiki/assets/images/ModbusTCP/mtcp7.png "mtcp7")

## Bloki do odczytu / zapisu danych

Standardowy blok do odczytu / zapisu danych w Modbus TCP posiada następujące wejścia / wyjścia:

![mtcp8](https://ba-pl.github.io/wiki/assets/images/ModbusTCP/mtcp8.png "mtcp8")

### Wejścia

* _sIPAddr_ - Adres IP urządzenia, z którym następuje komunikacja (a więc urządzenia spełniającego funkcjonalność Server w konfiguracji)
* _nTCPPort_ - port TCP, po którym następuje komunikacja (najczęściej 502)
* _nUnitID_ - dla urządzeń posiadajacych wiele serwerów na jednym adresie IP - jeżeli jest to urządzenie bez takiej funkcjonalności należy wpisać ID 255
* _nQuantity_ - dla bloków odczytu oraz bloków zapisu wielu cewek / rejestrów - jest to ilość cewek / rejestrów które chcemy odczytać / zapisać
* _nMBAddr_ - adres Modbusowy - czyli adres rejestru, na którym znajduje się wartość, którą chcemy odczytać / zapisać
* _cbLength_ - wielkość odczytu / zapisu **w bajtach**, dla rejestrów jest to najczęściej nQuantity * 2, dla cewek jest to najczęściej nQuantity / 8 (rejestry są dwubajtowe, cewki jednobitowe)
* _pDestAddr_ / _pSrcAddr_ - adres w pamięci, do którego zostaną zapisane / z którego zostaną pobrane wartości do procedury odczytu / zapisu poprzez Modbus TCP. Najczęściej odnosimy się do niego poprzez funkcję ADR dla zmiennej przeznaczonej dla danych
* _bExecute_ - uruchomienie procedury odczytu / zapisu. Należy pamiętać, że bloki działają asynchronicznie, a więc każda procedura musi zostać rozpoczęta podaniem zbocza narastającego na wejście bExecute
* _tTimeout_ - maksymalny czas, po jakim brak odpowiedzi ze strony urządzenia odpytywanego zostanie uznany za błąd

### Wyjścia

* _bBusy_ - zmienna informująca o pracy bloku - uruchamia się w momencie podania stanu wysokiego na Execute, opada w momencie zakończenia odczytu (z powodzeniem lub bez)
* _bError_ - zmienna informująca, że procedura zakończyła się niepowodzeniem
* _nErrId_ - w przypadku zmiennej bError w stanie wysokim informuje o typie błędu, jaki wystąpił. Błędy komunikacyjne opisane są w odpowiednim rozdziale [w Infosys](https://infosys.beckhoff.com/content/1033/tf6250_tc3_modbus_tcp/374277003.html)
* _cbRead_ - w przypadku bloków odczytu podaje ilość odczytanych bajtów danych (w przypadku odczytu zakończonego sukcesem)

# Najczęstsze przyczyny braku komunikacji Modbus TCP 

Poniżej podane są najczęściej spotykane problemy przy konfiguracji / uruchomieniu funkcjonalności Modbus TCP. Warto zapoznać się z tym rozdziałem, aby ich uniknąć.

* Zły adres w pamięci – należy zwrócić uwagę w dokumentacji czy adres zapisany jest w postaci dziesiętnej, czy szesnastkowej.

* Brak odblokowania portu w firewall – należy zwrócić uwagę, aby port (najczęściej będzie to port TCP 502) został odblokowany w sterowniku dla komunikacji w obu kierunkach.

* Brak zainstalowania dodatku TF6250 Modbus TCP Server lub jego niewłaściwa instalacja – należy pamiętać o software resecie sterownika (restart z poziomu systemu operacyjnego) w celu poprawnego zakończenia procesu instalacji, a instalacji dokonywać należy w domyślnej lokalizacji.

* Użycie niewłaściwej funkcji – najczęściej w dokumentacji urządzenia określona jest funkcja, którą należy odpytywać konkretne rejestry – nazewnictwo rejestrów nie zawsze jest spójne z funkcją!

# Przykład aplikacji 

Przykładową aplikację możesz pobrać tutaj:
<br>
<br>
[Download Modbus Sample](https://github.com/BA-PL/TF6250-Modbus-TCP-TC3/archive/refs/heads/main.zip){: .btn .btn-red }