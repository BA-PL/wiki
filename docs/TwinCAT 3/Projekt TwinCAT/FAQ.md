---
title: FAQ
parent: Projekt TwinCAT
nav_order: 7
layout: page
---


# FAQ
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Równoległa instalacja TwinCAT 2 i TwinCAT 3

Na chwilę obecną posiadanie na jednym komputerze zarówno narzędzia TwinCAT 2 jak i TwinCAT 3 jest możliwe tylko dla wersji TWinCAT 3 build 4024 lub niższy.
<br>
Dokładny opis postępowania znajduje się [tutaj.](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_installation/179471755.html)
## Przełączanie między środowiskami
Jeśli na komputerze zainstalowane są oba środowiska, przełączanie pomiędzy nimi następuje za pomocą narzędzia **TwinCAT Switch Runtime** (C:/TwinCAT/TwinCAT Switch Runtime/TwinCAT Switch Runtime.exe):

![faq1](https://ba-pl.github.io/wiki/assets/images/faq/faq1.png "faq1")

Po uruchomieniu narzędzie pojawi się następujące okno:

![faq2](https://ba-pl.github.io/wiki/assets/images/faq/faq2.png "faq2")

Na górze znajduje się informacja o aktywnym środowisku (1), poniżej znajduje się rozwijane menu (2), z którego należy wybrać na które środowisko chcemy się przełączyć. Po wybraniu należy kliknąć na przycisk Switch (3). Przycisk jest nieaktywny, gdy wszystkie instancje TwinCAT (4) są wyłączone. 
<br>
Jeśli chcemy uruchamiać oba środowisk z jednego miejsca (przy aktywnym Runtime TwinCAT 3), należy do lokalizacji **C:/TwinCAT/Target/StartMenuAdmin** wkleić skróty do aplikacji **TwinCAT PLC Control** oraz **TwinCAT System Manager**. Aplikacje te znajdują się odpowiednio w lokalizacjach:
- Dysk (C:) --> TwinCAT --> Plc --> TCatPlcCtrl.exe
- Dysk (C:) --> TwinCAT --> Io --> TCatSysManager.exe

![faq3](https://ba-pl.github.io/wiki/assets/images/faq/faq3.png "faq3")

# Projekt bootowalny, kod źródłowy na sterowniku
## Wgrywanie projektu bootowalnego

Jeśli chcemy Projekt PLC ustawić jako projekt bootowalny, należy po wgraniu programu na sterownik kliknąć PPM na nazwę projektu i wybrać Activate Boot Project (opcja aktywna po wylogowaniu z podglądu online programu), a następnie zaznaczyć opcję Autostart Boot Project. Na sterowniku w lokalizacji **…\TwinCAT\3.1\Boot\Plc** pojawi się plik, który w nazwie ma numer portu i jest typu AUTOSTART FILE.

![faq4](https://ba-pl.github.io/wiki/assets/images/faq/faq4.png "faq4")

W wersji TwinCAT 3.1.4024.0 i nowszych możliwość stworzenia projektu bootowalnego pojawia się podczas procedury aktywacji konfiguracji. W tym celu, podczas aktywacji konfiguracji, należy zaznaczyć poniższy checkbox:

![faq5](https://ba-pl.github.io/wiki/assets/images/faq/faq5.png "faq5")

## Wgrywanie kodu źródłowego

Kod źródłowy wgrywany jest automatycznie podczas wgrywania programu na urządzenie, jeśli zaznaczona jest opcja **Project Sources** w ustawieniach projektu PLC. Odznaczenie tej opcji dezaktywuje automatyczne wgrywanie kodu źródłowego.
<br>
**Uwaga!** Ustawienie to jest indywidualne dla każdego projektu.

![faq6](https://ba-pl.github.io/wiki/assets/images/faq/faq6.png "faq6")

## Pobieranie kodu źródłowego ze sterownika
Aby pobrać kod źródłowy oraz konfigurację z urządzenia należy wybrać **FILE --> Open --> Open Project From Target**. Następnie należy wybrać urządzenie, z którego chcemy pobrać program.

![faq7](https://ba-pl.github.io/wiki/assets/images/faq/faq7.png "faq7")

**UWAGA!**
Jeśli kod źródłowy nie został wgrany na urządzenie, to uda się pobrać jedynie konfigurację hardware oraz informację o programowych obszarach pamięci Input/Output, bez kodu programu PLC.
Następnie należy wskazać miejsce na dysku, w którym pobrany projekt zostanie zapisany. Po tych czynnościach kod źródłowy powinien się załadować w środowisku.

# Offline / online Beckhoff Information System
Korzystanie z Information System bez instalacji jest możliwe, jeśli komputer ma połączenie z siecią i włączone jest odpowiednie ustawienie w Visual Studio (Launch in Browser).

![faq8](https://ba-pl.github.io/wiki/assets/images/faq/faq8.png "faq8")

Przy takich ustawieniach, po zaznaczeniu nazwy bloczka i wybraniu na klawiaturze przycisku F1, opis danego bloczka otworzy nam się w wersji online Beckhoff Information System.

![faq9](https://ba-pl.github.io/wiki/assets/images/faq/faq9.png "faq9")

Aby móc lokalnie korzystać na komputerze z Beckhoff Information System bez dostępu do sieci należy pobrać plik instalacyjny ze strony [Beckhoff.](https://beckhoff.com]

![faq10](https://ba-pl.github.io/wiki/assets/images/faq/faq10.png "faq10")

Po zainstalowaniu Beckhoff Information System należy zmienić ustawienie wyświetlania pomocy na Launch in Help Viewer.

![faq11](https://ba-pl.github.io/wiki/assets/images/faq/faq11.png "faq11")

**Uwaga!** Zgodnie z instrukcją dostępną na stronie infosys.beckhoff.com aby zainstalować Offline Infosys w Microsoft Visual Studio należy najpierw umożliwić taką instalację. Link do instrukcji instalacji offline Infosys znajduje się [tutaj.](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_installation/72057594217401227.html) 

# Licencje testowe 
Środowisko TwinCAT 3 umożliwia aktywację darmowych licencji testowych. Są to licencje których ważność upływa po siedmiu dniach. Po upływie tego czasu licencję taką można ponownie aktywować. Listę licencji wykorzystywanych w projekcie można zobaczyć przechodząc w drzewie projektu do **SYSTEM --> License**. W przypadku gdy nie aktywowaliśmy jeszcze licencji jej status będzie widoczny jako **missing.**

![faq12](https://ba-pl.github.io/wiki/assets/images/faq/faq12.png "faq12")

Licencje testowe można wtedy aktywować wybierając opcję **7 Days Trial License…**

![faq13](https://ba-pl.github.io/wiki/assets/images/faq/faq13.png "faq13")

Należy wówczas przepisać kod zabezpieczający jaki pojawi nam się na ekranie oraz aktywować konfigurację. Status licencji ulegnie wówczas zmianie i będzie widoczna data wygaśnięcia licencji. Należy zwrócić uwagę, że licencje są aktywne tylko dla urządzenia, które jest w danym momencie wybrane jako Target System. W przypadku rozwijania projektu równolegle na różnych sterownikach należy licencję testowe aktywować dla każdego z osobna.

# Starsze wersje TwinCAT 3

W przypadku pobierania projektu ze sterownika może się zdarzyć, że został on przypięty do konkretnej wersji TwinCAT 3. W tym przypadku do otwarcia i modyfikacji kodu PLC niezbędna jest dokładnie ta wersja środowiska. Z pomocą przychodzi Remote Manager, który jest narzędziem do zarządzania różnymi wersjami środowiska w ramach jednego komputera inżynierskiego. W przypadku gdy korzystamy z TwinCAT 3.1.4024 to aby uzyskać dostęp do starszej wersji należy zgłosić się na **support@beckhoff.pl**
Dokładny opis Remote Managera znajduje się [tutaj.](https://download.beckhoff.com/download/document/automation/twincat3/TC3_Remote_Manager_en.pdf) 
<br>
Po zainstalowaniu dostępne wersje można wybrać na górnym pasku w środowisku:

![faq14](https://ba-pl.github.io/wiki/assets/images/faq/faq14.png "faq14")

W przypadku gdy korzystamy z wersji 3.1.4026 możliwe jest doinstalowanie 32-bitowego XAE Shella oraz Remote Managera do odpowiedniej wersji 3.1.4024 za pomocą Package Managera.

# Definiowanie własnych skrótów klawiszowych
Aby przyspieszyć pracę z TwinCAT 3 można zdefiniować własne skróty klawiszowe. Aby to zrobić należy z górnego menu wybrać **TOOLS --> Options**:

![faq15](https://ba-pl.github.io/wiki/assets/images/faq/faq15.png "faq15")

Następnie w oknie **Options** po lewej stronie należy wybrać pozycję **Keyboard(1)**. Z listy **komend(2)** należy wybrać tę do której chcemy utworzyć skrót klawiszowy. W polu **Use new shortcut in(3)** należy wybrać część środowiska w której skrót będzie obowiązywać (ustawienie Global odnosi się do całego środowiska). Następnie należy ustawić kursor w polu **Press shortcut keys(4)** i wcisnąć na klawiaturze wybrany przez siebie skrót klawiszowy. Wybór należy zatwierdzić przyciskiem **Assign(5).**

![faq16](https://ba-pl.github.io/wiki/assets/images/faq/faq16.png "faq16")

# Dostęp do zmiennych
## Zmienn globalne 
Aby dodać listę zmiennych globalnych należy w drzewie projektu PPM kliknąć na folder GVLs i wybrać **Add --> Global Variable List** a następnie nadać nazwę listy.

![faq17](https://ba-pl.github.io/wiki/assets/images/faq/faq17.png "faq17")

Pusta lista zmiennych globalnych wygląda następująco: 

![faq18](https://ba-pl.github.io/wiki/assets/images/faq/faq18.png "faq18")

Lista zmiennych globalnych posiada atrybut **’qualified_only’**. Po zadeklarowaniu zmiennej globalnej, odwołanie do niej w programie odbywa się poprzez podanie nazwy listy zmiennych globalnych i po kropce odwołanie do odpowiedniej zmiennej.

![faq19](https://ba-pl.github.io/wiki/assets/images/faq/faq19.png "faq19")

## Zmienne lokalne 
Zmienne lokalne deklaruje się pomiędzy znacznikami **VAR** oraz **END_VAR**. Odwołanie do takiej zmiennej odbywa się bezpośrednio poprzez jej nazwę, ale tylko w programie w którym została zadeklarowana.
<br>
Podczas używania zmiennej w programie można skorzystać z podpowiedzi wywoływanych za pomocą skrótu klawiszowego **Ctrl+Spacja**.

![faq20](https://ba-pl.github.io/wiki/assets/images/faq/faq20.png "faq20")

Podczas deklaracji zmiennych można je podzielić na sekcje, w celu uporządkowania danych. Dane sekcje można ukrywać/pokazywać.

![faq21](https://ba-pl.github.io/wiki/assets/images/faq/faq21.png "faq21")

# Wywołanie bloków funkcyjnych
## Wywołanie bloku funkcyjnego w języku ST
Aby wywołać blok funkcyjny w języku tekstowym najlepiej w polu kodu PLC wywołać Input Assistant poprzez wciśnięcie F2, następnie wyszukać interesujący nas blok i wybrać OK pamiętając o zaznaczeniu checkboxa **Insert with arguments**. Następnie należy zadeklarować blok funkcyjny i wtedy pojawi się on ze wszystkimi wejściami i wyjściami, które wystarczy przypisać (lub usunąć jeśli chcemy pozostawić je na wartościach domyślnych).

![faq22](https://ba-pl.github.io/wiki/assets/images/faq/faq22.png "faq22")

![faq23](https://ba-pl.github.io/wiki/assets/images/faq/faq23.png "faq23")

Można też w polu deklaracji zadeklarować jego nazwę i typ. Wtedy po użyciu nazwy bloku w polu kodu programu, pojawi się podpowiedź o dostępnych wejściach i wyjściach bloku.

![faq24](https://ba-pl.github.io/wiki/assets/images/faq/faq24.png "faq24")

## Przekazywanie wartości z bloku funkcyjnego
Jeżeli chcemy przekazywać wartości z wejść/wyjść konkretnego bloku funkcyjnego w inne miejsce programu, można się do odwoływać do wejść/wyjść bloku poprzez podanie nazwy bloku i po kropce nazwy konkretnego wejścia/wyjścia.

![faq25](https://ba-pl.github.io/wiki/assets/images/faq/faq25.png "faq25")

Bloczek Timera tonTimer2 będzie na swoje wejście PT pobierał wartość podaną na wejście PT bloczka Timera tonTimer1. Po zadeklarowaniu bloku funkcyjnego w programie można w trybie online rozwinąć jego pełną strukturę.

![faq26](https://ba-pl.github.io/wiki/assets/images/faq/faq26.png "faq26")

Odwołując się w kodzie programu do danego elementu struktury bloku, jako podpowiedzi wyświetlają się jedynie pola oznaczone jako wejścia i wyjścia, nie są widoczne zmienne wewnętrze (w przypadku timera TON: M oraz StarTime), można ich jednak również używać.

![faq27](https://ba-pl.github.io/wiki/assets/images/faq/faq27.png "faq27")

# Find and Replace 
Jeśli chcemy wyszukać daną frazę w projekcie należy wybrać **EDIT --> Find and Replace --> Quick Find**. W oknie dialogowy, które się pojawi w polu **Find what** wpisujemy szukaną frazę, a w polu **Look in** wybieramy jaki obszar Solution chcemy przeszukiwać.

![faq28](https://ba-pl.github.io/wiki/assets/images/faq/faq28.png "faq28")

Wyszukując daną frazę w Solution, możemy ją zastąpić inną frazą. Należy wtedy wybrać **EDIT --> Find and Replace --> Quick Replace**. W oknie dialogowym w polu **Find what** wpisujemy szukaną frazę, w polu **Replace with** wpisujemy frazę którą chcemy zastąpić wyszukany ciąg liter a w polu **Look in** określamy obszar poszukiwań.

![faq29](https://ba-pl.github.io/wiki/assets/images/faq/faq29.png "faq29")

# Komentarze 
## Komentarze w języku ST
Komentarze w polu deklaracji zmiennych dla wszystkich języków oraz w polu kodu języka ST można umieszczać po znakach //komentarz (komentarz dotyczy aktualnej linii) lub pomiędzy znakami (\* komentarz \*) (komentarz wielolinijkowy).

![faq30](https://ba-pl.github.io/wiki/assets/images/faq/faq30.png "faq30")

## Komentarze w językach IL, LD, FBD
Aby móc umieszczać komentarze w poszczególnych liniach kodu w językach IL, LD oraz FBD należy najpierw uaktywnić taką opcję w ustawieniach TwinCAT. Najpierw należy wybrać **TOOLS --> Options.**

![faq31](https://ba-pl.github.io/wiki/assets/images/faq/faq31.png "faq31")

Następnie odszukujemy opcje dotyczące **TwinCAT --> PLC Enviroment --> FBD, LD and IL editor**. Znajduje się tam ustawienie **Show network comment**, które należy zaznaczyć. Od tej pory możemy wstawiać komentarze w językach IL, LD i FBD w górnej części linii kodu. Wystarczy ustawić tam kursor i wpisać komentarz.

![faq32](https://ba-pl.github.io/wiki/assets/images/faq/faq32.png "faq32")

# Atrybuty 
Atrybuty są to specjalne instrukcje w kodzie źródłowym aplikacji wpływające na proces kompilacji. Poniżej opisano najczęściej wykorzystywane atrybuty. Pełna lista dostępna jest na stronie infosys.beckhoff.com w zakładce **TwinCAT 3 --> TE1000 XAE --> PLC --> Reference Programming --> Pragmas --> Attribute pragmas.**

## Atrybut TcContextName
Atrybut TcContextName pozwala na przydzielenie konkretnych zmiennych do poszczególnych tasków w programach które posiadają więcej niż jeden task. W przykładzie mamy skonfigurowane dwa taski: PLCTask_1 oraz PLCTask_2. Tworzymy jedną listę zmiennych globalnych dla całego projektu i przypisujemy zmiennym z tej listy atrybuty, które przyporządkowują je do odpowiednich tasków.
<br>
W przykładzie atrybuty są tak skonfigurowane, że wszystkie zmienne, poza zmienną bVar2, będą przypisane do PLCTask_2, ponieważ atrybut przypisujący do PLCTask_2 znajduje się przed całą listą.

![faq33](https://ba-pl.github.io/wiki/assets/images/faq/faq33.png "faq33")

## Atrybut TcLinkTo
Atrybut TcLinkTo pozwala na linkowanie zmiennych z poziomu pola deklaracji zmiennych, bez potrzeby robienia tego z poziomu konfiguracji hardware. W podanym przykładzie zmienna została zlinkowana do kanału 1 modułu EL1004.

![faq34](https://ba-pl.github.io/wiki/assets/images/faq/faq34.png "faq34")

## Atrybut Displaymode
Każdej zmiennej można przypisać atrybut określający w jakiej reprezentacji ma być przedstawiana jej wartość – binarnej, decymalnej lub hexadecymalnej.
W przedstawionym przykładzie 3 zmienne zostały nałożone na ten sam obszar pamięci ale każda ma przypisany inny atrybut. W efekcie mamy jedną wartość wyświetloną w 3 różnych formatach:

![faq35](https://ba-pl.github.io/wiki/assets/images/faq/faq35.png "faq35")

## Atrybut Hide
Atrybut ’Hide’ pozwala określić które zmienne mają być niewidoczne, np. niektóre zmienne lokalne w blokach funkcyjnych. W przykładzie, wewnątrz bloku są zadeklarowane dwie zmienne lokalne, jedna z nich (Local2) ma przypisany atrybut ’Hide’. Po wywołaniu bloku w programie i zalogowaniu, zmienna Local 2 nie będzie widoczna w strukturze bloku.

![faq36](https://ba-pl.github.io/wiki/assets/images/faq/faq36.png "faq36")

## Atrybut Hide_all_locals
Atrybut ’Hide all Locals’ pozwala ukryć wszystkie zmienne lokalne z bloku, tak aby po wywołaniu programu w bloku zmienne nie były widoczne w strukturze bloku.

![faq37](https://ba-pl.github.io/wiki/assets/images/faq/faq37.png "faq37")

## Atrybut Strict
Atrybut 'Strict' dodaje się automatycznie podczas tworzenia typu wyliczeniowego (ENUM). Atrybut ten nie dopuszcza do wykonywania operacji arytmetycznych na zmiennej ENUM. Chroni to np. przed sytuacją, kiedy w wyniku działań arytmetycznych na zmiennej ENUM dostajemy w wyniku liczbę, która nie była wcześniej zdefiniowana jako jedna ze stałych enumeracji. Przykład:

![faq38](https://ba-pl.github.io/wiki/assets/images/faq/faq38.png "faq38")

Został zdefiniowany typ danych o nazwie **E_Enum**. Jako stałe enumeracji zostały wykorzystane wartości 1,2 oraz 3. W kodzie programu powstał zapis:

![faq39](https://ba-pl.github.io/wiki/assets/images/faq/faq39.png "faq39")

w wyniku takiego zapisu, zmiennej **ENUM**, zostałaby przypisana wartość 5, która nie była wcześniej zdefiniowana jako jedna ze stałych enumeracji. Dzięki atrybutowi **Strict**, kompilator od razu zgłosi błąd.

![faq40](https://ba-pl.github.io/wiki/assets/images/faq/faq40.png "faq40")

# Zmiana ustawień środowiska
## Zmiana czcionki środowiska
Aby zmienić czcionkę dla całego środowiska, należy wejść w **TOOLS --> Options**. 

![faq41](https://ba-pl.github.io/wiki/assets/images/faq/faq41.png "faq41")

Następnie należy odszukać ustawienia **TwinCAT --> PLC Enviroment --> Text Editor** i w zakładce **Text Area**, kliknąć na pole **Font**.

![faq42](https://ba-pl.github.io/wiki/assets/images/faq/faq42.png "faq42")

## 3.9.2	Zmiana stylu środowiska
Aby zmienić styl środowiska należy wybrać **TOOLS --> Options**. Następnie w ustawieniach **Environment --> General** z rozwijanego menu można wybrać kolor środowiska: Niebieski, Ciemny, Jasny.

![faq43](https://ba-pl.github.io/wiki/assets/images/faq/faq43.png "faq43")

## Zmiana domyślnego koloru zaznaczania
Aby zmienić domyślny kolor podświetlenia zaznaczenia, należy w **Tools --> Options** przejść do Zakładki **TwinCAT --> PLC Enviroment --> Text Editor**, tam wybrać zakładkę **Text area** i zmienić kolor w polu **Selection color.**

![faq44](https://ba-pl.github.io/wiki/assets/images/faq/faq44.png "faq44")

## Zmiana długości wyświetlanych zmiennych w podglądzie online
Aby zmienić domyślną długość wyświetlanych zmiennych typu REAL i STRING w podglądzie online np. w języku CFC, należy w **Tools --> Options** przejść do Zakładki **TwinCAT --> PLC Enviroment --> Text Editor**, tam wybrać zakładkę Monitoring i zmienić pola **Number of displayed digits** i **String length.**

![faq45](https://ba-pl.github.io/wiki/assets/images/faq/faq45.png "faq45")

![faq46](https://ba-pl.github.io/wiki/assets/images/faq/faq46.png "faq46")

# Biblioteki 
##	Zapisywanie projektu w formie biblioteki
Aby zapisać projekt w formie biblioteki należy najpierw ustawić pewne właściwości projektu. W tym celu należy kliknąć PPM na nazwie projektu i wybrać **Properties**. W zakładcie Common należy uzupełnić pola: **Company, Title, Version.**

![faq47](https://ba-pl.github.io/wiki/assets/images/faq/faq47.png "faq47")

Po uzupełnieniu właściwości projektu można kliknąć na jego nazwie PPM i wybrać opcję **Save as library…** Następnie należy wybrać miejsce docelowe na dysku i zapisać bibliotekę. Plik będzie miał rozszerzenie **.library.**

![faq48](https://ba-pl.github.io/wiki/assets/images/faq/faq48.png "faq48")

## Dodawanie bibliotek systemowych do projektu
Aby dodać do projektu bibliotekę, z puli bibliotek dostępnych po zainstalowaniu TwinCAT 3, należy w projekcie PLC kliknąć PPM na **References --> Add library…** Następnie wybieramy bibliotekę z listy i klikamy OK.

![faq49](https://ba-pl.github.io/wiki/assets/images/faq/faq49.png "faq49")

## Korzystanie ze specyficznej wersji biblioteki
Niektóre biblioteki mają więcej niż jedną wersję i do projektu można dodać kilka wersji jednej biblioteki. Aby to zrobić należy PPM kliknąć na References i wybrać **Add library…** W oknie dialogowym które się pojawi należy wybrać przycisk **Advanced…**

![faq50](https://ba-pl.github.io/wiki/assets/images/faq/faq50.png "faq50")

Następnie, w kolejnym oknie, należy zaznaczyć opcję **Display all versions**. W przykładzie zostaną dodane dwie wersje biblioteki Tc2_Utilities (dostępne są 3.3.17.0, 3.3.16.0, \* oznacza korzystanie z najnowszej wersji). Aby dodać bibliotekę, należy zaznaczyć ją na liście i wybrać OK.

![faq51](https://ba-pl.github.io/wiki/assets/images/faq/faq51.png "faq51")

Teraz w Referencjach znajdują się 2 biblioteki o takiej samej nazwie:

![faq52](https://ba-pl.github.io/wiki/assets/images/faq/faq52.png "faq52")

Gdy teraz w projekcie zostanie zadeklarowany blok funkcyjny z biblioteki Tc2_Utilities to po przebudowaniu projektu pojawi się błąd. Błąd ten informuje, że kompilator nie wie z jakiej wersji biblioteki ma korzystać. W celu rozwiązania tego problemu, należy skorzystać z właściwości Namespace.
<br>
Aby przypisać bibliotece odpowiedni **Namespace**, należy wybrać ją z listy Referencji i w **Properties** biblioteki uzupełnić pole **Namespace**. W przykładzie do wersji biblioteki 3.3.16.0 została przypisana nazwa **Older**, a do wersji 3.3.17.0 nazwa **Newer**. Teraz, deklarując blok z biblioteki Tc2_Utilities, należy dodatkowo określić z której wersji biblioteki będziemy korzystać. Określa się to używając właśnie odpowiedniego **Namespace.**

![faq53](https://ba-pl.github.io/wiki/assets/images/faq/faq53.png "faq53")

## Instalowanie własnych bibliotek
Aby dodać do projektu bibliotekę która nie jest domyślną biblioteką dla TwinCAT, należy ją najpierw zainstalować. W tym celu należy kliknąć PPM na **References -> Library repository…** W oknie dialogowym które się pojawi wybieramy przycisk **Install** i dodajemy z dysku plik z biblioteką z rozszerzeniem **.library**. Plik powinien dodać się w kategorii **(Miscellaneous)**. Po instalacji bibliotkę można dodać do projektu poprzez opcję **Add library.**

![faq54](https://ba-pl.github.io/wiki/assets/images/faq/faq54.png "faq54")

## Przekazywanie projektów z bibliotekami własnymi
Przy pokazywaniu projektów w których znajdują się biblioteki nie będące domyślnymi bibliotekami dla TwinCAT, po otwarciu projektu należy biblioteki te doinstalować. Jeśli projekt został przekazany w odpowiedniej formie wystarczy kliknąć PPM na nazwie projektu PLC i wybrać opcję **Install Project Libraries.**

![faq55](https://ba-pl.github.io/wiki/assets/images/faq/faq55.png "faq55")

##	Kopiowanie bibliotek z TC2 do TC3
Czasami po konwersji projektu z TwinCAT 2 do TwinCAT 3 może wystąpić problem z niektórymi blokami funkcyjnymi znajdującymi się w importowanych bibliotekach. Aby go naprawić należy wykonać następujące kroki:
- Skopiować brakujące biblioteki z lokalizacji TC2:  **C:\TwinCAT\Plc** do lokalizacj **C:\TwinCAT\3.1\Components\Plc\Converter\Lib**
- Usunąć bieżące mapowanie konwersji biblioteki w opcjach Visual Studio. **Tools --> Options --> TwinCat --> PLC Environment --> Libraries.**  

![faq56](https://ba-pl.github.io/wiki/assets/images/faq/faq56.png "faq56")

- Po utworzeniu projektu nowego solution i zaimportowaniu projektu z TC2 wskazać na jaką bibliotekę w TC3 ma być użyta konwersja ( wraz z zaznaczonym checkbox „Remember this mapping for…”) .

![faq57](https://ba-pl.github.io/wiki/assets/images/faq/faq57.png "faq57")

# Tips and tricks 
## Watchlist 
Aby dodać zmienne do swojej Watchlisty należy kliknąć na wybraną zmienną PPM a następnie wybrać **Add To Watch**. Pojawi się okno **ADS Symbol Watch** gdzie będą widoczne wybrane zmienne. Po zapisaniu projektu, **Watchlist** również zostanie zapisany.

![faq58](https://ba-pl.github.io/wiki/assets/images/faq/faq58.png "faq58")

Aby wywołać Watchlist, należy wybrać **View --> Other Windows --> ADS Symbol Watch:**

![faq59](https://ba-pl.github.io/wiki/assets/images/faq/faq59.png "faq59")

##	Otwieranie aplikacji stworzonej w TwinCAT 2 w TwinCAT 3
Aby otworzyć w TC3 aplikację stworzą w TC2, należy kliknąć PPM na nazwę projektu w TC3 i wybrać opcję **Load Project from TwinCAT 2.xx Version**. W oknie które się pojawi należy wskazać plik z rozszerzeniem .tsm czyli plik konfiguracyjny z TC2 zawierający ścieżkę do programu.

![faq60](https://ba-pl.github.io/wiki/assets/images/faq/faq60.png "faq60")

Jeśli chcemy otworzyć jednie projekt PLC, wystarczyć wybrać opcję jak na zdjęciu poniżej i wskazać plik .pro: 

![faq61](https://ba-pl.github.io/wiki/assets/images/faq/faq61.png "faq61")


