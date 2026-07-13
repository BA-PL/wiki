---
title: Wersja 4024 
parent: Instrukcja instalacji 
nav_order: 2
layout: page
---


# Instalacja wersji 4024
{: .no_toc }
<h6> Data modyfikacji: 07.02.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Poniżej opisany zostanie cały proces instalacji podstawowej wersji inżynierskiej – od wskazania miejsca pobrania wymaganych plików, przez proces instalacji oprogramowania, instalacji powłoki programistycznej TcXaeShell aż po pierwsze uruchomienie TwinCAT 3. 
# Pobieranie plików instalacyjnych 
Pliki instalacyjne należy pobierać wyłącznie ze strony [Beckhoff.](https://www.beckhoff.com/pl-pl/) Zapewni to, że będą to zawsze najbardziej aktualne i najbezpieczniejsze wersje. Link do pobrania [najnowszej wersji TwinCAT 3.](https://www.beckhoff.com/pl-pl/support/download-finder/software-and-tools/) 
<br>
Stamtąd należy wybrać interesującą nas wersję TwinCAT (zalecane jest, aby wybierać najnowszą dostępną wersję).

![241](https://ba-pl.github.io/wiki/assets/images/4024/241.png "241")

Jeśli oprogramowanie jest pobierane ze strony po raz pierwszy, konieczne będzie założenie konta na stronie. Po tym zostaniemy przekierowani do okna, w którym będzie widniał docelowy link, umożliwiający pobranie oprogramowania:

![242](https://ba-pl.github.io/wiki/assets/images/4024/242.png "242")

Po zakończeniu pobierania plików można przystąpić do instalacji oprogramowania TwinCAT 3. Aby to zrobić należy otworzyć plik .exe pobrany ze strony beckhoff.com i postępować zgodnie z instrukcjami przedstawionymi na ekranie.
<br>
**(UWAGA! Wszystkie pliki instalacyjne należy uruchamiać poprzez wybranie PPM --> Uruchom jako Administrator!)**. Podczas procesu instalacyjnego można wybrać instalację domyślną lub instalację użytkownika, w której wybrać można np. miejsce w którym zainstalowane zostanie oprogramowanie. Zalecane jest, aby TwinCAT 3 zainstalowany był na tym samym dysku co system.
<br>
Podczas instalacji zostaniemy zapytani czy chcemy doinstalować oprogramowanie Git for Windows. Używane jest ono przy funkcji TwinCAT Multiuser, która dostępna jest w wersji 4024.7. Narzędzie to ułatwia później pracę kilku programistów nad jednym projektem, stąd zalecana jest jego instalacja.

![243](https://ba-pl.github.io/wiki/assets/images/4024/243.png "243")

Jeżeli na komputerze zainstalowany jest Microsoft Visual Studio do wersji 2017 włącznie, podczas instalacji pojawi się okno proponujące zintegrowanie oprogramowania TwinCAT 3 z powłoką Visual Studio (wersje niezainstalowane będą niedostępne). W tym samym oknie pojawi się również propozycja doinstalowania powłoki TcXaeShell, która jest dedykowanym oprogramowaniem do obsługi TwinCAT 3 opartym o Microsoft Visual Studio.

![244](https://ba-pl.github.io/wiki/assets/images/4024/244.png "244")

# Uruchamianie środowiska 
Po zakończeniu instalacji zostaniemy poproszeni o restart komputera. Po jego wykonaniu na pasku dolnym przy zegarku pojawi się ikona TwinCAT, która powinna mieć kolor niebieski – oznacza to że instalacja została wykonana poprawnie, a TwinCAT na lokalnym komputerze jest w trybie konfiguracyjnym.

![245](https://ba-pl.github.io/wiki/assets/images/4024/245.png "245")

Po kliknięciu PPM będziemy mieli możliwość uruchomienia oprogramowania (wersji XaeShell jeśli została doinstalowana oraz Visual Studio o ile było zainstalowane wcześniej, a TwinCAT został do niego dointegrowany). Po uruchomieniu powinna pojawić się strona startowa pierwszego uruchomienia (zazwyczaj trwa ono dłuższą chwilę, ponieważ wszystkie elementy interfejsu i pliki konfiguracyjne muszą zostać skonfigurowane przez system operacyjny)

![246](https://ba-pl.github.io/wiki/assets/images/4024/246.png "246")

# Możliwe problemy z instalacją
## Brak ikony TwinCAT na pasku 
Jeżeli ikona TwinCAT nie pojawiła się na pasku, należy przejść do folderu *TwinCAT/3.1/System* i uruchomić jako Administrator plik *TcSysUI.exe*. Jeśli po uruchomieniu tego pliku dostaniemy błąd, oznacza to że instalacja nie została wykonana poprawnie i należy wykonać ją ponownie.

![247](https://ba-pl.github.io/wiki/assets/images/4024/247.png "247")

## Szara ikona TwinCAT
Domyślnie po zainstalowaniu ikona TwinCAT na pasku przy zegarku powinna być w kolorze niebieskim. Ikonka w kolorze szarym oznacza, że instalacja nie została wykonana poprawnie i należy wykonać ją ponownie – najprawdopodobniej instalacja nie była wykonywana na prawach administratora.
# Instalacja bibliotek oraz dodatkowych narzędzi 
W rozdziale tym opisane zostanie, w jaki sposób zidentyfikować, jaka biblioteka potrzebna jest dla danej funkcji/bloku funkcyjnego a także jak pobrać i zainstalować dodatkowe biblioteki lub narzędzia inżynierskie jeśli takowych na komputerze nie posiadamy.
## Sprawdzanie wymaganej biblioteki 
Aby sprawdzić, jaka biblioteka wymagana jest dla danej funkcji lub bloku funkcyjnego, należy odszukać go na stronie infosys.beckhoff.com a następnie, na samym dole strony, znajdować się będzie informacja jakie biblioteki wymagane są do działania danego bloku funkcyjnego. Przykład dla bloku FB_MBReadCoils:

![248](https://ba-pl.github.io/wiki/assets/images/4024/248.png "248")

Na podstawie zdjęcia można stwierdzić, że do działania bloku potrzebna jest biblioteka Tc2_ModbusSrv (dodatkowo po przejściu do bloku odpowiedni dodatek będziemy widzieć w drzewku po lewej stronie ekranu i na samej górze opisu bloczka).

![249](https://ba-pl.github.io/wiki/assets/images/4024/249.png "249")

Biblioteka ta nie jest domyślnie zainstalowana razem z oprogramowaniem TwinCAT 3. Należy doinstalować ją dodatkowo.
## Instalacja dodatkowych bibliotek 
Aby zainstalować dodatkowe biblioteki, które nie były zainstalowane razem z podstawową wersją TwinCAT 3, należy wejśc na stronę [Beckhoff downloads](https://beckhoff.pl/english/download/tc3-downloads.htm), następnie w interesującą nas zakładkę w zakładce Function, a następnie wybrać z listy odpowiednią bibliotekę (w tym wypadku będzie to biblioteka TF6250 Modbus TCP Server):

![2410](https://ba-pl.github.io/wiki/assets/images/4024/2410.png "2410")

Jeśli dla biblioteki nie ma instalatora oznacza to, że jest to biblioteka domyślnie instalowana w podstawowej wersji TwinCAT 3.
## Instalacja dodatkowych narzędzi inżynierskiech 
Niektóre narzędzia inżynierskie, takie jak na przykład narzędzie TE2000 TwinCAT HMI wymagają dodatkowego doinstalowania (nie są instalowane razem z podstawową wersją narzędzia). W tym wypadku instalator takiego narzędzia pobiera się z tej samej strony, z której pobiera się podstawową wersję inżynierską. Następnie instalator taki należy uruchomić jako Administrator i postępować zgodnie z instrukcjami wyświetlanymi na ekranie. Po zakończeniu instalacji możliwość dodania projektów w innych narzędziach inżynierskich pojawi się w oknie dodawania nowego projektu (PPM na nazwie Solution --> Add --> New Project).

![2411](https://ba-pl.github.io/wiki/assets/images/4024/2411.png "2411")

# Remote Manager dla starszych wersji 

Istnieje możliwość zainstalowania na komputerze programisty kilku wersji oprogramowania TwinCAT jak również doinstalowania starszych wersji do już zainstalowanej najnowszej. Funkcjonalność tę umożliwia **TwinCAT Remote Manager.**
<br>
Należy wtedy pobrać nie standardowy instalator TwinCAT, a właśnie instalator [Remote manager.](https://www.beckhoff.com/pl-pl/support/download-finder/search-result/?download_group=97028330)
<br>
<br>
Pełna dokumenetacja znajduje się [tutaj.](https://download.beckhoff.com/download/document/automation/twincat3/TC3_Remote_Manager_en.pdf)
<br>
<br>
Po instalacji **RM**, aby na swoim komputerze móc wybrać odpowiednią wersję Remote Manager, należy to zrobić przed otwarciem projektu:

![2412](https://ba-pl.github.io/wiki/assets/images/4024/2412.png "2412")

