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

aaa








