---
title: Przygotowanie IPC 
parent: Windows 7/10/11 
nav_order: 3
layout: page
---

# Przygotowanie nowego IPC do pracy
{: .no_toc }
<h6> Data modyfikacji: 11.06.2026 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Poniższa instrukcja pokazuje krok po kroku jak należy skonfigurować komputer firmy Beckhoff do działania po wyjęciu z pudełka.

Pierwsze kroki zostały również pokazane w jednym z odcinków z serii **NA HARDKODZIE**: [Jak przygotować komputer Beckhoff do pracy?](https://youtu.be/ni1llA-99rc?si=EuX4neRvcpAZmxGs)

# Dostęp do komputera

Do komputera można podpiąć się na dwa sposoby:

1. **Fizycznie**: podpinając monitor, mysz i klawiaturę

2. **Sieciowo**: wykorzystując protokół RDP, czyli Podłączenie pulpitu zdalnego (ang. Remote Desktop Connection). Do realizacji takiego połączenia potrzebny jest adres sieciowy komputera, a sposób na jego uzyskanie opisany jest poniżej.

# Adres sieciowy komputera

Każdy adapter sieciowy w naszych komputerach posiada ustawiony adres DHCP.

Niektóre adaptery sieciowe posiadają przyporządkowane DIP Switche, które pozwalają na zmianę ustawień z DHCP na adres sieciowy z odpowiedniej puli. W takich przypadkach konfiguracja DIP Switchy opisana jest w dokumentacji sterownika.

Aby podłączyć się do komputera po sieci, musimy znać jego adres IP.

## Przypadek 1 - TwinCAT jest fabrycznie zainstalowany na IPC

Jeżeli TwinCAT jest zainstalowany, to możemy wyszukać komputer przy użyciu routera ADS, który wyszukuje wszystkie routery na sieci. Służy do tego opcja **Broadcast Search**.

![BroadcastSearch](https://ba-pl.github.io/wiki/assets/images/ipc/BroadcastSearch.png "Broadcastsearch")

## Przypadek 2 - Brak fabrycznie zainstalowanego TwinCAT'a na IPC

W przypadku braku routera ADS, należy wykorzystać oprogramowanie firm trzecich. Wybór narzędzia najlepiej uzależnić od konfiguracji sieciowej.

### Advance IP Scanner

<div class="code-example" markdown="1" style="background: rgba(210, 243, 242, 0.8)">

INFO
{: .label .label-purple }

Kiedy?

Na sieci znajduje się router, który nadaje adresy IP z konkretnej puli.
 
</div>

Narzędzie pozwala skanować konkretną podsieć, w której znajduje się nasz sterownik. Jeżeli znana jest podsieć, w której nadawane są adresy, to narzędzie wylistuje wszystkie urządzenia widoczne na tej sieci.

Dla przykładu, router nadaje adresy w podsieci **192.168.55.x**. Do wykonania skanu należy wpisać `192.168.55.0-255`, co pozwoli na przeskanowanie całej podsieci.

![AdvanceIpScanner](https://ba-pl.github.io/wiki/assets/images/ipc/AdvanceIpScanner.png "AdvancedIPScanner")

Komputer można zidentyfikować na podstawie **Hostname** lub adresu **MAC**.

Hostname może się różnić w zależności od systemu Windows oraz typu komputera. Może się składać z:
* Numeru seryjnego (BTN)
  * BTN-0000000
* Adresu MAC (6 ostatnich znaków jednej z kart sieciowych)
  * CX-ABCDEF
  * CP-ABCDEF

### Wireshark

<div class="code-example" markdown="1" style="background: rgba(210, 243, 242, 0.8)">

INFO
{: .label .label-purple }

Kiedy?

Sterownik jest podłączony bezpośrednio (1:1) z komputerem inżynierskim, bez routera.
 
</div>

Narzędzie pozwala prześledzić ruch na wybranym adapterze sieciowym. W celu znalezienia komputera należy wykonać poniższe kroki:
1. Rozpoczynamy przechwytywanie ramek na wybranym adapterze sieciowym.

2. Wypinamy i wpinamy przewód sieciowy do sterownika.

3. Ustawiamy filtry.

    `eth.addr[0:3] == 00:01:05 && (arp || nbns)`

4. Potwierdzamy na podstawie adresu **MAC**, że jest to nasz komputer.

![Wireshark](https://ba-pl.github.io/wiki/assets/images/ipc/Wireshark.png "Wireshark")

## Podłączenie zdalnego pulpitu

Do komputera łączymy się protokołem RDP, wykorzystując aplikację **Podłączenie pulpitu zdalnego** (ang. Remote Desktop Connection) i wpisując adres IP sterownika.

![Rdp](https://ba-pl.github.io/wiki/assets/images/ipc/Rdp.png "RDP")

* Login: **Administrator**
* Hasło: **1**

![Security](https://ba-pl.github.io/wiki/assets/images/ipc/Security.png "Security")

Potwierdzamy certyfikaty.

![Certs](https://ba-pl.github.io/wiki/assets/images/ipc/Certs.png "Certyfikaty")

# Instalacja TwinCAT XAR

Obecnie są do wyboru dwa buildy TwinCAT'a 3:
* **3.1.4024.x** - Starszy build, do którego wprowadzane są już tylko poprawki. Instalacja XAR i dodatków oparta o pliki .exe.
* **3.1.4026.x** - Aktualnie rozwijany build. Instalacja XAR i dodatków oparta o TwinCAT Package Managera.

<div class="code-example" markdown="1" style="background-color: rgba(255, 0, 0, 0.6); color: white">

UWAGA
{: .label .label-yellow }

Nie wolno mieszać typów instalacji — używamy wyłącznie **TwinCAT Package Managera** lub wyłącznie plików **.exe**.
 
</div>

## 3.1.4024.x

Najnowsza wersja dostępna jest na stronie internetowej [beckhoff.com](https://www.beckhoff.com).

![XarExe](https://ba-pl.github.io/wiki/assets/images/ipc/XarExe.png "Xarexe")

## 3.1.4026.x

Pierwsze kroki z Package Managerem są omówione w rozdziale:

[Pobieranie i instalacja Package Manager](https://ba-pl.github.io/wiki/docs/TwinCAT%203/Instalacja/Instalacja/#pobieranie-i-instalacja-package-manager).

<div class="code-example" markdown="1" style="background: rgba(210, 243, 242, 0.8)">

 Wskazówka
{: .label .label-purple }

Jeżeli nie instalujemy narzędzia inżynierskiego XAE, nie trzeba integrować edytora podczas konfiguracji TwinCAT Package Managera.

</div>

Do uruchomienia XAR należy zainstalować workload **TwinCAT Standard: Runtime**.

![TwincatStandardRuntime](https://ba-pl.github.io/wiki/assets/images/ipc/TwincatStandardRuntime.png "TcStandard")

# Uruchomienie win8settick.bat

Do aktywacji konfiguracji na sterowniku wymagane jest uruchomienie pliku **win8settick.bat**, który przygotowuje system Windows do pracy z RealTime'em.

Plik należy uruchomić jako Administrator, a następnie uruchomić ponownie komputer wykonując miekki restart.

| Build | Katalog |
|---|---|
| 4024 | `C:\TwinCAT\3.1\System` |
| 4026 | `C:\Program Files (x86)\Beckhoff\TwinCAT\3.1\System` |

# Instalacja Driverów RT

Instalacja driverów jest konieczna w celu uruchomienia protokołów RT na adapterach sieciowych. Do takich protokołów należą między innymi: **EtherCAT**, **Profinet**, **Ethernet/IP**.

<div class="code-example" markdown="1" style="background: rgba(210, 243, 242, 0.8)">

 Wskazówka
{: .label .label-purple }

Nie trzeba instalować driverów RT na każdym adapterze sieciowym. Wystarczą tylko te, które mają obsługiwać wymienione protokoły.
 
</div>

Służy do tego aplikacja **TcRteInstall.exe**, gdzie mamy dostęp do wszystkich adapterów sieciowych na sterowniku. Instalację należy przeprowadzać wyłącznie na kompatybilnych adapterach sieciowych.

![TcRteInstall](https://ba-pl.github.io/wiki/assets/images/ipc/TcRteInstall.png "TcRte")

| Build | Katalog |
|---|---|
| 4024 | `C:\TwinCAT\3.1\System` |
| 4026 | `C:\Program Files (x86)\Beckhoff\TwinCAT\3.1\System` |

<div class="code-example" markdown="1" style="background-color: rgba(255, 0, 0, 0.6); color: white">

UWAGA
{: .label .label-yellow }

Podczas instalacji driverów RT restartują się adaptery sieciowe, co może powodować chwilową utratę połączenia z komputerem. W takim przypadku należy odczekać kilka sekund i ponowić połączenie.

</div>
	