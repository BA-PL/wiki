---
title: Beckhoff Service Tool 
parent: Windows 7/10/11 
nav_order: 1
layout: page
---

# Beckhoff Service Tool (BST) 
{: .no_toc }
<h6> Data modyfikacji: 14.01.2026 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Niniejsza instrukcja przedstawia sposób korzystania z narzędzia Beckhoff Service Tool, służącego do tworzenia i przywracania kopii zapasowej danych komputera przemysłowego (jak i przywracania ustawień fabryczynych). Dostarczane jest ono poprzez odpowiednio skonfigurowaną pamięć USB (pendrive’a BST).

![bst000](https://ba-pl.github.io/wiki/assets/images/BST/bst000.png "bst000")

# Dobór wersji BST 

Przed użyciem BST na komputerze przemysłowym, należy sprawdzić i dostosować jego wersję. Aby sprawdzić wersję, należy wpiąć BST np. do swojego laptopa. Pojawi sie on jako nowy dysk z nazwą **Image**:

![bst00](https://ba-pl.github.io/wiki/assets/images/BST/bst00.png "bst00")

Następnie szukamy pliku z informacją o wersji:

![bst01](https://ba-pl.github.io/wiki/assets/images/BST/bst01.png "bst01")

Wersja BST powinna być dostosowana do komputera przemysłowego na którym będziemy BST operować. Wersję będziemy dobierać na podstawie dostępnej na komputerze pamięci RAM.

![bst02](https://ba-pl.github.io/wiki/assets/images/BST/bst02.png "bst02")

Jeśli zachodzi potrzeba zmiany wersji BST, można to zrobić za pomocą narzędzia ApplyBST. Narzędzie można pobrać wraz z odpowiednią wersją obrazu BST na [stronie Beckhoff:](https://www.beckhoff.com/pl-pl/products/ipc/panel-pcs/accessories/bst.html?)

![bst1](https://ba-pl.github.io/wiki/assets/images/BST/bst1.png "bst1")

Po rozpakowaniu pobranego archiwum (przy założeniu że BST jest wpięty do naszego laptopa) uruchamiamy narzędzie ApplyBST:

![bst2](https://ba-pl.github.io/wiki/assets/images/BST/bst2.png "bst2")

Po otwarciu aplikacji wyświetli się następujące okno:

![bst4](https://ba-pl.github.io/wiki/assets/images/BST/bst4.png "bst4")

W tym momencie należy wybrać ścieżkę do pliku BST o pożądanej wersji oraz naszego pendrive’a BST (w naszym przypadku jest to Dysk E), a następnie nacisnąć **Deploy** .
<br>
Może pokazać się okno:

![bst5](https://ba-pl.github.io/wiki/assets/images/BST/bst5.png "bst5")

W tym wypadku należy usunąć zawartość folderu **Images** znajdującego się na BST (folder ten zawiera wykonywane wcześniej kopie zapasowe, zalecamy przenieść je w inne miejsce, poza BST).
<br>
Następnie wyświetli się okno: 

![bst6](https://ba-pl.github.io/wiki/assets/images/BST/bst6.png "bst6")

Jest to informacja o usunięciu wszystkich plików z pendrive’a po zakończeniu procesu, należy potwierdzić aby kontynuować.
<br>
Jeśli na pedrive’ie znajduje się już jakaś wersja BST może wyświetlić się zapytanie o zachowanie lub nadpisanie ustawień:

![bst7](https://ba-pl.github.io/wiki/assets/images/BST/bst7.png "bst7")

Jeśli *Deployment Progress* wyniesie 100% można zamknąć okno ApplyBST.

# Uruchomienie BST na komputerze przemysłowym 

Aby przejść do tworzenia lub przywracania kopii zapasowej na komputerze przemysłowym, należy taki komputer zbootować z BST (wymaga to chwilowej przerwy w normalnej pracy komputera). Można to zrobić na dwa sposoby:
- wpiąć BST do komputera przemysłowego w trakcie jego pracy, a następnie wykonać restart systemu
- wyłączyć bezpiecznie system i zdjąć zasilanie z kompuetra przemysłowego --> wpiąć BST --> podać zasilanie

<br>
W obu przypadkach przy ponownym uruchomieniu powienin pojawić się interfejs BST (nie powienin wystartować system operacyjny).  

![bst10](https://ba-pl.github.io/wiki/assets/images/BST/bst10.png "bst10")

Jeśli dzieje się inaczej, należy w BIOS zmienić ustawienia **Boot order**. 
<br>
<br>
Aby mieć kontrolę nad BST potrzebujemy mieć podłączony do komputera przemysłowego dowolny monitor/panel przemysłowy lub można skorzystać ze zdalnego dostępu za pomocą aplikacji TightVNC Viewer (połączenie klasycznym pulpitem zdalnym RDP nie zadziała). 

## Tworzenie kopii zapasowej 

Po poprawnym zbootowaniu BST, aby rozpocząć tworzenie kopii zapasowej wybierz opcję **Backup** na ekranie głównym (1):

![bst18](https://ba-pl.github.io/wiki/assets/images/BST/bst18.png "bst18")

Następnie do wyboru jest format pliku naszej kopii zapasowej: wim lub tib. Zalecany jest wybór Acronis Image (2):

![bst19](https://ba-pl.github.io/wiki/assets/images/BST/bst19.png "bst19")

W kolejnym kroku wybierz dysk, którego kopie zapasową chcesz utworzyć (4), w przykładzie wybrany jest cały dysk komputera przemysłowego: 

![bst20](https://ba-pl.github.io/wiki/assets/images/BST/bst20.png "bst20")

Następnie można ustawić hasło do naszej kopii i ustawić sposób kompresji. Zalecane jest ustawienie domyślne High.

![bst21](https://ba-pl.github.io/wiki/assets/images/BST/bst21.png "bst21")

Tutaj nadaj nazwę swojej kopii (7) oraz wskaż ścieżkę (8) pod którą ma zostać zapisany plik z backupu. Możesz go zapisać:
- w katalogu *Images* bezpśrednio na pendrive’ie BST 
- na zewnętrznym dysku/pendrive'ie (musi być wcześniej podłączony do komputera przemysłowego)
- na dysku sieciowym (za pomocą opcji *Map Network Drive*)
<br>
Kopia nie musi znajdować się bezpośrednio na BST.

![bst22](https://ba-pl.github.io/wiki/assets/images/BST/bst22.png "bst22")

Następnie pojawi się ekran podsumowywujący, naciśnij **Proceed** jeśli wszystko zgadza się z Twoimi oczekiwaniami.
<br>
Jeśli paski **Current Process i Completed** wypełnią się w całości, pojawi się możliwość naciśnięcia **Close** i tym samym zakończenia procesu tworzenia kopii zapasowej danych.

![bst23](https://ba-pl.github.io/wiki/assets/images/BST/bst23.png "bst23")

Po powrocie do ekranu startowego, należy wybrać opcję **Shutdown**, a następnie:
- wybrać opcję Shutdown --> po chwili wyłączyć zasilanie komputera przemysłowego --> wypiąć BST --> podać zasilanie --> powienin wystartować system operacyjny /  **(opcja zalecana)**
- wybrać opcję restartu i w trakcie restartowania wypiąć BST z komputera przemysłowego --> powienin wystartować system operacyjny

## Przywracanie kopii zapasowej

Aby rozpocząć przywrócenie kopii zapasowej wybierz **Restore** na ekranie głównym:

![bst24](https://ba-pl.github.io/wiki/assets/images/BST/bst24.png "bst24")

Następnie należy wybrać format pliku stworzonej wcześniej kopii zapasowej:

![bst25](https://ba-pl.github.io/wiki/assets/images/BST/bst25.png "bst25")

Tutaj wskaż ścieżkę do pliku w folderze *Images* na pendrive’ie BST, zewnętrznym dysku twardym lub dysku sieciowym, w którym znajduje się zapisana kopia:

![bst26](https://ba-pl.github.io/wiki/assets/images/BST/bst26.png "bst26")

Wybierz, gdzie ma być przywrócona kopia zapasowa. W naszym przypadku, została wykonana kopia całego dysku, stąd zostaje ona przywrócona do całego dysku (6), (8):

![bst27](https://ba-pl.github.io/wiki/assets/images/BST/bst27.png "bst27")

![bst28](https://ba-pl.github.io/wiki/assets/images/BST/bst28.png "bst28")

Następnie pojawi się ekran podsumowywujący, naciśnij **Proceed** jeśli wszystko zgadza się z Twoimi oczekiwaniami.
<br>
Jeśli paski **Current Process i Completed** wypełnią się w całości pojawi się możliwość naciśnięcia Close i tym samym zakończenia procesu przywracania kopii zapasowej danych.

![bst29](https://ba-pl.github.io/wiki/assets/images/BST/bst29.png "bst29")

Po powrocie do ekranu startowego, należy wybrać opcję **Shutdown**, a następnie:
- wybrać opcję Shutdown --> po chwili wyłączyć zasilanie komputera przemysłowego --> wypiąć BST --> podać zasilanie --> powienin wystartować system operacyjny /  **(opcja zalecana)** 
- wybrać opcję restartu i w trakcie restartowania wypiąć BST z komputera przemysłowego --> powienin wystartować system operacyjny

## Aktywacja dostępu zdalnego (opcjonalne) 

### Aktywacja za pomocą pliku (przed użyciem BST)

Jeśli nie posiadamy żadnego monitora podłączonego do komputera przemysłowego, przed użyciem BST można aktywować na nim dostęp zdalny. W tym celu wpinamy BST do swojego laptopa i edytujemy znajdujący się na nim plik **Settings.XML** :

![bst14](https://ba-pl.github.io/wiki/assets/images/BST/bst14.png "bst14")

Po otwarciu pliku należy zmienić część kodu znajdującą się pod tagiem **RemoteControl** na pokazaną poniżej:

```
<RemoteControl>
   <Enabled>1</Enabled>
   <Password>NfRBGb4cE5eqcNBbzvp2U3CJShU19bdT</Password>
   <Application Path="" Arguments="" />
</RemoteControl>
```

Po tych zmianach zdalny dostęp do BST będzie możliwy po wpisaniu hasła: **admin**
<br>
Hasło można zmienić na późniejszym etapie, już po uruchomieniu BST.

### Aktywacja dostępu zdalnego z użyciem monitora 
Ta metoda wymaga podłączenia, przynajmniej raz, monitora do komputera na którym bootujemy BST. Po uruchomieniu BST na ekranie startowym wybieramy opcję **Settings**:

![bst10](https://ba-pl.github.io/wiki/assets/images/BST/bst10.png "bst10")

Następnie wchodzimy w ustawienia i zmieniamy ustawienie serwera TightVNC na *Enable*, można również ustawić swoje hasło:

![bst11](https://ba-pl.github.io/wiki/assets/images/BST/bst11.png "bst11")

## Dostęp zdalny 

Jeśli BST został podłączony do komputera przemysłowego i poprawnie się zbootował (należy odczekać ok. 5 minut), ale nie posiadamy ekranu, to można podłączyć się do BST zdalnie za pomocą aplikacji TightVNC Viewer.
<br>
W tym celu pobieramy oprogramowanie [TightVNC Viever](https://www.tightvnc.com/download.php)
<br>
Po instalacji i uruchomieniu aplikacji, pojawi się okno:

![bst15](https://ba-pl.github.io/wiki/assets/images/BST/bst15.png "bst15")

W miejscu Remote Host musi być wpisany adres IP Hosta – komputera przemysłowego uruchomionego z wpiętym pendrive’em BST. Aby znaleźć ten adres, należy odczytać z nameplate’a posiadanego przez nas sterownika:
- w przypadku sterownika z systemem operacyjnym Windows 7, 10 LTSB 2016, 10 LTSC 2019: adres MAC ID 1, gdzie nazwą hosta będzie: BST–6 ostatnich cyfr adresu MAC ID, które należy wypisać bez spacji, np. BST-8F9B36.
- w przypadku sterownika z systemem operacyjnym Windows 10 LTSC 2021, TwinCAT/BSD, TCRTOS: numer seryjny/numer BTN jednostki, gdzie nazwą hosta będzie: BST-numer seryjny/numer BTN, np. BST-000thrlc.
Sterownik z wpiętym BST dostaje adres IP z serwera DHCP lub z puli auto IP. Jeśli znajdujemy się w tej samej podsieci, możemy wykonać komendę ping po nazwie hosta, w celu weryfikacji adresu IP:

![bst15_1](https://ba-pl.github.io/wiki/assets/images/BST/bst15_1.png "bst15_1")

Można też użyć zewnętrzyn narzędzi, jak np. Advanced IP Scanner:

![bst16](https://ba-pl.github.io/wiki/assets/images/BST/bst16.png "bst16")

Jeśli natomiast komputer w trakcie normalnej pracy nie miał w systemie statycznego adresu IP, a adres automatyczny, to po zbootowaniu z BST pozostanie on taki sam.
<br>
Po wpisaniu poprawnego adresu IP w oknie aplikacji TightVNC Viwer, pojawi się kolejne okno:

![bst17](https://ba-pl.github.io/wiki/assets/images/BST/bst17.png "bst17")

w którym wpisz hasło domyślne (admin) lub własne, jeśli było ono przez Ciebie zmieniane i przejdź dalej naciskając OK. Spowoduje to otwarcie okienka Beckhoff Service Tool. Dalej postępuj zgodnie ze wskazówkami z powyższych rozdziałów dotyczących tworzenia lub przywracania kopii zapasowej. 
<br>
Więcej informacji o dostępie zdalnym znajdziesz [tutaj.](https://infosys.beckhoff.com/english.php?content=../content/1033/bst/4382504331.html)

## Przywracanie ustawień fabrycznych za pomocą BST
Przy pomocy narzędzia BST możliwe jest również przywrócenie systemu naszego komputera przemysłowego do ustawień fabrycznych. Procedura postępowania jest wówczas analogiczna do procedury przywracania kopii zapasowej opisanej w rozdziale [Przywracanie kopii zapasowej](https://ba-pl.github.io/wiki/docs/IPC/BigWindows/bst/#przywracanie-kopii-zapasowej), gdzie zamiast wykonanej przez nas kopii należy w kroku czwartym (4) wybrać plik z fabrycznym obrazem systemu. 
<br>
Plik ten można otrzymać kontaktując się z supportem Beckhoff (**support@beckhoff.pl**) podając numer seryjny posiadanego komputera przemysłowego. Plik taki należy umieścić na BST w katalogu *Images* przed rozpoczęciem procedury przywracania.

# Informacje dodatkowe

## Zmiana Boot Order w BIOS

Może się zdarzyć, że pomimo podłączenia BST do sterownika uruchamia się system Windows. Często powodem takiego zachowania jest zamieniona kolejność nośników rozruchu w BIOS. Parametr ten decyduje z jakiego urządzenia ładowany jest system operacyjny. Aby skorzystać z BST parametr ten musi być ustawiony na Service Stick. Zmiany możemy dokonać przechodząc w do BIOS (należy nacisnąć klawisz Delete w momencie uruchamiania systemu operacyjnego) --> Boot:

![bst33](https://ba-pl.github.io/wiki/assets/images/BST/bst33.png "bst33")

Po ustawieniu opcji jak na powyższym zdjęciu przechodzimy do zakładki Save & Exit gdzie zapisujemy ustawienia i restartujemy komputer.

## Dostęp do katalogu Boot 
 
 W następstwie błędu programistycznego może się zdarzyć, że sterownik przejdzie w tryb Exception. Oznaką tego jest zatrzymanie wykonywania programu PLC oraz zapalenie się diody TC na sterowniku na żółto. Jeśli w projekcie została zaznaczona opcja *Autostart boot project* to może się zdarzyć, że sterownik wpadnie w nieskończoną pętlę błędu. W takim przypadku, aby uniemożliwić uruchomienie programu PLC możemy zmienić nazwę katalogu Boot.
 TwinCAT uruchamia się wtedy w trybie Config, a program PLC nie jest wykonywany. W przypadku gdy nie możemy bezpośrednio odczytać dysku sterownika do zmiany nazwy wyżej wymienionego katalogu możemy wykorzystać BST. W tym celu należy zbootować BST na sterowniku jak opisano to w poprzednich rozdziałach a następnie postępować zgodnie z opisem poniżej, w zależności od wersji TwinCAT znajdującej się na sterowniku.
 
### Postępowanie dla TwinCAT 4024

Po zbootowaniu sterownika z BST:

 - klikamy przycisk **Manage Images**:
 
![bst30](https://ba-pl.github.io/wiki/assets/images/BST/bst30.png "bst30")
 
 - następnie **Browse**:
 
 ![bst31](https://ba-pl.github.io/wiki/assets/images/BST/bst31.png "bst31")
  
- następnie przechodzimy do domyślnej lokalizacji TwinCAT'a
   - **C:\TwinCAT\3.1** 

i odszukujemy folder **Boot**. Klikamy PPM i zmieniamy nazwę:

![bst32](https://ba-pl.github.io/wiki/assets/images/BST/bst32.png "bst32")


### Postępowanie dla TwinCAT 4026

Po zbootowaniu sterownika z BST:

 - klikamy przycisk **Settings**:

![bst34](https://ba-pl.github.io/wiki/assets/images/BST/bst34.png "bst34")

- w zakładce **Linked Buttons** dodajemy nowy przycisk ze ścieżką do cmd, jak  na zdjeciu poniżej

![bst35](https://ba-pl.github.io/wiki/assets/images/BST/bst35.png "bst35")

- po powrocie do ekranu głównego mozemy otworzyć **cmd**

![bst36](https://ba-pl.github.io/wiki/assets/images/BST/bst36.png "bst36")

- wpisujemy kolejno komendy:
	- C:
	- cd C:\ProgramData\Beckhoff\TwinCAT\3.1
	- ren Boot oldBoot
	
![bst37](https://ba-pl.github.io/wiki/assets/images/BST/bst37.png "bst37")

Nazwa folderu Boot została zmieniona.


	