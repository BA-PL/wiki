---
title: Przywracanie ustawień fabrycznych 
parent: Windows Embedded Compact
nav_order: 2
layout: page
---


# Przywracanie ustawień fabrycznych na Windows CE
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Ustawienia fabryczne na urządzeniach z Windows Embedded Compact można przywrócić na kilka sposobów. Wybór metody zależy od tego czy umożliwiony jest fizyczny dostęp do karty pamięci urządzenia czy możliwe są tylko zdalne operacje. 

# Przywracanie ustawień poprzez wgranie fabrycznego obrazu na kartę pamięci 
Aby  przywrócić  ustawienia  fabryczne  tą  metodą,  wymagane  jest posiadanie odpowiedniego czytnika kart.
<br>
W pierwszej kolejności należy pobrać fabryczny obraz systemu, który znajduje się  [tutaj.](https://download.beckhoff.com/download/software/embPC-Control) Na wyświetlonej liście należy odnaleźć symbol odpowiedniego urządzenia oraz wybrać odpowiedni system Windows. Przykładowo dla sterownika CX9020 z TC3, ścieżka wygląda następująco: 

![ce1](https://ba-pl.github.io/wiki/assets/images/ce1.png "ce1")

Następnie, przy odłączonym zasilaniu, należy wyjąć kartę ze urządzenia (informacje gdzie znajduje się karta i jak ją wyjąć można znaleźć w dokumentacji danego sterownika na stronie [beckhoff.](https://beckhoff.com) Po odczytaniu karty należy usunąć całą jej zawartość potwierdzając pojawiające się komunikaty dotyczące usuwania plików systemowych.
<br>
**UWAGA!** Karta nie wymaga formatowania. Formatowanie robimy tylko w przypadku nowej karty, wybierając system plików FAT16 lub FAT32
<br>
<br>
Po  wyczyszczeniu  karty  należy  wgrać  na  nią  pobrany  wcześniej fabryczny obraz. Robimy to kopiując zawartość folderu z obrazem.

![ce2](https://ba-pl.github.io/wiki/assets/images/ce2.png "ce2")

Po wgraniu obrazu, należy zamknąć folder z zawartością karty i bezpiecznie usnąć ją z komputera.

![ce3](https://ba-pl.github.io/wiki/assets/images/ce3.png "ce3")

Po  włożeniu  karty  do  sterownika  i  włączeniu  zasilania  uruchomi  się  on  z ustawieniami fabrycznymi.
# Poprzez  usunięcie  folderu  *Documents  and  Settings*  oraz  plików  z folderu *Boot* na karcie pamięci.
Po odczycie karty ze sterownika (kroki analogicznie jak w poprzednim rozdziale) należy usunąć folder Documents and Settings (przywrócona zostanie w ten sposób m.in. pierwotna nazwa sieciowa sterownika oraz sposób pobierania adresu IP ).

![ce4](https://ba-pl.github.io/wiki/assets/images/ce4.png "ce4")

Następnie należy odnaleźć zawartość folderu Boot (**TwinCat --> Boot** w przypadku TC2, **TwinCat --> 3.1 --> Boot**  w przypadku TC3)
i usunąć wszystkie pliki poza folderem WebVisu:

![ce5](https://ba-pl.github.io/wiki/assets/images/ce5.png "ce5")
# Poprzez logowanie do panelu konfiguracyjnego sterownika 
Sterowniki posiadają panel konfiguracyjny do którego można się zalogować przez przeglądarkę. Aby to zrobić należy w przeglądarce wpisać adres IP sterownika z  dopiskiem  **/config**. Np.  10.24.2.43/config.

![ce6](https://ba-pl.github.io/wiki/assets/images/ce6.png "ce6")

Pojawi się okno logowania. Dane do logowania są następujące:
- Użytkownik: **Administrator** ( w przypadku straszych wersji systemu **Guest** lub **webguest**)
- Hasło: **1**

![ce7](https://ba-pl.github.io/wiki/assets/images/ce7.png "ce7")

Po zalogowaniu pojawi się strona menagera urządzenia. Należy przejść do zakładki *Device(1)*, następnie *Boot Opt.(2)* i wybrać *Restore Setings(3)* a następnie zrestartować sterownik potwierdzając pojawiający się komunikat. 

![ce8](https://ba-pl.github.io/wiki/assets/images/ce8.png "ce8")

![ce9](https://ba-pl.github.io/wiki/assets/images/ce9.png "ce9")

Po  potwierdzeniu  komunikatu  nastąpi  restart  sterownika  i  przywrócenie ustawień fabrycznych. Przywrócona zostanie w ten sposób m.in. pierwotna nazwa sieciowa sterownika oraz sposób pobierania adresu IP.
<br>
<br>
**UWAGA!**
<br>
Nie  zostanie  usunięty  kod  źródłowy,  plik  z  projektem  bootowalnym, konfiguracją oraz informacje dotyczące zmiennych nieulotnych. Aby je usunąć można wykorzystać jedną z metod opisanych w tym dokumencie.
# Za pomocą aplikacji CERHOST
Przywracanie ustawień fabrycznych za pomocą aplikacji CERHOST jest jedną ze zdalnych metod. Aplikacja ta nie wymaga instalacji i można ją pobrać [tutaj.](https://infosys.beckhoff.com/content/1033/cx51x0_hw/Resources/5047075211.zip)
<br>
<br>
Domyślnie  zdalny  pulpit  (CE  Remote  Host  --> CERHOST)  jest  na sterownikach  zablokowany. Odblokować  go  można  za  pomocą  opisanego wcześniej  panelu  konfiguracyjnego  w  zakładce  *Boot  Opt.*,  przełączając ustawienie *Remote Display* z **Off** na **On** i restartując sterownik. Szerszy opis [tutaj.](https://infosys.beckhoff.com/english.php?content=../content/1033/cx51x0_hw/6436348939.html)
<br>
Po  odblokowaniu  i  pobraniu  programu,  otwieramy  go,  a  następnie wybieramy **File --> Connect**

![ce10](https://ba-pl.github.io/wiki/assets/images/ce10.png "ce10")

Program prosi nas o **Hostname** w miejsce którego wpisujemy Adres IP naszego sterownika i klikamy OK zostawiając okienko **Password** puste.

![ce11](https://ba-pl.github.io/wiki/assets/images/ce11.png "ce11")

Otwiera nam się pulpit urządzenia, z którego wybieramy START --> RUN

![ce12](https://ba-pl.github.io/wiki/assets/images/ce12.png "ce12")

W okienku RUN wpisujemy "explorex.exe" lub znak backslash:

![ce13](https://ba-pl.github.io/wiki/assets/images/ce13.png "ce13")

Z przeglądarki  plików  wybieramy  folder **Hard  Disk**,  po  jego  otwarciu odszukujemy folder **Documents and Settings** i zmieniamy jego nazwę na inną.

![ce14](https://ba-pl.github.io/wiki/assets/images/ce14.png "ce14")

Następnie, analogicznie jak opisano w poprzednich rozdziałach, usuwamy zawartość folderu **Boot**.
<br>
Po zmianie nazwy folderu i wyczyszczeniu zawartości folderu Boot, restartujemy sterownik. W tym celu należy wybrać START --> RESET.

![ce15](https://ba-pl.github.io/wiki/assets/images/ce15.png "ce15")

Po restarcie sterownika, system utworzy nowy folder **Documents and Settings**, a folder ze zmienioną nazwą należy usunąć.
