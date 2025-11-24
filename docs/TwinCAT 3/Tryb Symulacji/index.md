---
title: Tryb symulacji
nav_order: 2
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

![loc1](https://ba-pl.github.io/wiki/assets/images/local/loc1.png "loc1")

- uruchomienie pliku **win8settick.bat** z prawami Administratora, a następnie restart komputera
	- dla TwinCAT w wersji 3.1.4024.x i niższych, plik znajduje się w lokalizacji *C:\TwinCAT\3.1\System*
	- dla TwinCAT w wersji 3.1.4026 plik znajduje się w lokalizacji *C:\Program Files (x86)\Beckhoff\TwinCAT\3.1\System*

![loc2](https://ba-pl.github.io/wiki/assets/images/local/loc2.png "loc2")

# Przygotowanie projektu 
Jeżeli posiadamy gotowy projekt, który chcielibyśmy uruchomić w trybie symulacji, to w pierwszej kolejności należy:
- jako urządzenie docelowe wybrać *Local* czyli lokalny komputer

![loc3](https://ba-pl.github.io/wiki/assets/images/local/loc3.png "loc3")

- jeżeli w konfiguracji znajdują się urządzenia, należy je czasowo wyłączyć w projekcie (nie symulujemy hardware’u). Można to zrobić klikając na odpowiednie urządzenie główne PPM i wybierając opcję *Disable* (nie powoduje to żadnych zmian w konfiguracji oprócz czasowej dezaktywacji urządzeń, tj. ewentualne linkowanie zmiennych nie zostanie utracone)

![loc4](https://ba-pl.github.io/wiki/assets/images/local/loc4.png "loc4")

- dodatkowo, należy również czasowo zablokować w kodzie PLC elementy bezpośrednio odwołujące się do sprzętu, jak np. blok do obsługi 1s UPS

![loc5](https://ba-pl.github.io/wiki/assets/images/local/loc5.png "loc5")

- jeśli w projekcie do licencji używany jest dongle, którego aktualnie nie mamy, należy również go zablokować

![loc6](https://ba-pl.github.io/wiki/assets/images/local/loc6.png "loc6")

## Uruchomienie projektu
Po wykonaniu czynności z poprzednich rozdziałów, można wykonać próbę uruchomienia projektu:
- aktywujemy konfigurację 

![loc7](https://ba-pl.github.io/wiki/assets/images/local/loc7.png "loc7")

- opcję *Autostart PLC Boot Project* przy pierwszym uruchomieniu lepiej zostawić odznaczoną, potwierdzamy ten komunikat i kolejny o restarcie TwinCAT do trybu Run

![loc8](https://ba-pl.github.io/wiki/assets/images/local/loc8.png "loc8")

- jeżeli powyższe kroki się powiodą, uruchamiany projekt PLC

![loc9](https://ba-pl.github.io/wiki/assets/images/local/loc9.png "loc9")

## Możliwe problemy 

### Izolacja rdzenia
Podczas aktywacji konfiguracji może pojawić się komunikat:

![loc10](https://ba-pl.github.io/wiki/assets/images/local/loc10.png "loc10")

W takim wypadku przed uruchomieniem dobrze jest wykonać izolację procesora logicznego na komputerze (dobrze jest izolować ich parzystą liczbę) i uruchamiać TwinCAT właśnie na tych wyizolowanych rdzeniach. 
<br>
W projekcie należy odnaleźć ustawienia **Real-Time** i w zakładce **Settings** kliknąć na przycisk **Read from Target** (wyświetlone dane będą różnić się w zależności od komputera).

![loc11](https://ba-pl.github.io/wiki/assets/images/local/loc11.png "loc11")

Następnie w tej samej zakładce wybieramy przycisk **Set on Target**. W oknie **Change numbers of Windows CPU** edytujemy ustawienia w ten sposób, aby w polu Isolated znalazła się wartość Po zmianie wybieramy **Set** i potwierdzamy pojawiające się komunikaty. Zmiana tych ustawień wymaga restartu komputera.

![loc12](https://ba-pl.github.io/wiki/assets/images/local/loc12.png "loc12")

Po restarcie komputera należy wrócić do zakładki **Real-Time -> Settings**. Na liście dostępnych wątków, niektóre pojawią się z atrybutem Isolated. Należy przy nim zaznaczyć pole w kolumnie RT-CPU a wcześniej wybraną opcję odznaczyć. Aby zmiany zostały wprowadzone należy aktywować konfigurację.

### HyperV
Podczas aktywacji konfiguracji pojawia się błąd zawierający informację o HyperV:

![loc13](https://ba-pl.github.io/wiki/assets/images/local/loc13.png "loc13")

W takiej sytuacji należy wyłączyć HyperV w funkcjach systemu. Można to zrobić z poziomu panelu sterownia:

![loc14](https://ba-pl.github.io/wiki/assets/images/local/loc14.png "loc14")

- odznaczamy całą sekcję dotyczącą HyperV

![loc15](https://ba-pl.github.io/wiki/assets/images/local/loc15.png "loc15")

- jeśli jest zaznaczona opcja Virtual Machine Platform, to również odznaczamy

![loc16](https://ba-pl.github.io/wiki/assets/images/local/loc16.png "loc16")

- jeśli powyższe kroki zostały wykonane a błąd nadal się pojawia, może być konieczne wyłączenie uruchamiania Hypervisora. Robi się to komendą z poziomu cmd uruchomionego z prawami administratora, a następnie zrestartować system

```
bcdedit /set hypervisorlaunchtype off
```

(aby ponownie włączyć tę opcję, należy użyć komendy bcdedit /set hypervisorlaunchtype auto)

### Virtualization based security

Zabezpieczenia oparte na wirtualizacji wykorzystują funkcje wirtualizacji sprzętu do tworzenia i izolowania bezpiecznego regionu pamięci od normalnego systemu operacyjnego. Windows może używać tego wirtualnego trybu bezpiecznego do hostowania szeregu rozwiązań zabezpieczających, zapewniając im znacznie zwiększoną ochronę przed lukami w systemie operacyjnym i zapobiegając wykorzystaniu złośliwych exploitów, które próbują pokonać zabezpieczenia.

Aby sprawdzić czy VBS jest wykorzystywane, należy w okienku uruchomi wpisać **msinfo32.exe**

![loc17](https://ba-pl.github.io/wiki/assets/images/local/loc17.png "loc17")
![loc18](https://ba-pl.github.io/wiki/assets/images/local/loc18.png "loc18")
<br>

**Wyłączenie VBS** 

<div class="code-example" markdown="1" style="background: rgba(255, 0, 0, 0.8)">
Na własne ryzyko!
</div>

Od TwinCAT w wersji 4026.15 po instalacji pojawia się skrypt ułatwiający wyłączenie Virtualization Based Security na komputerach. Aby go uruchomić na komputerze programisty uruchamiamy PowerShell z uprawnieniami Administratora i uruchamiamy skrypt:

`C:\Program Files (x86)\Beckhoff\TwinCAT\3.1\System\DisableVirtualizationBasedSecurity.ps1`

W skrypcie znajduje się notatka z którą należy się zapoznać:
```
PLEASE NOTE:
The TwinCAT runtime environment cannot be started within a Hyper-V environment. As soon as a component of the computer uses Hyper-V, only the engineering environment (XAE) can be used on this computer, but not the runtime environment (XAR).
In addition to software solutions for virtual machines, Hyper-V can also be used by operating system tools (Device Guard, Credential Guard, VBS,...) or other Hyper-V programs.
Further Information about TwinCAT requirements: https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_overview/6162419083.html

WARNING:
This script helps to configure the system accordingly and will disable VBS, which are critical for protecting your system against advanced threats.
Consultation required: Please consult with your IT department or the end customer before proceeding to ensure this action
aligns with security policies.

PROCEED WITH CAUTION:
Understand the risks and seek guidance if necessary. By continuing, you accept responsibility for these changes.
```
Znane są nam przypadki w których wyłączenie VBS powoduje nieprawidłowe działanie funkcji BitLocker.
Jeśli korzystamy z niej, nie polecamy wyłączania VBS a inne rozwiązania jak np. maszyna wirtualna.

Inna alterantywa to TwinCAT User Mode w którym możemy uruchomić symulację: [link](https://infosys.beckhoff.com/content/1033/tc170x_tc3_usermode_runtime/index.html)
<br>
Niestety na ten moment posiada on ograniczenia, z którymi warto się zapoznać: [link](https://infosys.beckhoff.com/content/1033/tc170x_tc3_usermode_runtime/11319889035.html)


### Dodatek - przywracanie izolowanego rdzenia
Aby przywrócić na komputerze wyizolowany rdzeń należy wywołać okno „Set on Target” (w analogiczny sposób jak przy jego izolacji – rozdział 4.1). Następnie należy ustawić ilość rdzeni „Isolated” na 0 i wybrać „Set”. W następnym kroku wykonujemy restart komputera. 

![loc31](https://ba-pl.github.io/wiki/assets/images/local/loc31.png "loc31")

Jeśli mamy sytuację, w której rdzeń procesora nie jest widoczny w TwinCAT, przywracanie należy wykonać z poziomu systemu Windows. W tym celu należy w oknie uruchom wpisać komendę **msconfig**:

![loc32v2](https://ba-pl.github.io/wiki/assets/images/local/loc32v2.png "loc32v2")

W oknie które się pojawi należy wybrać zakładkę **Boot** i przycisk **Advanced options**. Następnie należy odznaczyć opcję **Number of processors** i zatwierdzić ustawienia przyciskiem OK. W kolejnym oknie należy wybrać przycisk **Apply** a następnie **OK**. Pojawi się komunikat o konieczności restartu komputera, który należy potwierdzić.

![loc33v2](https://ba-pl.github.io/wiki/assets/images/local/loc33v2.png "loc33v2")

Po restarcie komputera należy ponownie otworzyć zakładkę z ustawieniami rozruchu, zaznaczyć opcję **Number of processors** i z rozwijanej listy wybrać maksymalną ich liczbę. Zatwierdzić ustawienia w taki sam sposób jak poprzednio i ponownie zrestartować komputer. Wszystkie rdzenie będą aktywne.

![loc34v2](https://ba-pl.github.io/wiki/assets/images/local/loc34v2.png "loc34v2")

