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

# Ustawienia serwera 
W pierwszym oknie wskazujemy urządzenie, które jest serwerem bazy danych **(1)**, możemy również ustawić tryb i lokalizację zapisu logów **(2)** bazy (jest to rejestr błędów, który w przypadku problemów, pomoże nam namierzyć ich źródło).

![db2](https://ba-pl.github.io/wiki/assets/images/db1.png "db2")

Opcja Impersonate user **(3)** jest zaznaczana, jeżeli jest potrzeba, żeby zrealizować połączenie przez sieć z plikową bazą danych taką jak Access lub SQL Compact.
Pozostałe ustawienia zmieniamy w razie potrzeby.








