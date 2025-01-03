---
title: Instalacja dodatków 
parent: Windows Embedded Compact 
nav_order: 3
layout: page
---


# Instalacja dodatków na systemach z Windows CE
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Instrukcja przedstawia sposób instalacji plików na systemach Windows CE. Przykład będzie przeprowadzony krok po kroku na podstawie instalacji pliku **TwinCAT PLC HMI CE** dla TwinCAT 2 oraz dodatku **TwinCAT Database Server** dla TwinCAT 3.
Proces instalacji biblioteki dla Windows CE składa się z dwóch etapów. W pierwszym etapie w środowisku Windows XP/Vista/7/10 z pliku z rozszerzeniem \*.exe instalujemy pliki instalacyjne dla Windows CE, które mają rozszerzenie \*.cab. W drugim etapie instalujemy taki plik na urządzeniu docelowym.

# Pobranie pliku i instalacja na komputerze PC dla TwinCAT 2
Dodatki/biblioteki TwinCAT dostępne są na stronie [Beckhoff.](https://beckhoff.pl/)
Dla przykładu, link do pobrania dodatku [PLC HMI](https://www.beckhoff.com/en-en/support/download-finder/search-result/?download_group=97268980&download_item=97269126) 
<br>
Uruchomienie pobranego pliku, przywoła instalatora TwinCAT Installer. Akceptujemy warunki licencji i uzupełniamy dane w oknie:

![cei1](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei1.png "cei1")

Pola *User Name* i *Company Name* można uzupełnić według uznania. W polu *Serial Number* (Pxxx-xxxx-xxxx) należy wpisać klucz, który został wygenerowany przez firmę BECKHOFF. Po wpisaniu prawidłowego klucza i uzupełnieniu danych możliwe jest zainstalowanie biblioteki. Instalator tworzy katalog z plikami w folderze …\TwinCAT\CE\, domyślnie C:\TwinCAT\CE\.

![cei2](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei2.png "cei2")

W katalogu TwinCAT PLC HMI CE mamy dwa podkatalogi z plikami: ARM i X86 – dla odpowiednich typów procesorów. W każdym z nich jest odpowiedni plik instalacyjny dla systemu Windows CE (rozszerzenie pliku \*.cab). Wybieramy plik zgodnie z typem procesora urządzenia na którym zamierzamy zainstalować daną bibliotekę.

# Pobranie pliku i instalacja na komputerze PC dla TwinCAT 3
Dodatki/biblioteki TwinCAT dostępne są na stronie [Beckhoff.](https://beckhoff.pl/)
Dla przykładu, link do pobrania dodatku [Databse Server](https://www.beckhoff.com/pl-pl/support/download-finder/search-result/?download_group=97173780&download_item=97173979)
<br>
Uruchomienie pobranego pliku (należy pamiętać że dla TwinCAT 3 wszystkie pliki instalacyjne należy uruchamiać jako Administrator), przywoła instalatora TwinCAT Installer. Należy w nim wybrać które komponenty biblioteki zostaną zainstalowane a następnie wybrać *Next* i przejść proces instalacji.

![cei3](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei3.png "cei3")

W międzyczasie zostaniemy zapytani do których wersji Visual Studio dointegrowany zostanie dodatek na komputerze, na którym znajduje się wersja inżynierska TwinCAT 3.

![cei4](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei4.png "cei4")

Po zainstalowaniu dodatku domyślnie pojawi się on w folderze *TwinCAT/Functions/Kod dodatku*, np. *TwinCAT/Functions/TF6420-Database-Server*

![cei5](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei5.png "cei5")

Po otwarciu folder znajdują się w nim 3 foldery, każdy zawierający pliki dla odpowiednich systemów operacyjnych oraz procesorów:

![cei6](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei6.png "cei6")

W naszym przypadku korzystać będziemy z pliku z rozszerzeniem .cab (dodatek instalowany będzie na CX9020 z procesorem ARM).

# Skopiowanie pliku na sterownik
Plik możemy skopiować na sterownik na kilka sposobów:

1. Bezpośrednio na kartę Compact Flash / micro SD, jeżeli mamy do niej dostęp np. za pomocą czytnika kart CF lub micro SD.
2. Za pomocą folderu Public udostępnionego w sterowniku (SMB - Server Message Block).
3. Transfer pliku z poziomu TwinCAT podczas wgrywania projektu (tylko TwinCAT 3). Dokłady opis można znaleźć [tutaj.](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_plc_intro/3260050187.html) 
4. Za pomocą zewnętrznej pamięci, np. pendrive (pojawi się w systemie jako Hard Disk2).

**Dostęp do folderu udostępnionego w sterowniku (SMB - Server Message Block)**
<br>
<br>
**Uwaga!** 
<br>
**W systemie Windows 7 i Windows 8.1/10 należy zmienić zabezpieczenia sieci – patrz *Dodatek I***
<br>
<br>
Na komputerze z którego chcemy dostać się do folderu udostępnionego otwieramy Windows Explorer lub **Start --> Uruchom(Run)** i wpisujemy
- \\<IP_Address>, np.: \\10.24.2.123 
- lub \\<Host_Name>, np.: \\CX_123456 

<br>
W oknie logowania, należy wpisać odpowiednio:
<br>
**Login**: Administrator
<br>
**Hasło**: 1
<br>
<br>
W tym momencie powinien ukazać się udostępniony w sterowniku folder.
Można teraz skopiować do katalogu Public wybrany wcześniej plik. Od tej chwili plik będzie dostępny w systemie operacyjnym sterownika.

<br>
**Warto wiedzieć**: Folder Public znajduje się w pamięci RAM sterownika. Po zaniku zasilania znika jego zawartość.

<br>
**Uwaga!** W celu prawidłowego działania protokołu SMB konieczne jest odblokowanie portów 137-139 w ustawieniach Firewall na sterowniku (patrz **Dodatek II**)

# Instalacja pliku na sterowniku

Jeżeli do urządzenia nie jest podpięty wyświetlacz, to potrzebny nam będzie zdalny pulpit dla systemu Windows CE - program CERHOST.
<br>
W celu podłączenia się do sterownika należy kliknąć **File (1)** i wybrać **Connect**, następnie w polu **Hostname (2)** wpisać nazwę sieciową urządzenia lub jego adres IP i zatwierdzić to przyciskiem **OK (3)**. Poniżej przedstawione okno zdalnego pulpitu.

![cei7x](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei7x.png "cei7x")

Należy z Menu **Start** wybrać polecenie **Run…**, wpisać \(backslash) i zatwierdzić poleceniem OK. 
<br>
Otworzy nam się explorer plików w którym należy wejść do folderu Public (lub folderu Hard Disk jeśli plik kopiowany był bezpośrednio na kartę pamięci). 

![cei8x](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei8x.png "cei8x")
<br>
Następnie klikamy szybko 2xLPM na pliku instalacyjnym (1), przywoła to okno instalacji i klikamy w przycisk OK. (pole nr 2.) – instalujemy w domyślnej lokalizacji! Po zakończeniu instalacji plik zostanie automatycznie usunięty.

![cei8x2](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei8x2.png "cei8x2")

Na koniec należy zresetować sterownik z poziomu Windows CE przez polecenie w Menu Start --> Reset. Jest to bardzo ważne, ponieważ zmiany muszą zostać wprowadzone do systemu!

# Dodatek I - Przystosowanie Windows 7/8/10 do korzystania z folderu Public
**Windows 7**
<br>
<br>
W okienku **Uruchom** wpisujemy *mmc.exe*:

![cei81](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei81.png "cei81")

następnie wykonujemy kroki pokazane numerkami na rysunku:

![cei9](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei9.png "cei9")

Po zatwierdzeniu powyższego okna należy rozwinąć drzewo do pozycji pola 5. na poniższym rysunku i odszukać w rubryce Zasady pozycję Zabezpieczenia sieci poziom uwierzytelnienia LAN Manager (pole nr 6.).

![cei10](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei10.png "cei10")

Kliknięcie szybko 2xLPM przywoła okno pokazane na poniższym rysunku. Należy wybrać pozycję zaznaczoną ramką oraz zatwierdzić zmiany przyciskiem OK.

![cei11](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei11.png "cei11")

Dla Windows 8.1/10 procedura opisana jest pod [linkiem.](https://docs.microsoft.com/en-us/windows-server/storage/file-server/troubleshoot/detect-enable-and-disable-smbv1-v2-v3) W naszym przypadku do komunikacji ze sterownikiem konieczny jest protokół SMB v1.

# Dodatek II - Dodanie użytkownika w Windows CE / Odblokowanie portów w Firewall
**Dodawanie użytkowników**
<br>
<br>
W Panelu Sterowania (Control Panel) Windowsa CE wybieramy CX Configuration. Na zakładce FTP klikamy PPM w polu NTLM User i wybieramy opcję Add User.

![cei12](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei12.png "cei12")

Wypełniamy pola:
- User Name
- Password
- Confirm Password

![cei13](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei13.png "cei13")

Całość zatwierdzamy komendą Apply (zastosuj). Od tej pory użytkownik jest aktywny.
<br>
<br>
**Odblokowanie portów w Firewall**
<br>
<br>
W Panelu Sterowania (Control Panel) Windowsa CE wybieramy CX Configuration. Następnie przechodzimy do zakładki Firewall, w której wybieramy *Create Rule*:

![cei14](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei14.png "cei14")

Następnie wybieramy **Typ, Protokół**, a także **Porty**, które mają zostać odblokowane (tutaj porty dla serwera SMB), dodatkowo można dodać opis reguły, następnie zatwierdzamy przyciskiem OK:

![cei15](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei15.png "cei15")

Reguła powinna pojawić się na liście aktywnych zasad:

![cei16](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei16.png "cei16")

Aby zatwierdzić ustawienia Firewall należy użyć przycisku *Apply*.

![cei17](https://ba-pl.github.io/wiki/assets/images/InstalacjaCE/cei17.png "cei17")


