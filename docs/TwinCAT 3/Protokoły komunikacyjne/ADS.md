---
title: ADS
parent: Protokoły komunikacyjne TF6xxx
nav_order: 1
layout: page
---


# Komunikacja ADS
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Protokół ADS to asynchroniczny protokół komunikacyjny wykorzystywany w systemie TwinCAT. Definiuje on
komunikację typu klient – serwer. Serwerem nazywamy komputer (sterownik), który udostępnia pewne dane,
natomiast klientami nazywamy komputery (sterowniki), które modyfikują dane udostępnione przez serwer. Sieć
oparta na protokole ADS jest siecią typu multimaster, co oznacza, że w sieci może występować wiele jednostek
nadrzędnych – serwerów. W przypadku sterowników z serii CX możliwe jest nawiązanie nieograniczonej liczby
połączeń pomiędzy klientami a serwerami.

![ads](https://ba-pl.github.io/wiki/assets/images/ADS/ads.png "ads")

# Konfiguracja sterowników

W celu przesyłania danych pomiędzy dwoma sterownikami za pomocą protokołu ADS, każdy z nich musi mieć
dodany drugi sterownik jako route’a. Realizacja wymiany zmiennych za pomocą protokołu ADS odbywa
się w programie PLC.

![routes](https://ba-pl.github.io/wiki/assets/images/ADS/routes.png "routes")

**UWAGA! Dla sterowników z TwinCAT 3.1.4024.0 i nowszych dane do logowania to „Administrator” z hasłem „1” dla wszystkich urządzeń (również z Windows CE)!**

# Program PLC dla serwera

Możliwy jest odczyt/zapis danych znajdujących się w przestrzeni Memory, 
<br>
czyli (zadeklarowanych … AT %M …). 
<br>
W pokazanym przykładzie z serwera odczytywana jest zmienna typu STRING zadeklarowana w sposób pokazany poniżej.

![address](https://ba-pl.github.io/wiki/assets/images/ADS/address.png "address")

# Program PLC dla klienta

W programie sterownika pełniącego rolę klienta konieczne jest dodanie odpowiedniej biblioteki –
Tc2_System.lib.
<br>
W programie uruchomianym na kliencie używamy bloku funkcyjnego ADSREAD. W bloku tym wsytępują
następujące wejścia:

- **NETID** (T_AmsNetId): Adres ADS sterownika, który jest serwerem,
- **PORT** (T_AmsPort): Numer portu wykorzystywanego przez PLC Runtime. W przypadku PC i CX dla TC3 dla Runtime 1 jest to port 851, dla Runtime 2 jest to port 852 itd.
- **IDXGRP** (UDINT): Indeks grupy usługi ADS. Wskazuje on na obszar pamięci z którego dokonujemy
odczytu. W przypadku odczytu/zapisu zmiennych przestrzeni Memory (flaga %M) jest to zawsze
wartość 16#4020,
- **IDXOFFS** (UDINT): Offset indeksu usługi ADS. Wskazuje on od którego bajtu chcemy odczytywać
pamięć,
- **LEN** (UDINT): Ilość bajtów które chcemy odczytać. Jest to jednocześnie rozmiar zmiennej do której
dokonujemy zapisu (w bajtach) – najczęściej odnosimy się do niej funkcją SIZEOF,
- **DESTADR** (DWORD): Adres zmiennej do której wpisujemy odczytaną wartość, najczęsciej odczytujemy
go funkcją ADR,
- **READ** (BOOL): Wejście reagujące na zbocze narastające, służy do wydawania polecenia odczytu,
- **TMOUT** (TIME): Limit czasu odczytu, po jego przekroczeniu blok sygnalizuje błąd.

<br>
<br>
Wyjścia z bloku są następujące:
- **BUSY** (BOOL): Wyjście sygnalizujące, że blok wykonuje operację odczytu,
- **ERR** (BOOL): Wyjście sygnalizujące wystąpienie błędu. Przyjmuje ono wartość TRUE, gdy wyjście BUSY
przyjmie wartość FALSE, a odczyt zakończy się niepowodzeniem,
- **ERRID** (UDINT): ADS Return Code, znaczenie kodu można odnaleźć w dokumentacji protokołu.

![client](https://ba-pl.github.io/wiki/assets/images/ADS/client.png "client")

![adsread](https://ba-pl.github.io/wiki/assets/images/ADS/adsread.png "adsread")

Analogicznie wygląda wywołanie bloku ADSWRITE, z tą różnicą, że jako LEN i SRCADDR podaje się odnośniki do
zmiennej, którą chcemy ZAPISAĆ na wybranym obszarze pamięci w sterowniku który jest serwerem.
# Odczyt po nazwach 

Istnieje również możliwość odczytu zmiennych po ich nazwach. W tym celu należy użyć bloków [FB_ReadAdsSymByName](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_dataexchange/1783919627.html) lub [FB_WriteAdsSymByName](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_dataexchange/1783921547.html) pochodzaćych z bliblioteki Tc2_DataExchange. 
# Tips&Tricks

## Adresowanie 

W pokazanym wcześniej przykładzie przesyłana zmienna została zadeklarowana w pamięci jako **AT %MB0**, co
oznacza że zostanie ona zapisana w pamięci zaczynając od zerowego bajtu. **AT %MB4** oznaczałoby, że zmienna
zostanie zapisana w pamięci zaczynając od piątego bajtu. Inne możliwe deklaracje to:
- **AT %MX** : umożliwia adresowanie pojedynczych bitów, np. AT %MX4.3 to 4 bit 5 bajtu,
- **AT %MW** : służy do adresowania zmiennych dwubajtowych,
- **AT %MD** : służy do adresowania zamiennych czterobajtowych.

<br>
W praktyce zasadne jest używanie jedynie zmiennych deklarowanych jako AT %MX oraz AT %MB, ponieważ
ostatecznie wszystkie zmienne są umieszczane we wspólnym obszarze pamięci więc deklaracje AT %MB0, AT
%MW0 i AT %MD0 dają identyczny rezultat.
<br>
Przy deklarowaniu zmiennych należy uważać, aby adres był podzielny przez rozmiar adresowanej zmiennej, np.
dla zmiennej typu REAL (4-bajtowej) zalecane adresy to AT %MB0, AT %MB4, AT %MB8 itd. Podanie niezalecanego
adresu skutkuje warningiem, przykład poniżej.

![size](https://ba-pl.github.io/wiki/assets/images/ADS/size.png "size")

## Nakładanie obszarów pamięci 

Przydatną możliwością jest nakładanie jednej zmiennej na kilka innych i odczytywanie tym sposobem kilku
zmiennych jednocześnie (za pomocą jednego bloku). Zostało to pokazane na rysunku poniżej. Na tablicę bajtów
zostały nałożone różnego typu zmienne zmienne. Po stronie klienta i serwera układ zmiennych powinien być taki
sam, wówczas rozkodowanie zmiennych odbędzie się samoczynnie.

![overlap](https://ba-pl.github.io/wiki/assets/images/ADS/overlap.png "overlap")

W kliencie do bloku funkcyjnego odczytu/zapisu podpinamy tylko zmienną tablicową – ZM. Pozostałe zmienne
uzupełnią swoje wartości samoczynnie. Jest to pokazane poniżej.
<br>
Widok offline:

![adsoffline](https://ba-pl.github.io/wiki/assets/images/ADS/adsoffline.png "adsoffline")

Widok online:

![adsonline](https://ba-pl.github.io/wiki/assets/images/ADS/adsonline.png "adsonline")

## Odczyt adresu AMS Net ID 

Adres AMS Net Id sterownika można odczytać w kilku miejscach:
<br>
- Jeśli znamy adres IP: poprzez stronę diagnostyczną Device Manager

![amsdevman](https://ba-pl.github.io/wiki/assets/images/ADS/amsdevman.png "amsdevman")

- W TwinCAT: W zakładce Routes --> NetId Management

![netidman](https://ba-pl.github.io/wiki/assets/images/ADS/netidman.png "netidman")

- Po wybraniu Router --> Edit Routes po kliknięciu PPM na ikonie TC na pasku:

![routesman](https://ba-pl.github.io/wiki/assets/images/ADS/routesman.png "routesman")

- W zakładce „Choose Target System”:

![choosetarget](https://ba-pl.github.io/wiki/assets/images/ADS/choosetarget.png "choosetarget")

# Przykład aplikacji 

Przykładową aplikację możesz pobrać tutaj:
<br>
<br>
[Download ADS Sample](https://github.com/BA-PL/ADS-TC3/archive/refs/heads/main.zip){: .btn .btn-red }


