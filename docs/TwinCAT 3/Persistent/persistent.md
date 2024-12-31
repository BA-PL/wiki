---
title: Zmienne PERSISTENT 
parent: - Zmienne nieulotne 
nav_order: 1
layout: page
---


# Zmienne PERSISTENT 
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Zmienne typu Persistent używane są w momencie w którym musimy zapamiętać istotne dla nas wartości zmiennych. W zależności od aplikacji będą to różne zmienne, jednak najczęściej zmienne Persistent stosuje się dla wartości takich jak:
- Ilości wykonanych sztuk czy sumaryczny czas pracy – dla maszyn produkcyjnych
- Nastawy regulatorów PID – dla aplikacji, które wymagają takich regulacji
- Stany i wartości natężenia świateł – w aplikacjach budynkowych
- Inne zmienne – zależnie od aplikacji

Należy pamiętać o tym, że zmienne Persistent zapisywane są na nośniku pamięci, a więc mamy możliwość przeniesienia ich między maszynami. W niektórych przypadkach bardzo może to ułatwić akcje serwisowe (np. w przypadku konieczności wymiany sterownika na maszynie wystarczy że przeniesiemy plik z zapisanymi zmiennymi Persistent i będą miały one w projekcie wartości identyczne jak na „starym” sterowniku).
<br>
Zmienne te zapisują się w momencie zadziałania któregoś z wcześniej wymienionych bloków – a więc albo w momencie żądania zapisu po otrzymaniu zewnętrznego sygnału , albo w momencie wykrycia zaniku zasilania i zadziałania bloku do obsługi 1-second UPS (o ile wybrany tryb działania 1-second UPS to przewiduje). Zostaną zapisane również w momencie zadziałania UPS w zasilaczu CX2100 jeżeli zostanie to odpowiednio skonfigurowane w oprogramowaniu na sterowniku.

# Deklaracja zmiennych PERSISTENT
Zmienne Persistent deklaruje się bezpośrednio poprzez umieszczenie ich na liście VAR PERSISTENT. Taka deklaracja jest dla TwinCAT znakiem, że zmienną należy traktować jako zmienną nieulotną. Zmienne Persistent mogą być deklarowane zarówno jako zmienne globalne, jak i zmienne lokalne, a więc poprawny jest zarówno zapis

![pers1](https://ba-pl.github.io/wiki/assets/images/pers1.png "pers1")

jak ich

![pers2](https://ba-pl.github.io/wiki/assets/images/pers2.png "pers2")

**UWAGA!!! W przypadku deklarowania jako Persistent wartości będących przypisaniem z innych obiektów należy pamiętać o tym, aby jako Persistent zadeklarowany był cały obiekt, a nie tylko zmienna, do której przepisywana jest wartość!**
<br>
<br>
W przypadku deklarowania jako Persistent zmiennej w bloku funkcyjnym musimy pamiętać, że każde wywołanie tego bloku funkcyjnego w kodzie spowoduje, że zapisywać się będzie kolejna „instacja” tej zmiennej na nośniku pamięci.
<br>
W przypadku korzystania ze sterowników z 1-second UPS co do zasady przyjmuje się, aby wielkość zmiennych Persistent w projekcie nie przekraczała 1 MB, ponieważ wraz ze zużyciem kondensatora używanego do podtrzymania w 1-second UPS większa ilość danych może nie zostać zapisana.
<br>
W przypadku integracji sterownika z zewnętrznym UPS ograniczenie co do ilości możliwych danych typu Persistent do zapisu wynika z pojemności UPS, a więc jesteśmy w stanie zapisać tyle danych Persistent, na ile pozwoli nam długość podtrzymania bateryjnego z UPS.
<br>
Ilość zapisywanych zmiennych Persistent możemy kontrolować przez sprawdzanie wielkości plików Port_xxx.bootdata oraz Port_xxx.bootdata-old znajdujących się w lokalizacji **TwinCAT/3.1/Boot/Plc**

![pers3](https://ba-pl.github.io/wiki/assets/images/pers3.png "pers3")

Plik \*.bootdata to aktualnie zapisany plik, który znika w momencie przejścia TwinCAT na urządzeniu w tryb Run, a tworzy się w momencie przejścia w tryb Config, natomiast plik \*.bootdata-old jest to poprzednio zapisany plik \*.bootdata. W momencie przejścia w tryb Run zawartość pliku \*.bootdata przepisywany jest do \*.bootdata-old.

# Zapis zmiennych PERSISTENT 
Sposób zapisu zmiennych niuelotnych będzie zależał od posiadanego hardware.
## Rodzjae UPS i sposoby integracji
W tym rozdziale opisane zostaną rodzaje zasilaczy z UPS, jakie mogą być zintegrowane z urządzeniami firmy Beckhoff.
<br>
UPS (ang. Uninterruptible Power Supply) jest to urządzenie posiadające własne podtrzymanie (najczęściej bateryjne lub akumulatorowe), które po utracie zasilania pozwala jeszcze na pewien czas niezakłóconej pracy urządzeniu do niego podłączonemu. Najczęściej jest to czas, który poświęcany jest na zapisanie pracy, ewentualne dokończenie procesu i bezpieczne wyłączenie urządzenia.
### Zewnętrzny zasilacz z UPS
Pierwszym z opisywanych rodzajów UPS jest zewnętrzny zasilacz z UPS, który następnie integrowany jest z urządzeniem. W takim wypadku zasilacz najczęściej jest w stanie przesłać do urządzenia sygnał o działaniu na podtrzymaniu bateryjnym, który to sygnał jest obsługiwany na różne sposoby – albo bezpośrednio w urządzeniu, albo w programie PLC.
<br>
Przykładowym urządzeniem takiego rodzaju są urządzenia serii **CU81** firmy Beckhoff.
### Zasilacz z UPS do sterowników modułowych (CX2xxx)
Drugim rodzajem UPS jest możliwość wyboru zasilacza do CX2xxx z wbudowanym UPS lub z możliwością integracji zewnętrznego UPS z tym zasilaczem. Wówczas zasilacz z UPS jest integralną częścią sterownika z możliwością jego wymiany na inny.
<br>
Integracja takiego UPS ze sterownikiem polega na konfiguracji go w odpowiednim programie, który domyślnie znajduje się w obrazie CX2xxx.
<br>
Są to urządzenia oznaczone jako CX2100-09xx.
### 1-second UPS 
1-second UPS jest to rozwiązanie stosowane przede wszystkim w sterownikach serii Embedded PC, ale znajduje zastosowanie również w niektórych Industrial PC, jak np. C6017.
<br>
Polega ono na tym, że na płycie głównej zintegrowany jest niewielki kondensator, który w wypadku zaniku zasilania z sieci podtrzymuje zasilanie samego sterownika na kilka sekund, tak by zdążył on wykonać określoną przez użytkownika akcję.
<br>
UPS taki integruje się przy pomocy bloku funkcyjnego w PLC, gdzie blok musi być odpowiedni dla zastosowanego sterownika – tj. dla CX9020 blok będzie inny niż dla CX5140. Odpowiednie bloki można znaleźć w dokumentacji sterownika w [Infosys](https://infosys.beckhoff.com)
<br>
W sterownikach serii CX81xx oraz CX5xxx występuje on jako standard. W sterownikach CX9020 oraz C60xx można wybrać go jako opcję przy zamówieniu.
## Bloki w PLC służące do zapisu zmiennych Persistent
Bloki służące do zapisu zmiennych Persistent dzielimy na dwie grupy: bloczki służące do zapisu danych Persistent w dowolnym momencie (na żądanie oraz w momencie obsługi sygnału pochodzącego z zewnętrznego UPS) oraz bloczki służące do integracji z konkretnym 1-second UPS.
### Blok ogólny FB_WritePersistentData
Jest to blok służący do zapisu zmiennych Persistent „na żądanie”, a więc w momencie wystąpienia zbocza narastającego na wejściu START.

![pers4](https://ba-pl.github.io/wiki/assets/images/pers4.png "pers4")

- NETID: AMS Net ID urządzenia na którym będziemy zapisywać dane. W przypadku pozostawienia pustej nóżki domyślnie jako urządzenie docelowe wybierane jest urządzenie lokalne
- PORT: numer Runtime, na którym zapisujemy dane. Runtime 1 to Port 851, Runtime 2 to 852 itd.
- START: po podaniu zbocza narastającego na tej nóżce nastąpi zapis zmiennych Persistent
- TMOUT: domyślny Timeout, po którego przekroczeniu blok zgłosi błąd
- MODE: tryb zapisu zmiennych Persistent, dokładny opis dostępny jest [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_utilities/35342347.html)
- BUSY: TRUE, jeżeli bloczek nie zakończył swojego działania
- ERR: TRUE, jeżeli wystąpił błąd
- ERRID: numer ewentualnego błędu

## Bloki służące do integracji z 1-second UPS
Do integracji z 1-second UPS należy zastosować odpowiedni blok funkcyjny dla każdego z urządzeń wyposażonych w tę funkcjonalność. Oznacza to, że w przypadku zastosowania niewłaściwego bloku UPS możemy doprowadzić do sytuacji, w której nasze zmienne Persistent w projekcie będą zapisywane w sposób niewłaściwy.
<br>
Wyróżniamy dwie „podgrupy” bloczków do integracji z 1-second UPS: bloki starszej generacji, gdzie każdy ze sterowników miał odpowiadający swojej płycie głównej bloczek, oraz nowszy bloczek BAPI, który w założeniu ma działać dla różnych płyt głównych – stosowany w przypadku sterowników nowszej generacji.
<br>
Jak zostało wspomniane wcześniej, informacje nt. 1-second UPS w danych sterownikach oraz nt. odpowiednich bloków funkcyjnych można znaleźć w [Infosys](https://infosys.beckhoff.com)
### FB_S_UPS_CB3011
Jest to blok funkcyjny przeznaczony dla urządzeń, które posiadają płytę główną o oznaczeniu CB3011. Są to urządzenia takie jak CP26xx-0000 oraz CP6606-0020
Blok funkcyjny wygląda następująco:

![pers5](https://ba-pl.github.io/wiki/assets/images/pers5.png "pers5")

- sNetID: AMS Net ID urządzenia na którym będziemy zapisywać dane. W przypadku pozostawienia pustej nóżki domyślnie jako urządzenie docelowe wybierane jest urządzenie lokalne
- iPLCPort: numer Runtime, na którym zapisujemy dane. Runtime 1 to Port 851, Runtime 2 to 852 itd.
- tTimeout: domyślny Timeout, po którego przekroczeniu blok zgłosi błąd
- eUpsMode: tryb działania bloku UPS (z/bez zapisu danych Persistent, z/bez szybkiego zamknięcia systemu), więcej informacji [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30505867.html)
- ePersistentMode: tryb zapisu zmiennych Persistent, dokładny opis dostępny [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_utilities/35342347.html)
- tRecoverTime: czas, po którym blok powróci do stanu pierwotnego w przypadku wybrania opcji bez zamknięcia systemu operacyjnego – czyli gdy zasilanie wróci szybciej niż rozładuje się kondensator
- bPowerFailDetect: TRUE gdy blok wykryje brak zasilania i sterownik przechodzi na zasilanie z wbudowanego kondensatora
- eState: aktualny stan bloku, dokładny opis znajduje się [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30507403.html)

### FB_S_UPS_CX81xx
Jest to blok funkcyjny przeznaczony dla urządzeń CX81xx.
Blok funkcyjny wygląda następująco:

![pers6](https://ba-pl.github.io/wiki/assets/images/pers6.png "pers6")

- sNetID: AMS Net ID urządzenia na którym będziemy zapisywać dane. W przypadku pozostawienia pustej nóżki domyślnie jako urządzenie docelowe wybierane jest urządzenie lokalne
- iPLCPort: numer Runtime, na którym zapisujemy dane. Runtime 1 to Port 851, Runtime 2 to 852 itd.
- tTimeout: domyślny Timeout, po którego przekroczeniu blok zgłosi błąd
- eUpsMode: tryb działania bloku UPS (z/bez zapisu danych Persistent, z/bez szybkiego zamknięcia systemu), więcej informacji [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30505867.html)
- ePersistentMode: tryb zapisu zmiennych Persistent, dokładny opis dostępny [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_utilities/35342347.html)
- tRecoverTime: czas, po którym blok powróci do stanu pierwotnego w przypadku wybrania opcji bez zamknięcia systemu operacyjnego – czyli gdy zasilanie wróci szybciej niż rozładuje się kondensator
- bPowerFailDetect: TRUE gdy blok wykryje brak zasilania i sterownik przechodzi na zasilanie z wbudowanego kondensatora
- eState: aktualny stan bloku, dokładny opis znajduje się [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30507403.html)

### FB_S_UPS_CX9020_U900
Jest to blok funkcyjny przeznaczony dla urządzenia CX9020, do którego dokupiona została opcja 1-second UPS o oznaczeniu CX9020-U900.
Blok funkcyjny wygląda następująco:

![pers7](https://ba-pl.github.io/wiki/assets/images/pers7.png "pers7")

- sNetID: AMS Net ID urządzenia na którym będziemy zapisywać dane. W przypadku pozostawienia pustej nóżki domyślnie jako urządzenie docelowe wybierane jest urządzenie lokalne
- iPLCPort: numer Runtime, na którym zapisujemy dane. Runtime 1 to Port 851, Runtime 2 to 852 itd.
- tTimeout: domyślny Timeout, po którego przekroczeniu blok zgłosi błąd
- eUpsMode: tryb działania bloku UPS (z/bez zapisu danych Persistent, z/bez szybkiego zamknięcia systemu), więcej informacji [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30505867.html)
- ePersistentMode: tryb zapisu zmiennych Persistent, dokładny opis dostępny [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_utilities/35342347.html)
- tRecoverTime: czas, po którym blok powróci do stanu pierwotnego w przypadku wybrania opcji bez zamknięcia systemu operacyjnego – czyli gdy zasilanie wróci szybciej niż rozładuje się kondensator
- bPowerFailDetect: TRUE gdy blok wykryje brak zasilania i sterownik przechodzi na zasilanie z wbudowanego kondensatora
- eState: aktualny stan bloku, dokładny opis znajduje się [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30507403.html)

### FB_S_UPS
Jest to blok funkcyjny przeznaczony dla urządzeń CX50x0.
Blok funkcyjny wygląda następująco:

![pers8](https://ba-pl.github.io/wiki/assets/images/pers8.png "pers8")

- sNetID: AMS Net ID urządzenia na którym będziemy zapisywać dane. W przypadku pozostawienia pustej nóżki domyślnie jako urządzenie docelowe wybierane jest urządzenie lokalne
- iPLCPort: numer Runtime, na którym zapisujemy dane. Runtime 1 to Port 851, Runtime 2 to 852 itd.
- iUPSPort: port do odczytu stanu UPS. Zwykle pozostawia się pusty, by przyjął wartość domyślną.
- tTimeout: domyślny Timeout, po którego przekroczeniu blok zgłosi błąd
- eUpsMode: tryb działania bloku UPS (z/bez zapisu danych Persistent, z/bez szybkiego zamknięcia systemu), więcej informacji [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30505867.html)
- ePersistentMode: tryb zapisu zmiennych Persistent, dokładny opis dostępny [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_utilities/35342347.html)
- tRecoverTime: czas, po którym blok powróci do stanu pierwotnego w przypadku wybrania opcji bez zamknięcia systemu operacyjnego – czyli gdy zasilanie wróci szybciej niż rozładuje się kondensator
- bPowerFailDetect: TRUE gdy blok wykryje brak zasilania i sterownik przechodzi na zasilanie z wbudowanego kondensatora
- eState: aktualny stan bloku, dokładny opis znajduje się [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30507403.html)

### FB_S_UPS_CX51x0
Jest to blok funkcyjny przeznaczony dla urządzeń CX51x0.
Blok funkcyjny wygląda następująco:

![pers9](https://ba-pl.github.io/wiki/assets/images/pers9.png "pers9")

-sNetID: AMS Net ID urządzenia na którym będziemy zapisywać dane. W przypadku pozostawienia pustej nóżki domyślnie jako urządzenie docelowe wybierane jest urządzenie lokalne
-iPLCPort: numer Runtime, na którym zapisujemy dane. Runtime 1 to Port 851, Runtime 2 to 852 itd.
-iUPSPort: port do odczytu stanu UPS. Zwykle pozostawia się pusty, by przyjął wartość domyślną.
-tTimeout: domyślny Timeout, po którego przekroczeniu blok zgłosi błąd
-eUpsMode: tryb działania bloku UPS (z/bez zapisu danych Persistent, z/bez szybkiego zamknięcia systemu), więcej informacji [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30505867.html)
-ePersistentMode: tryb zapisu zmiennych Persistent, dokładny opis dostępny [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_utilities/35342347.html)
-tRecoverTime: czas, po którym blok powróci do stanu pierwotnego w przypadku wybrania opcji bez zamknięcia systemu operacyjnego – czyli gdy zasilanie wróci szybciej niż rozładuje się kondensator
-bPowerFailDetect: TRUE gdy blok wykryje brak zasilania i sterownik przechodzi na zasilanie z wbudowanego kondensatora
-eState: aktualny stan bloku, dokładny opis znajduje się [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30507403.html)

### FB_S_UPS_BAPI
Jest to najbardziej zaawansowany blok funkcyjny przeznaczony do obsługi urządzeń wyposażonych w funkcjonalność 1-second UPS. Są to urządzenia posiadające BIOS-API w wersji 1.15 lub wyższej, a więc urządzenia takie jak:
- CX52xx
- C6017
- C6027

![pers10](https://ba-pl.github.io/wiki/assets/images/pers10.png "pers10")

- sNetID: AMS Net ID urządzenia na którym będziemy zapisywać dane. W przypadku pozostawienia pustej nóżki domyślnie jako urządzenie docelowe wybierane jest urządzenie lokalne
- iPLCPort: numer Runtime, na którym zapisujemy dane. Runtime 1 to Port 851, Runtime 2 to 852 itd.
- tTimeout: domyślny Timeout, po którego przekroczeniu blok zgłosi błąd
- eUpsMode: tryb działania bloku UPS (z/bez zapisu danych Persistent, z/bez szybkiego zamknięcia systemu), więcej informacji [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30505867.html)
- ePersistentMode: tryb zapisu zmiennych Persistent, dokładny opis dostępny [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_utilities/35342347.html)
- tRecoverTime: czas, po którym blok powróci do stanu pierwotnego w przypadku wybrania opcji bez zamknięcia systemu operacyjnego – czyli gdy zasilanie wróci szybciej niż rozładuje się kondensator
- bPowerFailDetect: TRUE gdy blok wykryje brak zasilania i sterownik przechodzi na zasilanie z wbudowanego kondensatora
- eState: aktualny stan bloku, dokładny opis znajduje się [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_sups/30507403.html)
- nCapacity: aktualny stan naładowania kondensatora w procentach
- bBusy: TRUE tak długo, aż blok nie zakończy działania
- bError: TRUE w momencie wystąpienia błędu
- nErrID: numer błędu w razie jego wystąpienia


