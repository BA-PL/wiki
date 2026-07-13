---
title: Funkcje Checks 
parent: Debugowanie projektu 
nav_order: 1
layout: page
---

# Funkcje Checks 
<br>
<h6> Data modyfikacji: 24.06.2026 </h6>
<br>

Instrukcja opisująca w jaki sposób wykorzystać funkcje diagnostyczne "Checks".
Służą one do wykrywania błędów programistycznych, które mogą prowadzić do tzw. Exception Mode lub samoczynnego restartu sterownika.
Szczegółowa dokumentacja: [Object POUs for implicit checks](https://infosys.beckhoff.com/content/1033/tc3_plc_intro/2530351499.html)

Funkcje zabezpieczają program PLC przed takimi operacjami jak:
* Dzielenie przez zero
* Przekroczenie zakresu tablicy
* Przekroczenie zakresu liczby ze znakiem lub bez znaku

Od wersji **TC3.1.4026.14** dostępny jest nowy sposób debugowania projektu przy ich użyciu.

<div class="code-example" markdown="1" style="background: rgba(210, 243, 242, 0.8)">

INFO
{: .label .label-purple }

Od wersji 3.1.4026.14 dostępna jest funkcja `CreateCallstackCoreDump()`, która pozwala na wykrycie miejsca błędu bez zatrzymywania sterownika. Dokumentacja: [Creation of a core dump via runtime function](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_plc_intro/18677193867.html&id=)
 
</div>

| Wersja Checksów | Do pobrania | Sposób debugowania |
|---|---|---|
| Podstawowa |[Download Checks](https://github.com/BA-PL/Checks_Base/archive/refs/heads/main.zip){: .btn .btn-red } | Breakpoint |
| Rozszerzona (TC >= 4026.14) | [Download Checks](https://github.com/BA-PL/Checks_Ex/archive/refs/heads/main.zip){: .btn .btn-red }| Breakpoint <br> `CreateCallstackCoreDump()` |

# Informacje ogólne

Funkcje Checks:
* Systemowe funkcje, które nie wymagają wywołania
* Działają automatycznie po dodaniu do projektu i wgraniu programu
* Przy każdym wystąpieniu błędu inkrementują licznik w GVL_Checks

<div class="code-example" markdown="1" style="background-color: rgba(255, 0, 0, 0.6); color: white">

UWAGA
{: .label .label-yellow }

 Nie można modyfikować nazwy oraz zmiennych wejściowych funkcji. Do poprawnego działania potrzebują zachować swoją pierwotną nazwę oraz argumenty wejściowe.
 
</div>

Opis zmiennych:

| Zmienna | Opis |
|---|---|
| `nCheckBounds` | Licznik przekroczeń zakresu indeksów (np. tablic) |
| `nCheckDivDInt` | Licznik prób dzielenia DINT przez 0 |
| `nCheckDivLInt` | Licznik prób dzielenia LINT przez 0 |
| `nCheckDivLReal` | Licznik prób dzielenia LREAL przez 0 |
| `nCheckDivReal` | Licznik prób dzielenia REAL przez 0 |
| `nCheckRangeSigned` | Licznik przekroczeń liczby ze znakiem, np. wartość poza zakresem INT(10..20) |
| `nCheckRangeUnsigned` | Licznik przekroczeń liczby bez znaku, np. wartość poza zakresem UINT(10..20) |
| `nCheckLRangeSigned` | Licznik przekroczeń liczby ze znakiem, np. wartość poza zakresem LINT(10..20) |
| `nCheckLRangeUnsigned` | Licznik przekroczeń liczby bez znaku, np. wartość poza zakresem ULINT(10..20) |

# Użycie funkcji Checks

## Import do programu

Pobrane funkcje Checks można zaimportować bezpośrednio do projektu.

<div class="code-example" markdown="1" style="background-color: rgba(255, 0, 0, 0.6);  ">

UWAGA
{: .label .label-yellow }

    Po dodaniu funkcji do projektu nie ma możliwości wgrania programu opcją Online Change.
    Wymagane jest wgranie projektu w trybie Download, czyli z zatrzymaniem programu.
 
</div>

![ImportFromZip](https://ba-pl.github.io/wiki/assets/images/checks/ImportFromZip.png)
![ImportFromZipFiles](https://ba-pl.github.io/wiki/assets/images/checks/ImportFromZipFiles.png)

<div class="code-example" markdown="1" style="background: rgba(210, 243, 242, 0.8)">

INFO
{: .label .label-purple }

	Domyślne funkcje Checks
	
    Istnieje możliwość importu domyślnych funkcji Checks bezpośrednio przez narzędzie TwinCAT. Liczniki każdej funkcji znajdują się wtedy wewnątrz, co utrudnia diagnostykę.
 
</div>

![DefaultChecks](https://ba-pl.github.io/wiki/assets/images/checks/DefaultChecks.png)

## Diagnostyka

### Sprawdzanie poprawności kodu
Do sprawdzania poprawności programu służą liczniki na liście zmiennych globalnych **GVL_Checks**.
![Liczniki](https://ba-pl.github.io/wiki/assets/images/checks/Liczniki.png)

Jeżeli któryś z liczników ma wartość różną od zera, oznacza to, że wystąpił błąd określonego typu.

### Szukanie miejsca wystąpienia błędu
Znalezienie miejsca wystąpienia błędu można zrealizować na dwa sposoby:
1. Breakpoint
2. Funkcja `CreateCallstackCoreDump()` - dostępna wraz z najnowszymi funkcjami Checks

#### Breakpoint
<br>
Breakpoint to znacznik ustawiany w kodzie, który zatrzymuje wykonanie programu w wybranym miejscu. Dzięki temu programista może podejrzeć aktualne wartości zmiennych i prześledzić działanie programu krok po kroku.

<div class="code-example" markdown="1" style="background-color: rgba(255, 0, 0, 0.6); ">

UWAGA
{: .label .label-yellow }

    Korzystanie z Breakpointów powoduje zatrzymanie działania całego programu PLC, co bardzo często wiąże się z zatrzymaniem całej maszyny w sposób nagły, niekontrolowany.
    Należy robić to z rozwagą.
 
</div>

Na podstawie licznika znajdujemy odpowiadającą mu nazwę zmiennej i otwieramy funkcję. W miejscu, gdzie naliczane są liczniki, ustawiamy Breakpointa.

![Breakpoint](https://ba-pl.github.io/wiki/assets/images/checks/Breakpoint.png)

Jeżeli linia kodu, na której jest postawiony Breakpoint, się wykona, to program zostanie zatrzymany. Pozwala to prześledzić ostatnie wywołania z okna **Call Stack**.

![CallStack](https://ba-pl.github.io/wiki/assets/images/checks/CallStack.png)

Wybierając poprzednie wywołanie z listy, jesteśmy w stanie namierzyć miejsce, które spowodowało wystąpienie Breakpoint'a.

![IdentyfikacjaMiejsca](https://ba-pl.github.io/wiki/assets/images/checks/IdentyfikacjaMiejsca.png)

#### Funkcja `CreateCallstackCoreDump()`
<br>

Funkcja pozwalająca w wybranym momencie zapisać plik core dump zawierający stos wywołań (Call Stack). Odbywa się to bez konieczności zatrzymywania programu breakpointem.
Plik **.core** tworzy się w katalogu `.\Boot\Plc\CoreDump`.

![KatalogBoot](https://ba-pl.github.io/wiki/assets/images/checks/KatalogBoot.png)

Plik taki należy przekopiować na komputer inżynierski, a następnie otworzyć w środowisku TwinCAT 3.

![LoadCoreDump](https://ba-pl.github.io/wiki/assets/images/checks/LoadCoreDump.png)

Po otwarciu pliku .core, narzędzie od razu przeniesie nas do funkcji, która spowodowała stworzenie pliku.
Korzystając ze stosu wywołań (**Call Stack**) można sprawdzić miejsce w kodzie, które spowodowało naliczenie się licznika.

![CallStack](Chttps://ba-pl.github.io/wiki/assets/images/checks/allStack.png)

Wybierając poprzednie wywołanie z listy, jesteśmy w stanie namierzyć miejsce, które spowodowało stworzenie pliku .core.


![IdentyfikacjaMiejsca](https://ba-pl.github.io/wiki/assets/images/checks/IdentyfikacjaMiejsca.png)

Plik .core należy zamknąć. Zwykłe wylogowanie się (Logout) nie powoduje zamknięcia pliku.

![CloseCoreDump](Chttps://ba-pl.github.io/wiki/assets/images/checks/loseCoreDump.png)

### Poprawienie kodu
Po znalezieniu miejsca, należy przeanalizować kod i dokonać zmiany w programie.
Nie należy zostawiać naliczających się Checks'ów.

### Zerowanie liczników
Po znalezieniu i poprawieniu programu, dobrą praktyką jest zerowanie liczników. Dzięki temu łatwiej jest zauważyć, że wystąpił błąd od ostatniej diagnostyki.

![LicznikiZera](https://ba-pl.github.io/wiki/assets/images/checks/LicznikiZera.png)

<div class="code-example" markdown="1" style="background-color: rgba(255, 0, 0, 0.6);">

UWAGA
{: .label .label-yellow }

    Pliki .core
	
    W celu ochrony dysku, plik .core zostanie stworzony tylko jeden raz, przy pierwszym naliczeniu się licznika.
    Ważne jest, aby po znalezieniu każdego kolejnego błędu resetować licznik do 0. Pozwoli to na stworzenie nowego pliku przy kolejnym błędzie.
	
</div>

