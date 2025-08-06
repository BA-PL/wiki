---
title: InfluxDB 2.0
parent: Database Server 
nav_order: 1
layout: page
---

# InfluxDB 2.0
{: .no_toc }
<h6> Data modyfikacji: 06.08.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

InfluxDB to wysokowydajny magazyn danych stworzony specjalnie dla danych szeregów czasowych. Umożliwia przyjmowanie, kompresję i zapytania w czasie rzeczywistym o wysokiej przepustowości.
Influx jest zoptymalizowany pod kątem szybkiego, wysoce dostępnego przechowywania i wyszukiwania danych szeregów czasowych w takich dziedzinach jak monitorowanie operacji, zbieranie danych z czujników IoT i analiza w czasie rzeczywistym.
InfluxDB współpracuje z InfluxQL, językiem zapytań podobnym do SQL, do interakcji z danymi. InfluxQL obsługuje wyrażenia arytmetyczne i funkcje specyficzne dla szeregów czasowych, aby przyspieszyć przetwarzanie danych.
InfluxDB może obsłużyć miliony punktów danych na sekundę. Zdarza się, że praca z taką ilością danych przez długi czas może prowadzić do problemów z przechowywaniem. InfluxDB automatycznie kompaktuje dane, w celu minimalizacji przestrzeni dyskowej. InfluxDB posiada dwie funkcje, które pomagają zautomatyzować procesy próbkowania i wygasania danych – ciągłe zapytania (Continuous Queries) i zasady przechowania (Retention Policies), które określają jak długo dane mają być przechowywane.

<div class="code-example" markdown="1" style="background: rgba(255, 238, 140, 0.8)">

INFO
{: .label .label-purple }

Obecnie TwinCAT 3 współpracuje wyłącznie z InfluxDB w wersjach **1.x i 2.x**, poniższy poradnik dotyczy tylko i wyłącznie współpracy z InfluxDB **2.x**.
 
</div>

# Instalacja InfluxDB 2.0

Krokiem pierwszym jest aktywacja serwera InfluxDB2. Aby to zrobić należy wejść na stronę [InfluxData.](https://docs.influxdata.com/influxdb/v2/install/?t=Windows)
<br>
Następnie wybieramy odpowiednia wersję **(1)** oraz odpowiedni system operacyjny **(2)**:

![inf1](https://ba-pl.github.io/wiki/assets/images/influx/inf1.png "inf1")

i pobieramy **(3)** InfluxDB v2:

![inf2](https://ba-pl.github.io/wiki/assets/images/influx/inf2.png "inf2")

Jeżeli plik pobrał się poprawnie, należy stworzyć folder **InfluxData**, w folderze Program Files, i tam wypakować pobrany plik. Widok po wypakowaniu:

![inf3](https://ba-pl.github.io/wiki/assets/images/influx/inf3.png "inf3")

Następnie należy skonfigurować zmienne środowiskowe systemu, abyśmy mogli z dowolnego miejsca w systemie korzystać z influxd:

![inf4](https://ba-pl.github.io/wiki/assets/images/influx/inf4.png "inf4")

![inf5](https://ba-pl.github.io/wiki/assets/images/influx/inf5.png "inf5")

![inf6](https://ba-pl.github.io/wiki/assets/images/influx/inf6.png "inf6")

W tym momencie można sprawdzić, czy wszystko zostało poprawnie skonfigurowane. Wchodzimy w wiersz poleceń, i wpisujemy komendę:
```
influxd version
```

Powinniśmy otrzymać rezultat jak poniżej: 

![inf7](https://ba-pl.github.io/wiki/assets/images/influx/inf7.png "inf7")

Następnie aktywujemy serwer przy pomocy komendy *influxd*: 

```
influxd 
```

![inf8](https://ba-pl.github.io/wiki/assets/images/influx/inf8.png "inf8")

Komenda ta na tym etapie powinna pozostać uruchomiona tak długo aż jej nie wyłączymy (Ctrl+c).

## Interfejs InfluxDB2

Jeśli wykonaliśmy wszystkie poprzednie kroki poprawnie, w przeglądarce wpisujemy adres **localhost:8086**:

![inf8a](https://ba-pl.github.io/wiki/assets/images/influx/inf8a.png "inf8a")

Następnie przechodzimy proces zakładania konta (zapamietując podane dane, potem będą potrzebne).
Finalnie powinniśmy uzyskać okno jak poniżej, gdzie powinna się wyświetlać nazwa konta, organizacji oraz bucketu które stworzyliśmy:

![inf9](https://ba-pl.github.io/wiki/assets/images/influx/inf9.png "inf9")

# Projekt TwinCAT

Aby korzystać z InfluxDB, należy na komputerze z narzędziem inżynierskim oraz na sterowniku, który będzie łączył się bazą, zainstalować dodatek TF6420.
<br> 
Instrukcja instalacji dodatków znajduje się [tutaj.](https://ba-pl.github.io/wiki/docs/TwinCAT%203/Instalacja/Instalacja/#modyfikowanie-instalacji)
<br>
Po zainstalowaniu dodatku tworzymy nowy projekt *Empty TwinCAT Database Server Project*:

![inf10](https://ba-pl.github.io/wiki/assets/images/influx/inf10.png "inf10")

Następnie dodajemy do drzewka projektu *TwinCAT XAE Project* (standardowy projekt).

![inf11](https://ba-pl.github.io/wiki/assets/images/influx/inf11.png "inf11")

![inf12](https://ba-pl.github.io/wiki/assets/images/influx/inf12.png "inf12")

W następnym kroku generujemy licencję trial dla dodatku TF6420 (można to zrobić zarówno dla sterownika jak i dla swojego komputera, docelowo licenjca będzie potrzebna jedynie na sterowniku).

![inf13](https://ba-pl.github.io/wiki/assets/images/influx/inf13.png "inf13")

![inf14](https://ba-pl.github.io/wiki/assets/images/influx/inf14.png "inf14")

Aby licencja została odczytana, należy przerestartować Runtime TwinCAT.
<br>
<br>
W ustawieniach TcDbServer wybieramy urządzenie na którym ma znaleźć się serwer **(1)** TF6420 (czyli urządzenie, które ma się łączyć do bazy z poziomu TwinCATa, nie musi być to to samo urządzenie na ktróym znajduje się baza).
<br>
Jeśli na wybranym urządzeniu jest zainstalowany dodatek i aktywna licencja, na fioletowej ikonie powienin pojawiać się zielony kwadracik **(2)**.

![inf16a](https://ba-pl.github.io/wiki/assets/images/influx/inf16a.png "inf16a")

Teraz dodajemy nową bazę:

![inf17](https://ba-pl.github.io/wiki/assets/images/influx/inf17.png "inf17")

W oknie, które się pojawi, dokonujemy odpowiednich ustawień: 

![inf18](https://ba-pl.github.io/wiki/assets/images/influx/inf18.png "inf18")

Wchodzimy w nową bazę danych, i wybieramy:
<br>
1. InfluxDB2 jako typ naszej bazy danych.
2. Server, to jest *localhost:8086*, organizację taka jaką uprzednio utworzyliśmy (w tym przypadku *Test5*), oraz wcześniej utworzony Bucket (w tym wypadku *TestBucket1*).
3. Wpisujemy nazwę użytkownika, oraz hasło takie jakie utworzyliśmy wcześniej.
4. Klikamy check.
<br>
<br>
Po kliknięciu **Check** powinno pokazać się okno:

![inf19](https://ba-pl.github.io/wiki/assets/images/influx/inf19.png "inf19")

Przetestujmy teraz jeszcz jedną opcję - zmieniamy nazwę **(1)** Bucket (na nieistniejącą) a następnie klikamy **Create** (2). 

![inf20](https://ba-pl.github.io/wiki/assets/images/influx/inf20.png "inf20")

W rezultacie powinien pojawić się komunikat:

![inf21](https://ba-pl.github.io/wiki/assets/images/influx/inf21.png "inf21")

Nowo utworzony Bucket powinien się również pojawić na stronie serwera InfluxDB:

![inf22](https://ba-pl.github.io/wiki/assets/images/influx/inf22.png "inf22")

Wracamy do TwinCATa i dodajemy nową grupę Autlogowania:

![inf23](https://ba-pl.github.io/wiki/assets/images/influx/inf23.png "inf23")

W ustawieniach grupy sprawdzamy, czy tryb zapisu jest ustawiony na *Append*:

![inf24](https://ba-pl.github.io/wiki/assets/images/influx/inf24.png "inf24")

W zakładce *AdsDevice* wybieramy urządzenie, z którego będziemy brali dane do logowania do bazy, a następnie tworzymy testowy program PLC, który będzie generował przykładowe dane.

![inf25](https://ba-pl.github.io/wiki/assets/images/influx/inf25.png "inf25")

Kod przykładowego programu:

```
// Deklaracja Danych
PROGRAM MAIN
VAR
rand  : DRAND;  // Instance of DRAND function block 
nrand : LREAL;    
fValue1 : LREAL; 
fValue2 : LREAL; 
fValue3 : LREAL; 
END_VAR

// PROGRAM
rand(); 
nrand := rand.Num; 
fValue1 := 900  + 500 * nrand; 
fValue2 := 1000 + 300 * nrand; 
fValue3 := 1100 + 100 * nrand; 

```

Program należy zbudować, aktywować konfigurację, oraz zalogować się do trybu online.
<br>
<br>
Teraz możemy wybrać konkretne symbole do logowania. W tym celu otwieramy *Target browser*:

![inf26](https://ba-pl.github.io/wiki/assets/images/influx/inf26.png "inf26")

i przeciągamy wybrane zmienne:

![inf27](https://ba-pl.github.io/wiki/assets/images/influx/inf27.png "inf27")

Po tych zmianach aktywujmey konfigurację projektu DbServer:

![inf28](https://ba-pl.github.io/wiki/assets/images/influx/inf28.png "inf28")

W lewm dolnym rogu powinien pojawić się rezultat:

![inf29](https://ba-pl.github.io/wiki/assets/images/influx/inf29.png "inf29")

W następnym kroku aktywujemy Przybornik dla dodatku Database Server: 

![inf30](https://ba-pl.github.io/wiki/assets/images/influx/inf30.png "inf30")

i otwieramy *SQL Query Editor*:

![inf31](https://ba-pl.github.io/wiki/assets/images/influx/inf31.png "inf31")

W *SQL Query Editor* ustawiamy odpowiedni Target **(1)** i wcześniej utworzony Bucket **(2)**:

![inf32](https://ba-pl.github.io/wiki/assets/images/influx/inf32.png "inf32")

Po zaznaczeniu naszego zasobnika, tworzymy tabelę **(1)** nadając jej nazwę **(2)** oraz uzupełniając pola jak na zdjeciu poniżej **(3)**, za pomocą kliknięcia PPM i opcji *Append Datafield*. Na koniec wykonujmey komedę **Create** (4):

![inf33](https://ba-pl.github.io/wiki/assets/images/influx/inf33.png "inf33")

Ponownie w lewym dolnym rogu narzędzia powinniśmy dostać informację zwrotną:

![inf34](https://ba-pl.github.io/wiki/assets/images/influx/inf34.png "inf34")

Tabela powinna się pojawić również po stronie serwra Influx:

![inf35](https://ba-pl.github.io/wiki/assets/images/influx/inf35.png "inf35")

Kolejnym krokiem jest przypisanie naszych zmiennych z programu, odpowiednim zmiennym z tabeli. 
Aby to zrobić wchodzimy w zakładkę DBTable **(1)**, wybieramy TableMode **(2)** jako CUSTOM, jako tabelę **(3)** naszą tabelę, i finalnie przypisujemy odpowiednie zmienne odpowiednim kolumnom w tabeli **(4)**. 
<br>
Kolumnie **_time** nic nie przypisujemy (powinna uzupełniać się automatycznie).

![inf36](https://ba-pl.github.io/wiki/assets/images/influx/inf36.png "inf36")

Następnie należy znów aktywować konfigurację TcDbServer **(1)**, po czym wejść w *Autolog Group View* **(2)**:

![inf37](https://ba-pl.github.io/wiki/assets/images/influx/inf37.png "inf37")

Teraz w okienku obserwacji grupy autologowania ustawiamy odpowiednie urządzenie **(1)** i logujemy się do obserwacji **(2)**. 
Powinna się w tym momencie wyświetlić nasza grupa autologowania. Jeśli wszystko przebiegło pomyślnie, klikamy w Autolog Start **(3)**. Jeśli licznik cykli jest większy niż zero oraz mamy komunikat *NoError* **(4)**, to oznacza ze nasze dane są logowane.

![inf38](https://ba-pl.github.io/wiki/assets/images/influx/inf38.png "inf38")

Pozostawiamy Autloggera włączonego i przechodzimy do interfejsu Influx. Kilkamy przycisk **Submit** (1) i możemy teraz manualnie odświeżać naszą bazę danych, przy pomocy przycisku odświeżania **(2)**.
Powinniśmy również zaobserwować odpowiedni wykres **(3)**.

![inf39](https://ba-pl.github.io/wiki/assets/images/influx/inf39.png "inf39")

Jeśli chcemy aby nasze dane aktualizowały się automatycznie, wchodzimy w zakładkę *Dashboards* i klikamy **Create Dashboard**:

![inf40](https://ba-pl.github.io/wiki/assets/images/influx/inf40.png "inf40")

Następnie wybieramy **Add Cell** i dokonujemy odpowiednich ustawień, na koniec zatwierdzamy je przyciskiem w prawym górnym rogu:

![inf41](https://ba-pl.github.io/wiki/assets/images/influx/inf41.png "inf41")

W naszym Dashboard możemy wybrać automatyczne odświeżanie, czas ukazany na wykresie, oraz wiele innych ustawień.

![inf42](https://ba-pl.github.io/wiki/assets/images/influx/inf42.png "inf42")

# Autostart logowania i bazy 
Teraz zajmiemy się skonfigurowaniem naszego urządzenia w taki sposób, aby logger uruchamiał się automatycznie wraz z uruchomieniem urządzenia.
<br>
<br>
Na początek potrzebujemy zmienić ustawienia grupy autologowania, musimy jednak najpierw zatrzymać aktywne logowanie:

![inf43](https://ba-pl.github.io/wiki/assets/images/influx/inf43.png "inf43")

W ustawieniach grupy **(1)** zmieniamy parametr *StartUp* **(2)** i aktywujemy konfigurację **(3)**:

![inf44](https://ba-pl.github.io/wiki/assets/images/influx/inf44.png "inf44")

Powyższe usatwienie spowoduje, że grupa bedzie się uruchamiać wraz ze startem TwinCAT do trybu Run Mode (zakładamy więc, że na sterowniku jest wgrany program, bootproject i stan TwinCAT po uruchomienu sterownika automatycznie ustawia się na Run Mode).

Ostatnim krokiem jest dodanie Influxd do usług (na urządzeniu, gdzie jest serwer bazy InfluxDB2).
<br>
Pobieramy w tym celu [NSSM](https://nssm.cc/download):

![inf45](https://ba-pl.github.io/wiki/assets/images/influx/inf45.png "inf45")

Rozpakowujemy pobrany zip pod dowolną ścieżką:

![inf46](https://ba-pl.github.io/wiki/assets/images/influx/inf46.png "inf46")

Uruchamiamy wiersz poleceń i przechodzimy do tej ścieżki i odpowiedniego katalogu w zależności od tego czy nasza platforma jest 32 czy 64 bitowa:

![inf47](https://ba-pl.github.io/wiki/assets/images/influx/inf47.png "inf47")

Sprawdzamy działanie nssm komedą:

```
nssm --help
```

![inf48](https://ba-pl.github.io/wiki/assets/images/influx/inf48.png "inf48")

Jeśli usługa działa, wywołujemy komendę:

```
nssm install InfluxDB
```

Powinno pojawić się okno które uzupełniamy odpowiednimi danymi:

![inf49](https://ba-pl.github.io/wiki/assets/images/influx/inf49.png "inf49")

Teraz sprawdźmy czy na pewno nasza usługa jest zainstalowana poprawnie.
<br>
Otwieramy okienko *Uruchom* i wpisujemy **services.msc**:

![inf49a](https://ba-pl.github.io/wiki/assets/images/influx/inf49a.png "inf49a")

Upewniamy się, że wszystkie konsole, na których mieliśmy włączony serwer InfluxDB są wyłączone i odnajdujemy wśród usług, usługę InfluxDB **(1)**, w którą wchodzimy.
Upewniamy się, że tryb uruchomienia jest automatyczny **(2)**, po czym kikamy Uruchom **(3)** i zatwierdzamy ustawienia **(4)**. 

![inf50](https://ba-pl.github.io/wiki/assets/images/influx/inf50.png "inf50")

Po restarcie komputera dane powinny logować się automatycznie.

