---
title: - Tryb symulacji
nav_order: 1
layout: page
parent: TwinCAT 3
---

# Tryb symulacji - poradnik
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Dokumentacja przedstawia szereg ustawień, które mogą być wymagane do uruchomienia lokalnej symulacji aplikacji PLC (bez komputera Beckhoff).

# Przygotowanie komputera 
W celu uruchomienia TwinCAT w symulacji, niezbędne są dwa kroki:
- sprawdzenie, czy na komputerze włączona jest obsługa wirtualizacji (ustawienie w BIOS)

![loc1](https://ba-pl.github.io/wiki/assets/images/loc1.png "loc1")

- uruchomienie pliku **win8settick.bat** z prawami Administratora, a następnie restart komputera
	- dla TwinCAT w wersji 3.1.4024.x i niższych, plik znajduje się w lokalizacji *C:\TwinCAT\3.1\System*
	- dla TwinCAT w wersji 3.1.4026 plik znajduje się w lokalizacji *C:\Program Files (x86)\Beckhoff\TwinCAT\3.1\System*

![loc2](https://ba-pl.github.io/wiki/assets/images/loc2.png "loc2")

# Przygotowanie projektu 
Jeżeli posiadamy gotowy projekt, który chcielibyśmy uruchomić w trybie symulacji, to w pierwszej kolejności należy:
- jako urządzenie docelowe wybrać *<Local>* czyli lokalny komputer

![loc3](https://ba-pl.github.io/wiki/assets/images/loc3.png "loc3")

- jeżeli w konfiguracji znajdują się urządzenia, należy je czasowo wyłączyć w projekcie (nie symulujemy hardware’u). Można to zrobić klikając na odpowiednie urządzenie główne PPM i wybierając opcję *Disable* (nie powoduje to żadnych zmian w konfiguracji oprócz czasowej dezaktywacji urządzeń, tj. ewentualne linkowanie zmiennych nie zostanie utracone)

![loc4](https://ba-pl.github.io/wiki/assets/images/loc4.png "loc4")

- dodatkowo, należy również czasowo zablokować w kodzie PLC elementy bezpośrednio odwołujące się do sprzętu, jak np. blok do obsługi 1s UPS

![loc5](https://ba-pl.github.io/wiki/assets/images/loc5.png "loc5")

- jeśli w projekcie do licencji używany jest dongle, którego aktualnie nie mamy, należy również go zablokować

![loc6](https://ba-pl.github.io/wiki/assets/images/loc6.png "loc6")

## Uruchomienie projektu
Po wykonaniu czynności z poprzednich rozdziałów, można wykonać próbę uruchomienia projektu:
- aktywujemy konfigurację 

![loc7](https://ba-pl.github.io/wiki/assets/images/loc7.png "loc7")

- opcję *Autostart PLC Boot Project* przy pierwszym uruchomieniu lepiej zostawić odznaczoną, potwierdzamy ten komunikat i kolejny o restarcie TwinCAT do trybu Run

![loc8](https://ba-pl.github.io/wiki/assets/images/loc8.png "loc8")

- jeżeli powyższe kroki się powiodą, uruchamiany projekt PLC

![loc9](https://ba-pl.github.io/wiki/assets/images/loc9.png "loc9")

## Możliwe problemy 
### Izolacja rdzenia
Podczas aktywacji konfiguracji może pojawić się komunikat:

![loc10](https://ba-pl.github.io/wiki/assets/images/loc10.png "loc10")

W takim wypadku przed uruchomieniem dobrze jest wykonać izolację procesora logicznego na komputerze (dobrze jest izolować ich parzystą liczbę) i uruchamiać TwinCAT właśnie na tych wyizolowanych rdzeniach. 
<br>
W projekcie należy odnaleźć ustawienia **Real-Time** i w zakładce **Settings** kliknąć na przycisk **Read from Target** (wyświetlone dane będą różnić się w zależności od komputera).

![loc11](https://ba-pl.github.io/wiki/assets/images/loc11.png "loc11")

Następnie w tej samej zakładce wybieramy przycisk **Set on Target**. W oknie **Change numbers of Windows CPU** edytujemy ustawienia w ten sposób, aby w polu Isolated znalazła się wartość Po zmianie wybieramy **Set** i potwierdzamy pojawiające się komunikaty. Zmiana tych ustawień wymaga restartu komputera.

![loc12](https://ba-pl.github.io/wiki/assets/images/loc12.png "loc12")

Po restarcie komputera należy wrócić do zakładki **Real-Time -> Settings**. Na liście dostępnych wątków, niektóre pojawią się z atrybutem Isolated. Należy przy nim zaznaczyć pole w kolumnie RT-CPU a wcześniej wybraną opcję odznaczyć. Aby zmiany zostały wprowadzone należy aktywować konfigurację.

### HyperV
Podczas aktywacji konfiguracji pojawia się błąd zawierający informację o HyperV:

![loc13](https://ba-pl.github.io/wiki/assets/images/loc13.png "loc13")

W takiej sytuacji należy wyłączyć HyperV w funkcjach systemu. Można to zrobić z poziomu panelu sterownia:

![loc14](https://ba-pl.github.io/wiki/assets/images/loc14.png "loc14")

- odznaczamy całą sekcję dotyczącą HyperV

![loc15](https://ba-pl.github.io/wiki/assets/images/loc15.png "loc15")

- jeśli jest zaznaczona opcja Virtual Machine Platform, to również odznaczamy

![loc16](https://ba-pl.github.io/wiki/assets/images/loc16.png "loc16")

- jeśli powyższe kroki zostały wykonane a błąd nadal się pojawia, może być konieczne wyłączenie uruchamiania Hypervisora. Robi się to komendą z poziomu cmd uruchomionego z prawami administratora, a następnie zrestartować system

bcdedit /set hypervisorlaunchtype off

(aby ponownie włączyć tę opcję, należy użyć komendy bcdedit /set hypervisorlaunchtype auto)

### Virtualization based security

Zabezpieczenia oparte na wirtualizacji wykorzystują funkcje wirtualizacji sprzętu do tworzenia i izolowania bezpiecznego regionu pamięci od normalnego systemu operacyjnego. Windows może używać tego wirtualnego trybu bezpiecznego do hostowania szeregu rozwiązań zabezpieczających, zapewniając im znacznie zwiększoną ochronę przed lukami w systemie operacyjnym i zapobiegając wykorzystaniu złośliwych exploitów, które próbują pokonać zabezpieczenia.

Aby sprawdzi czy VBS jest wykorzystywane, należy w okienku uruchomi wpisać **msinfo32.exe**

![loc17](https://ba-pl.github.io/wiki/assets/images/loc17.png "loc17")
![loc18](https://ba-pl.github.io/wiki/assets/images/loc18.png "loc18")

#### Wyłączenie VBS dla Windows 7 lub Windows 10 

Aby wyłączyć tę funkcję w oknie uruchom wpisz komendę **gpedit.msc**

![loc19](https://ba-pl.github.io/wiki/assets/images/loc19.png "loc19")

Przechodzimy do sekcji  Computer configuration -> Administrative Templates -> System -> Device Guard -> Turn on virtualization based security

![loc20](https://ba-pl.github.io/wiki/assets/images/loc20.png "loc20")

W oknie, które się pojawi, wykonujemy konfigurację jak na zdjęciu poniżej:

![loc21](https://ba-pl.github.io/wiki/assets/images/loc21.png "loc21")

Następnie używamy cmd z prawami administratora i wykonujemy komedę **gpupdate /force**

![loc22](https://ba-pl.github.io/wiki/assets/images/loc22.png "loc22")

Następnie należy ponownie uruchomić komputer. **„Shutdown”** nie działa z Windows 10/11, ponieważ nie przeładowuje jądra.

#### Wyłącznie VBS dla Windows 11

Otwieramy ustawienia i wyszukujemy opcji *Core Isolation*:

![loc23](https://ba-pl.github.io/wiki/assets/images/loc23.png "loc23")

Następnie wyłączamy funkcję **Memory integrity** i restartujemy system.

![loc24](https://ba-pl.github.io/wiki/assets/images/loc24.png "loc24")

#### Dodatkowe ustawienia które mogą być wymagane

Może być konieczne wprowadzenie dodatkowych zmian w rejestrach.
**WAŻNE**: Jeśli dane rejestry ze zdjęć nie istnieją na Twoim komputerze, nie należy ich tworzyć
- Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity 
	- ENABLED = 0

![loc25](https://ba-pl.github.io/wiki/assets/images/loc25.png "loc25")

- Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\DeviceGuard 
	- EnableVirtualizationBasedSecurity = 0

![loc26](https://ba-pl.github.io/wiki/assets/images/loc26.png "loc26")

- Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\SystemGuard
	- ENABLED = 0 

![loc27](https://ba-pl.github.io/wiki/assets/images/loc27.png "loc27")

- Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\CredentialGuard
	- ENABLED = 0 

![loc28](https://ba-pl.github.io/wiki/assets/images/loc28.png "loc28")

#### Sprawdzenie czy VBS zostało poprawnie wyłączone

Aby sprawdzi czy VBS jest wyłączone, należy w okienku uruchomi wpisać **msinfo32.exae**:

![loc29](https://ba-pl.github.io/wiki/assets/images/loc29.png "loc29")

Ustawienie *Virtualization based security* powinno mieć status **Not enabled**:

![loc30](https://ba-pl.github.io/wiki/assets/images/loc30.png "loc30")

Zauważ również, że zobaczysz wiele wpisów dotyczących Hyper-V z wartością „yes”. Jest to w porządku, są to zależności Hyper-V dla osób, które chcą skonfigurować Hyper-V. W rzeczywistości nie jest on uruchomiony.
Jeśli zabezpieczenia oparte na wirtualizacji są wyłączone, powinieneś być gotowy do pracy z TwinCAT.

## Dodatek - przywracanie izolowanego rdzenia
Aby przywrócić na komputerze wyizolowany rdzeń należy wywołać okno „Set on Target” (w analogiczny sposób jak przy jego izolacji – rozdział 4.1). Następnie należy ustawić ilość rdzeni „Isolated” na 0 i wybrać „Set”. W następnym kroku wykonujemy restart komputera. 

![loc31](https://ba-pl.github.io/wiki/assets/images/loc31.png "loc31")

Jeśli mamy sytuację, w której rdzeń procesora nie jest widoczny w TwinCAT, przywracanie należy wykonać z poziomu systemu Windows. W tym celu należy w oknie uruchom wpisać komendę **msconfig**:

![loc32](https://ba-pl.github.io/wiki/assets/images/loc32.png "loc32")

W oknie które się pojawi należy wybrać zakładkę **Boot** i przycisk **Advanced options**. Następnie należy odznaczyć opcję **Number of processors** i zatwierdzić ustawienia przyciskiem OK. W kolejnym oknie należy wybrać przycisk **Apply** a następnie **OK**. Pojawi się komunikat o konieczności restartu komputera, który należy potwierdzić.

![loc33](https://ba-pl.github.io/wiki/assets/images/loc33.png "loc33")

Po restarcie komputera należy ponownie otworzyć zakładkę z ustawieniami rozruchu, zaznaczyć opcję **Number of processors** i z rozwijanej listy wybrać maksymalną ich liczbę. Zatwierdzić ustawienia w taki sam sposób jak poprzednio i ponownie zrestartować komputer. Wszystkie rdzenie będą aktywne.

![loc34](https://ba-pl.github.io/wiki/assets/images/loc34.png "loc34")
