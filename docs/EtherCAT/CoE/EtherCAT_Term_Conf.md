---
title: Konfiguracja modułów 
nav_order: 2
layout: page
parent: EtherCAT
---

# Konfiguracja modułów 
{: .no_toc }
<h6> Data modyfikacji: 27.10.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}


# Zalecana konfiguracja modułów EtherCAT

W przypadku magistrali EtherCAT konfiguracja oraz diagnostyka modułów w pełni została zaimplementowana w środowisku TwinCAT. Pozwala to na wygodną obsługę urządzeń opartych o tę magistralę, bez konieczności instalowania dodatkowego oprogramowania, tak jak było to w przypadku magistrali K-Bus.

Poniżej opisane zostaną sposoby konfiguracji modułów, a także zalecane ustawienia, o których należy pamiętać, aby obsługa maszyny dla Służb Utrzymania Ruchu była bezproblemowa.

## Konfiguracja modułów EtherCAT przy pomocy zakładki CoE - Online

Zakładka CoE-Online (CANopen-over-EtherCAT) jest zakładką diagnostyczno-konfiguracyjną, która dostępna jest przy modułach posiadających rozszerzone Process Data (nie znajdziemy jej w prostych modułach, takich jak np standardowe moduły DI / DO serii EL100x czy EL200x). 
Zawiera ona szereg parametrów, które pozwalają zarówno na wybór sposobu działania modułu (jak na przykład wybór typu czujnika dla modułów RTD dla poszczególnych kanałów czy wybór parametrów komunikacji dla modułów komunikacji szeregowej) jak i na jego diagnostykę (jak na przykład indeksy diagnostyczne dla modułów enkoderowych czy silnikowych, które pozwalają na szybkie podejrzenie parametrów takich jak zasilanie, poprawność podłączenia czy poprawność zakresu sygnału wejściowego). 
Dzięki nim można szybko i łatwo dostać się do najważniejszych parametrów bez konieczności uruchamiania PLC czy manipulacji danymi procesowymi modułu.

Poniżej przedstawiono kilka funkcjonalności, które można zrealizować dzięki interfejsowi CoE-Online.

### Konfiguracja modułów (indeksy 16#80xx)

Są to indeksy, które najczęściej służą wyborowi takich parametrów jak tryb pracy, filtry, typy czujników czy parametry pracy. Są dostępne dla każdego kanału z osobna, co przekłada się na możliwość stosowania różnych typów urządzeń z tym samym modułem na różnych kanałach

![coe1](https://ba-pl.github.io/wiki/assets/images/CoE/ceo1.png "coe1")

### Diagnostyka modułów

Są to indeksy, które pozwalają na sprawdzenie parametrów takich jak aktualne wartości wejść dla modułów AI:

![coe2](https://ba-pl.github.io/wiki/assets/images/CoE/ceo2.png "coe2")

Czy indeksy diagnostyczne dla modułów posiadających takowy interfejs:

![coe3](https://ba-pl.github.io/wiki/assets/images/CoE/ceo3.png "coe3")

![coe4](https://ba-pl.github.io/wiki/assets/images/CoE/ceo4.png "coe4")

### Przywracanie ustawień fabrycznych modułów

Dzięki interfejsowi CoE-Online łatwo można przywrócić moduł do ustawień fabrycznych. Można to wykonać na dwa sposoby:

![coe5](https://ba-pl.github.io/wiki/assets/images/CoE/ceo5.png "coe5")

1. Wpisanie wartości "0x64616F6C" w pole HEX w Indes 1011, SubIndex 001:

![coe6](https://ba-pl.github.io/wiki/assets/images/CoE/ceo6.png "coe6")

2. Edycja wartości poprzez Hex Edit i wpisanie wartości 'load'

![coe7](https://ba-pl.github.io/wiki/assets/images/CoE/ceo7.png "coe7")

![coe8](https://ba-pl.github.io/wiki/assets/images/CoE/ceo8.png "coe8")

## Konfiguracja modułów przy pomocy zakładki Startup

Zakładka Startup posiada inny interfejs niż zakładka CoE-Online i służy przede wszystkim do konfiguracji modułów (nie posiada funkcjonalności diagnostycznych). Jej najważniejszą cechą jest to, że **o ile to, co konfigurujemy w zakładce CoE-Online zapisuje się w EEPROM modułu, tak zakładka Startup zapisywana jest bezpośrednio w sterowniku, który w tym przypadku jest Masterem sieci EtherCAT**. Dzięki temu jest ona ładowana **przy każdym starcie systemu**, co przekłada się na to, że moduł skonfigurowany przy pomocy tej zakładki będzie konfigurowany przy każdym uruchomieniu maszyny.

### Sposób konfiguracji przy pomocy zakładki Startup

W zakładce tej wybieramy, który parametr ma zostać nadpisany - oraz w którym momencie - jako że wybieramy tzw. Transition - czyli moment przejścia między stanami EtherCAT modułu, w którym dane zostaną wpisane.

**Liczba pojedyncza jest tu nieprzypadkowa, każdy pojedyczny SubIndex należy skonfigurować oddzielnie!!!**

![coe9](https://ba-pl.github.io/wiki/assets/images/CoE/ceo9.png "coe9")

![coe10](https://ba-pl.github.io/wiki/assets/images/CoE/ceo10.png "coe10")

## Dlaczego warto stosować konfigurację modułu przy pomocy Startup List?

Po uruchomieniu urządzenia typu EtherCAT Slave następuje przejście modułu przez wszystkie stany (INIT -> PREOP -> SAFEOP ->OP). W takim przypadku tranzycje zdefiniowane w Startup List zawsze mają miejsce. Daje nam to następujące zalety:
* nawet jeśli urządzenie zostało przekonfigurowane w trakcie pracy, po restarcie powróci do ustawień ze Startup List, co jest idealne w przypadku konieczności zastosowania rozwiązań "tymczasowych" (np. inny typ czujnika w przypadku awarii)
* Starup List jest eksportowalna / importowalna. Dzięki temu można w łatwy i wygodny sposób przenosić konfigurację między projektami czy poszczególnymi modułami, jeśli mamy w aplikacji wiele modułów tego samego typu
* W przypadku awarii modułu mamy pewność, że nieważne skąd będzie pochodzić moduł (nowy z pudełka, z innej maszyny, "z odzysku") - jeśli pierwszą komendą w Startup List będzie przywrócenie do ustawień fabrycznych (patrz wyżej) moduł ZAWSZE będzie traktowany jak moduł nowy, wyjęty z pudełka
* Służby Utrzymania Ruchu nie muszą podczas wymiany sprzętu martwić się o jego konfigurację - wystarczy w miejsce uszkodzonego modułu wstawić moduł tego samego typu, a podczas uruchamiania "skonfiguruje się sam".
* Mamy możliwość reakcji w przypadku nieprawidłowego zadziałania aplikacji (możliwość konfigurowania tranzycji nie tylko w "pozytywną" stronę, ale również tranzycji takich jak przejście modułu ze stanu OP do SAFEOP wywołane np. watchdogiem)