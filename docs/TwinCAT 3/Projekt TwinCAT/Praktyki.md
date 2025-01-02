---
title: Dobre praktyki
parent: Projekt TwinCAT
nav_order: 5
layout: page
---


# Poradnik projektu
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

**Zbiór dobrych praktyk, które warto stosować przy tworzeniu projektu w TwinCAT 3**
<br>
Niniejszy dokument zawiera zbiór przydatnych informacji oraz dobrych praktyk w tworzeniu projektu w środowisku TwinCAT 3. Nie jest to kompletna instrukcja obsługi środowiska, a jedynie wskazówki (z drobnym objaśnieniem) skierowane do zaawansowanych użytkowników, w celu utworzenia swoistej „checklisty” działań koniecznych przy nowych projektach w TC3.
<br>
Ze względu na swoją charakterystykę instrukcja ta oparta jest na uproszczeniach oraz skrótach myślowych. W żadnym wypadku nie zastępuje ona dokumentacji technicznej środowiska oraz urządzeń. Zebrane w niej wskazówki dobrane są dla ogółu użytkowników i mogą nie być idealnym rozwiązaniem dla poszczególnych, dedykowanych dla konkretnego projektu, przypadków.

# Przed rozpoczęciem projektu

## (opcjonalne) Wgraj nowy, czysty obraz na urządzenie
Jeśli sterownik, kótry posiadasz, nie jest nowym urządzeniem, pracę warto rozpocząć od aktualizacji obrazu systemu operacyjnego. 
Obraz należy dostosować do posiadanego urządzenia (zgodnie z zamówieniem / dokumentem otrzymanym od odpowiedniego oddziału Beckhoff).
Procedura wymiany obrazu:
- dla [Windows CE](https://ba-pl.github.io/wiki/docs/IPC/Windows%20CE/Przywracanie%20ustawien/)
- dla [Windows 7 / 10](https://ba-pl.github.io/wiki/docs/IPC/BigWindows/bst/) 


## Uruchamiaj wszystko jako administrator
Większość oprogramowania firmy Beckhoff w celu poprawnego działania wymaga uruchamiania jako Administrator zarówno samych programów, jak i ich instalatorów – zagwarantuje to poprawne działanie bez błędów.

![por1](https://ba-pl.github.io/wiki/assets/images/praktyki/por1.png "por1")

## Zainstaluj/zaktualizuj TC i dodatki

Na PC powinna znajdować się wersja TwinCAT kompatybilna z PLC. Zawsze należy sprawdzić wersję TwinCAT na urządzeniu, a następnie użyć odpowiedniej wersji TC na komputerze w celu zaprogramowania go. Zapobiegnie to błędom spowodowanych brakiem kompatybilności bibliotek czy różnic w kompilatorach. Dla TC2 w celu uzyskania wcześniejszej wersji należy skontaktować się z działem wsparcia technicznego pod adresem **support@beckhoff.pl**. Dla TC3 można doinstalować **Remote Manager** który dostępny jest do pobrania ze strony Beckhoff.
<br>
Link do pobrania [Remote Manager.](https://www.beckhoff.com/en-en/support/download-finder/search-result/?download_group=97028330&download_item=689165722)

![por2](https://ba-pl.github.io/wiki/assets/images/praktyki/por2.png "por2")

## Skonfiguruj nazwę sieciową urządzenia (hostname), adresy IP, AmsNetId oraz pozostałe ustawienia sieciowe
Dokonuje się tego w Device Manager dla Windowsa CE lub w ustawieniach Windowsa dla pełnych Windowsów. Odpowiedni dobór ustawień sieciowych pozwala na połączenie go z komputerem z którego będzie programowany, a także (poprzez ustawienie odpowiedniego HostName) na jego prostą identyfikację w systemie, w którym znajduje się wiele urządzeń. Należy również zdecydować czy adres IP będzie statyczny, czy dynamiczny, w zależności od wymagań projektu. Należy pamiętać również, że zarówno adres AMS Net Id urządzenia, adres IP jak i nazwa sieciowa muszą być unikalne w sieci, aby uniknąć problemów z połączeniem.

![por3](https://ba-pl.github.io/wiki/assets/images/praktyki/por3.png "por3")

## Dodaj wyjątki firewall dla dodatkowych usług
Domyślnie w systemach Windows dedykowanych dla urządzeń firmy Beckhoff odblokowana jest tylko część portów. W celu umożliwienia komunikacji poprzez różne protokoły komunikacyjne należy dodać wyjątki w firewall (np. dla komunikacji poprzez Modbus TCP należy odblokować w firewall port 502.). Ustawień dokonuje się poprzez wybranie Control Panel --> CX Configuration --> Firewall na urządzeniach z Windows CE lub poprzez Panel Sterowania --> Ustawienia zapory ogniowej na Windowsach innych niż CE.

![por4](https://ba-pl.github.io/wiki/assets/images/praktyki/por4.png "por4")

![por5](https://ba-pl.github.io/wiki/assets/images/praktyki/por5.png "por5")

## Tworząc nowy projekt zadbaj o nazewnictwo już w trakcie tworzenia projektu
Podczas nazewnictwa plików projektu należy unikać używania polskich znaków, a także zadbać o odpowiednie nazewnictwo plików i zmiennych (np. programy rozpoczynać od P_\*, nazwy bloków funkcyjnych od FB_\*, przy nazewnictwie zmiennych stosować notację węgierską). Ułatwi to korzystanie z projektu, a także spowoduje, że będzie on bardziej czytelny.

![por6](https://ba-pl.github.io/wiki/assets/images/praktyki/por6.png "por6")

## Zablokuj wersję TwinCAT

Zawsze warto zablokować wersję TwinCAT i bibliotek w projekcie – opcja Pin Version – spowoduje to wyświetlenie warningu w przypadku próby otwarcia projektu w innej wersji TC niż ta, w której został stworzony projekt, a co za tym idzie, zapobiegnie kompilacji projektu w wersji narzędzia inżynierskiego niezgodnej z wersją TC na urządzeniu.

![por7](https://ba-pl.github.io/wiki/assets/images/praktyki/por7.png "por7")

## Zablokuj wersje bibliotek w projekcie
W celu zablokowania wersji biblioteki należy w drzewku projektu w zakładce References wybrać bibliotekę, a następnie w zakładce Properties, w polu Resolution wybrać żądaną wersję biblioteki (domyślnie ustawiona ‘\*’ oznacza „używaj najnowszej dostępnej wersji biblioteki”).

![por8](https://ba-pl.github.io/wiki/assets/images/praktyki/por8.png "por8")

## Zaktualizuj pliki opisowe urządzeń (esi dla EtherCAT i inne)
Są to pliki, które najczęściej dostępne są na stronie producenta. Umożliwiają one odczytywanie parametrów urządzenia w trybie online, a także ich modyfikację. Pozwalają one także na odczytanie odpowiednich indeksów oraz subindeksów dla poszczególnych parametrów urządzenia, umożliwiając odczytywanie i zapisywanie parametrów z poziomu programu PLC.
<br>
Aktualizacji dokonuje się w folderze **TwinCAT\3.1\Config\Io** wybierając folder z odpowiednim protokołem komunikacyjnym. Pliki te powinny być dostarczone przez producenta urządzenia.
<br>
Dla urządzeń EtherCAT w celu aktualizacji plików opisowych należy wybrać w górnyn menu zakładkę TwinCAT --> EtherCAT Devices --> Update Device Descriptions via ETG website.

![por9](https://ba-pl.github.io/wiki/assets/images/praktyki/por9.png "por9")

## Korzystaj z wersjonowania projektu (TFS, Git, TC Multiuser)
Wersjonowanie projektu pozwala zarówno na porównywanie ze sobą poszczególnych wersji (dokonanych zmian w projekcie, kodzie itp.), jak i na powrót do poprzednich wersji. Ułatwia to także pracę zespołową, gdy kilka osób pracuje nad jednym projektem. Można stosować takie repozytoria jak git czy TFS (Team Foundation Server). Od Buildu 4024.0 dostępne jest także stworzone przez Beckhoff narzędzie TwinCAT Multiuser, pozwalające na pracę wielu programistów nad jednym projektem.
<br>
Informacje dotyczące TwinCAT Multiuser (wraz z wprowadzeniem i opisem technologii) znajdują się [tutaj.](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_multiuser/6778303627.html)

## Oddzielny projekt dla Safety
Dla obsługi Safety należy stworzyć osobny projekt PLC, w którym będą realizowane wszystkie zadania automatyki bezpiecznej. Projekt powinien zostać utworzony zgodnie z wytycznymi do automatyki bezpiecznej. Wytyczne te dostępne są na stronie **beckhoff.com** lub po kontakcie z działem wsparcia technicznego pod adresem **support@beckhoff.pl**

![por10](https://ba-pl.github.io/wiki/assets/images/praktyki/por10.png "por10")

## Zainstaluj sterowniki real-time do karty sieciowej (jeśli wymaga tego wybrany typ komunikacji)
zależności od rodzaju używanego urządzenia, sterowniki real-time do karty sieciowej są zainstalowane domyślnie lub trzeba je doinstalować (z folderu TwinCAT/3.1/System/TcRteInstall.exe). Sterowniki z Windowsem CE oraz komputery z serii Embedded PC (czyli CX) mają sterowniki zainstalowane domyślnie. Dla urządzeń Industrial PC sterowniki można zamówić zainstalowane fabrycznie lub zainstalować je samemu. Umożliwiają one obsługę sieci EtherCAT – w przypadku ich braku niemożliwe będzie np. zeskanowanie za pomocą TwinCAT wyspy EK1100.

![por11](https://ba-pl.github.io/wiki/assets/images/praktyki/por11.png "por11")

## Wykorzystaj wielordzeniowość procesora (jeśli sterownik jest zbyt obciążony) 
Ważna jest opcja Read From Target w zakładce Real-Time w TwinCAT – pozwala na odczytanie liczby rdzeni w urządzeniu. W tym miejscu możemy również zoptymalizować wykorzystanie zasobów obliczeniowych procesora, przez przypisanie tasków odpowiednim rdzeniom, co pozwala na zmniejszenie zużycia Runtime’u sterownika.

![por12](https://ba-pl.github.io/wiki/assets/images/praktyki/por12.png "por12")

# W trakcie edycji projektu 

## Podziel EtherCAT na sync unit’y
W przypadku niekótrych urządzeń do ich prawidłowego działania konieczne jest ustawienie Sync Units. Więcej informacji znajdziesz [tutaj.](https://ba-pl.github.io/wiki/docs/EtherCAT/Sync%20Units/) 
## Monitoruj wydajność urządzenia (czas cyklu, taski, priorytety)
W zależności od typu realizowanej aplikacji, należy dopasować odpowiednie czasy cyklu, odpowiednie dopasowanie programów do tasków, a także odpowiednie priorytety dla poszczególnych tasków. Dla przykładu: taski komunikacyjne potrzebują krótkiego czasu cyklu (rzędu kilku ms) i wysokiego priorytetu, a taski wizualizacji mogą mieć dłuższy czas cyklu (rzędu nawet 100-200 ms) i niższy priorytet. Przekroczenie czasów cyklu może spowodować nieodświeżanie się odpowiednio magistrali, brak realizacji odczytu wejść/zapisu wyjść czy przedstawianie przez środowisko nieaktualnych danych. Należy badać zużycie procesora a także Exceed Counters (wartość powinna wynosić 0).

![por13](https://ba-pl.github.io/wiki/assets/images/praktyki/por13.png "por13")

![por14](https://ba-pl.github.io/wiki/assets/images/praktyki/por14.png "por14")

## Monitoruj wartości zmiennych w projekcie
Do monitorowania wartości zmiennych w TwinCAT 3 można wykorzystać trzy narzędzia: Watch, ADS Symbol Watch lub TwinCAT 3 Scope View. Pierwszy i drugi z nich jest zintegrowany z projektem PLC, trzeci wymaga oddzielnego projektu.
<br>
Aby móc odczytywać zmienne przy pomocy Watch, należy w trybie online wybrać zmienną PPM i wybrać opcję **Add Watch**. Można zrobić tak z kilkoma zmiennymi. Następnie po wywołaniu **PLC --> Windows --> Watch 1** będzie możliwy ich podgląd.

![por15](https://ba-pl.github.io/wiki/assets/images/praktyki/por15.png "por15")

Kolejnym sposobem na podgląd wartości zmiennej jest ADS Symbol Watch. Służy on do podglądu wartości zmiennych z modułów obecnych w komunikacji sprzętowej, np. zmiennej WcState modułów ELxxxx. Aby go dodać, należy rozwinąć drzewko odpowiedniego modułu, wybrać interesującą nas zmienną, następnie kliknąć na nią PPM i wybrać **Add to Watch**. Można zrobić tak z wieloma zmiennymi. Następnie po wywołaniu **View --> Other Windows --> ADS Symbol Watch** będzie możliwy ich podgląd (**Path** wskazuje na konkretny kanał lub ogólnie na moduł, z którego wybrana została zmienna).

![por16](https://ba-pl.github.io/wiki/assets/images/praktyki/por16.png "por16")

Najbardziej zaawansowanym narzędziem do monitorowania wartości zmiennych niewątpliwie jest **TwinCAT 3 Scope View**. Obsługa jego wymaga jednak dodania oddzielnego projektu do rozwiązania. Aby narysować zmienne przy pomocy TwinCAT 3 Scope View:
1. Dodaj nowy projekt TwinCAT Measurement YT Scope
2. Wybierz TwinCAT -> Target Browser -> Target Browser z górnego menu
3. Odszukaj “swój” sterownik, wybierz port na który wgrany jest program PLC (zwykle 851)
4. Wybierz zmienne które chcesz narysować poprzez dwukrotne kliknięcie LPM na nich
5. Dodaj je do odpowiednich wykresów i osi metodą drag & drop z Data Pool

![por17](https://ba-pl.github.io/wiki/assets/images/praktyki/por17.png "por17")

Następnie uruchom nagrywanie z górnego przybornika (w tym samym przyborniku można zatrzymać nagrywanie). Jeśli przybornik nie pojawił się, można włączyć go w **View --> Toolbars --> TwinCAT 3 Scope.** 
<br>
**Uwaga!** Przybornik jest aktywny w momencie, w którym focus w drzewku projektu znajduje się na Measurement Project.

![por18](https://ba-pl.github.io/wiki/assets/images/praktyki/por18.png "por18")

## Sprawdź błędy za pomocą funkcji Checks

Dokładny opis użycia funkcji Checks znajdziesz [tutaj.]()
<br>
Po użyciu i ostatecznym wgraniu projektu na sterownik, usuń z niego funkcje Checks.

# Na koniec projektu 

## Wgraj wymagane licencje
Najwygodnieszym sposobem na posiadanie licencji jest posiadanie ich na module EL6070 lub na dongle USB, uzyskujemy wtedy pełną przenoszalność licencji pomiędzy urządzeniami. Instrukcja licencjonowania dostępna jest [tutaj.](https://ba-pl.github.io/wiki/docs/TwinCAT%203/Licencjonowanie/)

![por19](https://ba-pl.github.io/wiki/assets/images/praktyki/por19.png "por19")

## Rebuild solution!
Po zakończeniu programowania najlepiej zamiast Build Solution wybrać opcję Rebuild Solution. Powoduje to wyczyszczenie projektu, a następnie zbudowanie go od nowa. Gwarantuje to poprawną kompilację kodu.

![por20](https://ba-pl.github.io/wiki/assets/images/praktyki/por20.png "por20")

## Dodaj boot project oraz wgraj kod źródłowy
Jeżeli chcemy, aby po uruchomieniu się urządzenia program uruchamiał się samoczynnie (czyli np. po chwilowym zaniku napięcia powraca do normalnej pracy bez naszej ingerencji) należy aktywować boot project na urządzeniu. Kod źródłowy programu PLC domyślnie jest wgrywany, jednakże jest opcja odznaczenia wgrywania kodu np. w celu jego zabezpieczenia.

![por21](https://ba-pl.github.io/wiki/assets/images/praktyki/por21.png "por21")

## Wybierz tryb uruchamiania TwinCAT po restarcie sterownika
Jeżeli chcemy, aby sterownik uruchamiał się automatycznie z działającym programem należy pamiętać o zaznaczeniu, by TwinCAT na nim uruchamiał się w trybie RUN (opcja ustawiona domyślnie na sterownikach z Windows CE, na urządzeniach z pełnym Windowsem należy tę opcję aktywować). W tej samej zakładce można również ustawić automatyczne logowanie do systemu Windows na wybrane przez nas konto.

![por22](https://ba-pl.github.io/wiki/assets/images/praktyki/por22.png "por22")

## Zabezpiecz konto administratora urządzenia hasłem (innym niż „1”)
Domyślne hasło dla nowych urządzeń to „1”. Hasło to należy po zakupie sterownika zmienić w celu zapewnienia bezpieczeństwa. Przy zalogowaniu do Device Managera poprzez stronę internetową (https://IP_sterownika/config) pojawi się okno z ostrzeżeniem o domyslnych użytkowniku i haśle – przechodząc przez ten wizard można zmienić domyślne hasło dla urządzenia.

![por23](https://ba-pl.github.io/wiki/assets/images/praktyki/por23.png "por23")

## Utwórz kopię zapasową projektu oraz całego urządzenia
[Beckhoff Service Tool](https://ba-pl.github.io/wiki/docs/IPC/BigWindows/bst/) pozwala na utworzenie kopii zapasowej obrazu całego systemu (dla urządzeń z Windowsem 7 lub 10). Dla sterowników z Windowsem CE wystarczy przekopiować zawartość karty pamięci na komputer. Backup taki jest potem łatwo przywrócić. Warto także utworzyć kopię zapasową projektu nad którym pracujemy, zwłaszcza gdy chcemy dokonywać na nim zmian. Można np. [spakować projekt do archiwum](https://ba-pl.github.io/wiki/docs/TwinCAT%203/Projekt%20TwinCAT/archive/)

# Dodatkowe Informacje

## Zainstaluj Offline Information System
W sytuacjach, w których podczas obsługi oprogramowania TwinCAT 3 nie mamy dostępu do internetu, warto zadbać o to, aby mieć dostęp do pomocy (w tym wypadku najlepszą „skarbnicą wiedzy” jest strona infosys.beckhoff.com). Pomoc tę (co jakiś czas aktualizowaną) można pobrać ze strony **Beckhoff** a następnie zaznaczyć, by po wybraniu funkcji czy bloku funkcyjnego i wciśnięciu F1 uruchamiał się asystent pomocowy Windowsa w którym dostępna będzie offline’owa wersja Infosys.

![por24](https://ba-pl.github.io/wiki/assets/images/praktyki/por24.png "por24")

## Folder wewnątrz projektu do własnych grafik w wizualizacji
Jeśli do wizualizacji dodajemy własne grafiki, dobrze jest zadbać o to, by folder z nimi znalazł się w folderze projektu – spowoduje to sytuację, w której grafiki będą przesłane do urządzenia wraz z projektem, co wykluczy problemy z ich wyświetlaniem. Do nazw grafik nie należy używać polskich znaków.


