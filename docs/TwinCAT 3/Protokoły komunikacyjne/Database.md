---
title: Database Server
parent: Protokoły komunikacyjne
nav_order: 5
layout: page
---


# Database Server
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

TwinCAT Database Server pozwala na komunikację TwinCAT’a z różnymi systemami bazodanowymi np. Microsoft SQL Server, MySQL itp. Przy prostych zadaniach wystarczy korzystać z graficznego konfiguratora serwera w TwinCAT. Konfigurator zapisuje konfigurację do pliku XML, który może być przesłany na urządzenie docelowe. Bardziej zaawansowane aplikacje mogą wymagać wykorzystania bloków funkcyjnych z biblioteki Tc3_Database. TwinCAT Database Server umożliwia też korzystanie z komend SQL z poziomu programu PLC.

Konfiguracji TwinCAT Database Server można więc dokonać w trzech trybach:
- Konfiguracji - tylko konfiguracja w TwinCAT, bez implementacji kodu PLC
- PLC Expert – realizacja komend SQL generowanych poprzez bloki funkcyjne
- SQL Expert – komendy SQL tworzone są w programie PLC i przesyłane są w całości do bazy

Tryby można ze sobą mieszać.

W wielu przypadkach, oprócz konfiguracji TwinCAT Database Server, wymagana jest instalacja systemu zarządzania wybraną bazą danych. W tej instrukcji skorzystano z SQLite, która jest plikową bazą danych i nie wymaga instalacji dodatkowego oprogramowania.
Aby rozpocząć pracę z TwinCAT Database Server, konieczne jest zainstalowanie dodatku Tc3_DatabaseServer TF6420 na komputerze programisty oraz na komputerze, który będzie serwerem (jeśli nie będą to te same urządzenia). Dodatek TF6420 dostępny jest pod adresem: [TF6420](https://www.beckhoff.com/pl-pl/products/automation/twincat/tfxxxx-twincat-3-functions/tf6xxx-tc3-connectivity/tf6420.html)

Uwaga! Należy pamiętać o uruchamianiu instalatora Tc3_DatabaseServer jako administrator.

Po instalacji dodatku należy wygenerować licencję, na czas testów może być to licencja siedmiodniowa: [Licencja testowa](https://infosys.beckhoff.com/english.php?content=../content/1033/tf6420_tc3_database_server/262609675.html)

# Tryb Konfiguracji
Tryb konfiguracji umożliwia zestawienie połączenia z serwerem bazy danych (może być to serwer lokalny), utworzenie tabel w bazie oraz wybranie zmiennych z programu PLC, które mogą być automatycznie logowane do bazy. Aby wykonać powyższe kroki, należy w środowisku TwinCAT dodać nowy projekt Empty TwinCAT Database Server Project.

![db1](https://ba-pl.github.io/wiki/assets/images/db1.png "db1")

## Ustawienia serwera 
W pierwszym oknie wskazujemy urządzenie, które jest serwerem bazy danych **(1)**, możemy również ustawić tryb i lokalizację zapisu logów **(2)** bazy (jest to rejestr błędów, który w przypadku problemów, pomoże nam namierzyć ich źródło).

![db2](https://ba-pl.github.io/wiki/assets/images/db2.png "db2")

Opcja Impersonate user **(3)** jest zaznaczana, jeżeli jest potrzeba, żeby zrealizować połączenie przez sieć z plikową bazą danych taką jak Access lub SQL Compact.
Pozostałe ustawienia zmieniamy w razie potrzeby.

## Dodawanie bazy 
W celu dodania bazy należy kliknąć **PPM na TcDbServer** i wybrać **Add -> Add New Database (1)**. Pojawi się okno do wskazania rodzaju bazy danych **(2)**, z którą ma zachodzić komunikacja poprzez TwinCAT Database Server.

![db3](https://ba-pl.github.io/wiki/assets/images/db3.png "db3")

Z listy rozwijanej można wybrać jeden z obsługiwanych typów baz danych albo typ *ODBC*, gdzie jest możliwość ręcznego utworzenia *Connection String* do połączenia z bazami spoza listy (więcej informacji dostępne w dokumentacji), zgodnie ze standardem SQL.
DBID nadawane jest automatycznie przy każdej dodanej bazie i umożliwia rozróżnienie w konfiguracji i programie PLC wielu baz w jednym projekcie.

![db4](https://ba-pl.github.io/wiki/assets/images/db4.png "db4")

W przykładzie wybrano bazę **SQLite (1)**. W polu **SQLite Database File (2)** należy podać ścieżkę do pliku bazy, o rozszerzeniu **.db** (plik nie musi na tym etapie istnieć). Opcjonalnie można podać hasło po zaznaczeniu opcji **Authentication**. Jeżeli baza ta jeszcze nie istnieje, można ją utworzyć klikając **Create (3)**. Wybierając **Check** można sprawdzić, czy połączenie z bazą działa. W tym przypadku, po kliknięciu przycisku **Check**, powinien pojawić się komunikat pokazany poniżej.

![db5](https://ba-pl.github.io/wiki/assets/images/db5.png "db5")

Na dole okna widać automatycznie wygenerowany **Connection String (4)** zawierający wykonane ustawienia.

Po tak wykonanej konfiguracji, można dodatkowo wybrać w polu **FailoverDB** tak zwaną failover database, w której zachowane zostaną dane w razie napotkania błędu w trybie konfiguracyjnym. W razie rozłączenia z siecią, ta funkcja automatycznie zapewni, że dane nie przepadną i zostaną zapisane w innym miejscu.
Na zakończenie tego etapu należy zapisać zmiany i aktywować konfigurację projektu bazy danych (1) **(nie mylić z aktywowaniem konfiguracji PLC)**. Spowoduje to przesłanie pliku z konfiguracją na urządzenie docelowe wybrane w Server Settings.

![db6](https://ba-pl.github.io/wiki/assets/images/db6.png "db6")

Operacja ta nie wywołuje żadnego okna popup, należy obserwować okno błędów oraz okno **Output (Ctrl+Alt+O)**. Jeśli aktywacja się powiodła, powinna pojawić się informacja jak poniżej:

![db7](https://ba-pl.github.io/wiki/assets/images/db7.png "db7")

## SQL Query Editor

Sql Query Editor służy do ręcznego tworzenia/usuwania tabel oraz wpisów. Aby otworzyć okno edytora należy w pierwszej kolejności uaktywnić Toolbar dodatku Database Server:

![db8](https://ba-pl.github.io/wiki/assets/images/db8.png "db8")

Z dodanego Toolbara należy wybrać ikonę **Sql Querry Editor.**

![db9](https://ba-pl.github.io/wiki/assets/images/db9.png "db9")

W oknie jak obok należy wybrać serwer bazy danych (1), w razie potrzeby odświeżyć listę dostępnych baz (2) i na danej bazie wybrać komendę **Create Table.**

![db10](https://ba-pl.github.io/wiki/assets/images/db10.png "db10")

W przykładzie wykorzystano standardową strukturę tabeli. Aby utworzyć taką tabelę należy na pustym polu kliknąć **PPM** i wybrać opcję **Set Standard Table Schema.**

![db11](https://ba-pl.github.io/wiki/assets/images/db11.png "db11")

Kolumny zawierają kolejno: auto inkrementujące się ID wiersza, będące kluczem głównym; stempel czasowy zawierający aktualną datę i godzinę; nazwę zmiennej; wartość zmiennej. Pozostawienie ich bez zmian pozwoli na wykorzystanie standardowej metody logowania tabeli.
<br>
W kolejnym kroku należy wpisać nazwę tabeli **(1)**, automatycznie wygenerować komendę SQL **(2)** i ją wykonać **(3)**.

![db12](https://ba-pl.github.io/wiki/assets/images/db12.png "db12")

Jeżeli operacja się powiodła, na dolnym pasku statusowym środowiska TwinCAT, powinna się pojawić informacja:

![db13](https://ba-pl.github.io/wiki/assets/images/db13.png "db13")

Aby ręcznie wykonać wpis do bazy (w celach testowych) należy:
- kliknąć na utworzoną wcześniej tabelę PPM i wybrać Insert **(1)**
- wybrać opcję Get Table Schema **(2)** (powoduje automatyczne nadanie wartości Timestamp)
- z prawej strony w kolumnie Value wpisać wartości, które mają trafić do bazy **(3)**
- kliknąć komendę Create Query **(4)** (generowanie wyrażenia SQL)
- wykonać komendę opcją FB_SQLCommandEvt.Execute **(5)**

![db14](https://ba-pl.github.io/wiki/assets/images/db14.png "db14")

Ponownie w oknie **Output** powinna pojawić się informacja:

![db15](https://ba-pl.github.io/wiki/assets/images/db15.png "db15")

Aby odczytać wszystkie wartości znajdujące się w tabeli należy:
- kliknąć **PPM** na tabelę i wybierać opcję **Select (1)**
- wybrać kolejno opcje **Get Table Schema (2)** oraz **Create Query (3)** (utworzy się zapytanie do bazy o wszystkie dane z tabeli)
- wykonać komendę opcją **FB_SQLCommandEvt.ExecuteDataReturn (4)**

![db16](https://ba-pl.github.io/wiki/assets/images/db16.png "db16")

Aby wczytać tylko część wyników należałoby wykorzystać komendę WHERE zgodnie ze standardem języka SQL. Można również ręcznie wpisać całą komendę SQL, której wyniki wpisane zostaną do tabeli w dolnej części okna, trzeba jednak zadbać o zgodność typów zmiennych. Informacje o równoważnych typach zmiennych znajdują się pod linkiem: [Typy danych](https://infosys.beckhoff.com/english.php?content=../content/1033/tf6420_tc3_database_server/27021597872201739.html)

Ostatnia opcja - **Drop/Delete** - służy do usuwania wszystkich wpisów z tabeli **(Delete Datasets )** lub całej tabeli **(Drop Table)**. Aby usunąć tylko wybrane elementy należy ręcznie wpisać komendy zgodnie ze standardem języka SQL.

![db17](https://ba-pl.github.io/wiki/assets/images/db17.png "db17")

## AutoLog Group 
Docelowym krokiem trybu konfiguracyjnego jest dodanie i skonfigurowanie grupy autologowania. Grupa pozwala na automatyczne przekazywanie zmiennych z PLC do bazy albo na odwrót. Do jednej bazy danych może być dołączonych wiele różnych grup autologowania jednocześnie.

Aby rozpocząć pracę z grupami należy dodać je na wybranej bazie, klikając **PPM** i wybierając opcję **Add New AutlogGroup**:

![db18](https://ba-pl.github.io/wiki/assets/images/db18.png "db18")

Pierwsze okno, które się pojawi, to główne ustawienia trybu logowania:
- **Start Up** – wybranie opcji *Automatic* spowoduje uruchamianie logowania grupy razem ze startem TwinCATA, opcja *Manual* wymaga sterownia grupą z poziomu kodu PLC
- **Direction** - decyduje o kierunku przepływu danych. Wybranie *DeviceAsTarget* daje możliwość uaktualniania zmiennych PLC wartościami znajdującymi się w bazie. Wybranie *DeviceAsSource* pozwoli na zapisywanie zmiennych PLC na jeden z trzech sposobów, które konfiguruje się w polu *Write Mode*
- **Write Mode**:
	- UPDATE – wartości zmiennych w bazie będą aktualizowane (nadpisywane)
	- APPEND – wartości będą każdorazowo dopisane jako nowy wiersz
	- RINGBUFFER_TIME – określa maksymalny czas żywotności rekordu w sekundach, po upłynięciu tego czasu zostanie on usunięty. Czas określany jest w polu *Ringbuffer Parameter*
	- RINGBUFFER_COUNT – określa maksymalną ilość rekordów, po jej przekroczeniu kolejne rekordy będą usuwane, zaczynając od najstarszego. Ilość rekordów jest określana w polu *Ringbuffer Parameter*
- **Log Mode** – dane mogę być logowane w momencie zmiany wartości zmiennej (onChange) lub co określony w milisekundach w parametrze *Cyclic Time* czasu (opcja Cyclic)

![db19](https://ba-pl.github.io/wiki/assets/images/db19.png "db19")

W opisywanym przykładzie konfiguracja wygląda jak na zdjęciu powyżej (Start Up → Manual, Write Mode → Append).

### AdsDevice
W oknie AdsDevice, w polach **AMS NetID i ADS Device**, należy wybrać urządzenie, z którego pobierane będą zmienne. Zmieniając typ połączenia można zdecydować, czy zmienne mają być odszukiwane po adresie w pamięci **(Connection Type → byIGroupIOffset)** czy po nazwie **(Connection Type → bySymbolName)**. Ma to znaczenie w przypadku ręcznego dodawania symbolów zmiennych. Przy odszukiwaniu po adresie w trakcie deklarowania kolejnych zmiennych adresy mogą się zmieniać, co może potem powodować nieprzewidziane skutki. Przy odszukiwaniu po nazwie ten problem nie występuje.

![db20](https://ba-pl.github.io/wiki/assets/images/db20.png "db20")

### Symbols
Zakładka **Symbols** służy do skonfigurowania zmiennych, które mają być logowane do bazy. Można w tym celu skorzystać z narzędzia **Target Browser**. Program na sterowniku, z którego będą pobierane zmienne musi być uruchomiony.

![db21](https://ba-pl.github.io/wiki/assets/images/db21.png "db21")

Wybrane zmienne należy z okna **Target Browser** umieścić w zakładce **Symbols** metodą drag’n’drop.

![db22](https://ba-pl.github.io/wiki/assets/images/db22.png "db22")

### DBTable
Ostatnim krokiem jest dodanie standardowej tabeli w zakładce **DBTable**. Do tej tabeli wpisywane będą zmienne. W wersji **STANDARD** każda zmienna zostanie przekazana osobno. Można również wybrać tryb **CUSTOM**, który pozwoli na ręczne dopasowanie nazwy, typu i zawartości każdej kolumny, jednak trzeba pamiętać, aby były one zgodne z istniejącą tabelą.
W przykładzie wybrano **TableMode → Standard i Table → (Nazwa utworzonej wcześniej tabeli)**.

![db23](https://ba-pl.github.io/wiki/assets/images/db23.png "db23")

### Uruchomienie
**Uwaga!** Po zakończeniu konfiguracji należy zapisać wszystkie zmiany wybierając **Save All**, niezapisane zmiany nie zostaną wprowadzone. Następnie należy aktywować konfigurację projektu bazy danych.

![db24](https://ba-pl.github.io/wiki/assets/images/db24.png "db24")

Status serwera można sprawdzić obserwując kolor ikony w drzewie projektu.
Dozwolone kolory to:
- Zielony – gotowy do pracy
- Czerwony – nie można połączyć z TC3 DataBase Server
- Niebieski – brak licencji Tc3_Database
- Fioletowy – Grupa autologowania jest w trakcie pracy

![db25](https://ba-pl.github.io/wiki/assets/images/db25.png "db25")

Jeżeli aktywacja konfiguracji przebiegła pomyślnie, można uruchomić grupę autologowania. Należy w tym celu otworzyć okno **AutoLog Viewer** zieloną ikoną z Toolbara dodatku Database Server:

![db26](https://ba-pl.github.io/wiki/assets/images/db26.png "db26")

W opisywanym przykładzie zarządzanie grupą autologowania zostało ustawione na tryb Manualny. Pozwala to na ręcznie uruchamianie/zatrzymywanie grupy. W oknie **AutoLog Viewer** najpierw należy wybrać Serwer **(1)** a następnie uruchomić logowanie **(2)**. Ewentualne błędy pojawią się w kolumnach *HRESULT* i Error Type.

![db27](https://ba-pl.github.io/wiki/assets/images/db27.png "db27")

Jeśli kilka cykli zaloguje się poprawnie, grupę można zatrzymać. Aby odczytać dane wpisane do bazy, należy wykonać komendę **Select**, jak opisano to we wcześniejszej części instrukcji (w oknie **Sql Query Editor**).
Przykład odczytanych danych:

![db28](https://ba-pl.github.io/wiki/assets/images/db28.png "db28")

Jeśli grupa autologowania działa poprawnie, można zmienić tryb jej uruchamiania na AutoStart i aktywować konfigurację projektu baz danych. Od tej pory skonfigurowane zmienne będą logować się do bazy automatycznie, razem z uruchomieniem się Run-Time’u TwinCATa.

![db29](https://ba-pl.github.io/wiki/assets/images/db29.png "db29")

# Tryb Konfiguracji - przykład niestandardowej tabeli 
W trybie konfiguracyjnym można skorzystać z opcji stworzenia własnej tabeli. W tym celu należy postępować jak w rozdziale 2.3, ale na etapie tworzenia tabeli, zamiast opcji *Set Standard Table Schema*, należy dodać odpowiednie pola opcją **Append Datafield**. W każdej tabeli niezbędny jest klucz główny, dlatego zaleca się dodanie ID w pierwszej kolumnie. Wybranie typu *BigInt* oraz własności *IDENTITY(1,1)* spowoduje, że ID będzie miało wartość początkową równą 1 i będzie inkrementowane automatycznie przez serwer. Stempel czasowy można uzyskać poprzez dodanie kolumny *Timestamp* typu *DateTime*. Analogicznie należy dodać kolejne kolumny.

![db30](https://ba-pl.github.io/wiki/assets/images/db30.png "db30")

## Rzutowanie zmiennych 
Okno **SQL Query Editor** pozwala na eksport utworzonej tabeli do struktury zmiennych dla programu PLC. Dzięki temu zyskujemy pewność, że przygotowane do logowania w programie PLC zmienne będą odpowiedniego typu. Aby wykonać eksport należy, po utworzeniu tabeli, należy:
- kliknąć na tabelę PPM i wybrać opcję **Select (1)**
- wykonać komendę **Get Table Schema (2)**
- wykonać eksport opcją **Export Tablestruct to TwinCAT3 DUT (3)**

![db31](https://ba-pl.github.io/wiki/assets/images/db31.png "db31")

Plik można zapisać w dowolnym miejscu na dysku komputera.

Aby zaimportować strukturę do projektu PLC należy kliknąć **PPM** na folder **DUT** i wybrać opcję **Import PLCopenXML…** Po zaimportowaniu wcześniej wyeksportowanej struktury widać, że typy zmiennych SQL zostały zamienione na odpowiadające im typy zmiennych PLC.
Następnie należy strukturę takiego typu zadeklarować w programie PLC oraz wgrać i uruchomić program.

![db32](https://ba-pl.github.io/wiki/assets/images/db32.png "db32")

## Niestandardowa grupa autologowania
Aby uruchomić niestandardową grupę autologowania należy skonfigurować zakładki **AutoLogGroup** oraz **AdsDevice** jak opisano w rozdziale 2.4. W zakładce **Symbols**, korzystając z okna **Target Browser**, należy umieścić zawartość opisanej wyżej struktury z programu PLC (program PLC musi być uruchomiony):

![db33](https://ba-pl.github.io/wiki/assets/images/db33.png "db33")

W zakładce **DBTable** w polu **TableMode** należy wybrać opcję **CUSTOM**, a w polu **Tables** wybrać odpowiednią tabelę. Następnie trzeba ręcznie wykonać powiązanie zmiennych:

![db34](https://ba-pl.github.io/wiki/assets/images/db34.png "db34")

Jeżeli grupa ma za zadanie zapisywać wartości do bazy to zmienne *ID* oraz *Timestamp* należy odpowiednio ustawić jako *<AutoID>* oraz *<Data Timestamps>*. Jeżeli jednak grupa będzie miała za zadanie odczytywanie zmiennych z bazy to *<AutoID>* oraz *<Data Timestamps>* należy zastąpić zmiennymi ze struktury, do których wpisywane będą odczytywane wartości.

Na koniec należy wykonać zapis (Save All) i aktywację konfiguracji projektu baz danych.

# Sterowanie grupami autologowania poprzez PLC

W programie PLC możemy w dowolnym momencie sprawdzić status lub rozpocząć/przerwać pracę każdej grupy autologowania. Służy do tego blok FB_PLCDBAutoLogEvt oraz metody RunOnce(), Start(), Status(), Stop(). Aby rozróżnić konkretne bazy oraz grupy potrzebujemy DBID otrzymane przy konfiguracji serwera bazy oraz AutoLogGrpID otrzymane podczas dodawania grupy. Pod [linkiem](HTTPS://INFOSYS.BECKHOFF.COM/ENGLISH.PHP?CONTENT=../CONTENT/1033/TF6420_TC3_DATABASE_SERVER/2674373259.HTML) więcej informacji.

# Tryb PLC Expert
Tryb ten ma podobne możliwości oraz wymagania, co tryb konfiguracji. Jedyną różnicą jest wprowadzanie wszystkich wyżej wymienionych ustawień bezpośrednio w kodzie PLC. Wszystkie bloki funkcyjne zawierają metody pozwalające na wykonanie wielu różnych komend SQL w zależności od zapotrzebowania. Skrócone opisy możliwych do zrealizowania czynności:
- FB_ConfigTcDBSrv – Wprowadza zmiany w pliku konfiguracyjnym CurrentConfigDataBase.xml. Plik ten zawiera informacje o całej konfiguracji Tc3_DataBase_Server na danym komputerze i znajduje się domyślnie w folderze C:\TwinCAT\3.1\Boot. Za pomocą tego bloku możemy więc dowolnie odczytywać, dodawać oraz usuwać kolejne bazy danych oraz grupy autologowania zmiennych.
- FB_PLCDBAutoLogEvt – Blok ten pozwala na uruchamianie, zatrzymywanie oraz sprawdzanie statusu grup autologowania.
- FB_PLCDBCreateEvt – Pozwala na utworzenie pliku bazy danych określonego wcześniej w pliku konfiguracyjnym, oraz nowych tabeli do dowolnych baz danych.
- FB_PLCDBReadEvt – Działa w dwóch trybach. Pierwszy pozwala na odczytywanie dowolnej ilości rejestrów z tabeli w formacie zgodnym ze standardową tabelą Tc3_Database. Drugi tryb pozwala na odczytanie dowolnej tabeli, ale wymaga załączenia struktury zgodnej z kolejnymi kolumnami tabeli.
- FB_PLCDBWriteEvt – Pozwala na zapis wartości w trzech trybach. Pierwsze dwa zapisują wartości zmiennych do tabeli zgodnej ze standardową tabelą Tc3_Database, odpowiednio po nazwie zmiennej oraz po adresie ADS. Trzeci zapisuje w tabeli zawartość dowolnej zgodnej z nią struktury.
- FB_PLCDBCmdEvt – Pozwala na wykonywanie bardziej zaawansowanych funkcji odczytu oraz zapisu. Możliwe jest między innymi odczytywanie/zapisywanie tylko części kolumn z danego wpisu oraz selektywne odczytywanie rekordów przy pomocy komendy WHERE. Blok ten wysyła do serwera bazy danych bezpośrednią komendę SQL, którą możemy ręcznie zdefiniować. W komendzie tej mogą znajdować się placeholdery, które następnie ustawiamy na wejście bloku w formie tablicy struktur ST_ExpParameter. Dokładne możliwości tego bloku zależne są od obsługiwanych przez daną bazę komendami SQL.
<br>
Bloki te posiadają na wyjściu interfejs *ipTcResultEvent* zgodny z formułą *EventLoggera*. Pozwala to na bardzo łatwe przetwarzanie wiadomości o błędach. Przy pomocy biblioteki Tc3_EventLogger wiadomości z bloków można wpisać do zmiennych o specjalnym typie. Skorzystamy z tego w następnych przykładach.

Uruchamiamy program **P_PLCExpert_AutoLog**. Jest to przykład wykorzystania trybu PLC Expert zrobiony według schematu:

![db35](https://ba-pl.github.io/wiki/assets/images/db35.png "db35")

Zmiana zmiennej **bRun** na TRUE spowoduje uruchomienie grupy autologowania.

![db36](https://ba-pl.github.io/wiki/assets/images/db36.png "db36")

W *Sql Query Editor* można zaobserwować pojawiające się rekordy.

![db37](https://ba-pl.github.io/wiki/assets/images/db37.png "db37")

Kiedy zbierzemy zadowalającą nas liczbę danych, możemy zmienić **bStop** na TRUE, żeby zatrzymać grupę autologowania.
Możemy też zmienić okres co jaki aktualizowany jest status grupy za pomocą zmiennej **tAutoLogStatusCheckCycle**.
Status można podejrzeć w zmiennej **ipTcResult**.

![db38](https://ba-pl.github.io/wiki/assets/images/db38.png "db38")

