---
title: Import składowych projektu
parent: Projekt TwinCAT
nav_order: 4
layout: page
---


# Import składowych projektu w TwinCAT 3
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

**Import elementów  projektu PLC utworzonych poza aktualnym rozwiązaniem otwartym w  TwinCAT 3**
<br>
Niniejsza instrukcja opisuje sposoby importu składowych projektu PLC w środowisku TwinCAT 3. Opisuje ona 3 podstawowe sposoby importu:
- import z archiwum \*.zip
- import poprzez dodanie istniejącego elementu
- import poprzez drag & drop

# Import z archiwum \*.zip
W celu importu programów z katalogu .zip należy kliknąć PPM na nazwie **projektu PLC**, a następnie wybrać opcję **Import from .zip:**

![import1](https://ba-pl.github.io/wiki/assets/images/import/import1.png "import1")

Następnie należy wybrać odpowiednie archiwum .zip (w przypadku tej instrukcji będzie to archiwum *Diag.zip*):

![import2](https://ba-pl.github.io/wiki/assets/images/import/import2.png "import2")

Po wybraniu odpowiedniego pliku .zip i kliknięciu **Open** pojawi się okno ZipImportDialog, w którym można wybrać poszczególne elementy archiwum, które zaimportowane zostaną do projektu PLC:

![import3](https://ba-pl.github.io/wiki/assets/images/import/import3.png "import3")

Po zaimportowaniu folder **Diag** dodał się do rozwiązania i znajduje się w drzewie projektu pod węzłem **PLC:**

![import4](https://ba-pl.github.io/wiki/assets/images/import/import4.png "import4")

# Import istniejących elementów projektu PLC

## Import pojedynczego elementu
W celu importu istniejących elementów do projektu należy kliknąć PPM na nazwie **odpowiedniego folderu do którego w chcemy zaimportować element** a następnie wybrać **Add... --> Existing Item**(można zaznaczyć kilka elementów jednocześnie przytrzymując klawisz **Ctrl**):

![import5](https://ba-pl.github.io/wiki/assets/images/import/import5.png "import5")

Następnie należy wybrać żądany element i kliknąć **Add:**

![import6](https://ba-pl.github.io/wiki/assets/images/import/import6.png "import6")

Element zostanie zaimportowany:

![import7](https://ba-pl.github.io/wiki/assets/images/import/import7.png "import7")

## Import całej zawartości folderu
W celu importu istniejących elementów do projektu należy kliknąć PPM na nazwie **odpowiedniego folderu do którego w chcemy zaimportować zawartość** a następnie wybrać **Add... --> Existing Folder Content**:

![import8](https://ba-pl.github.io/wiki/assets/images/import/import8.png "import8")

Następnie należy wybrać folder i klikąć **Select Folder**:

![import9](https://ba-pl.github.io/wiki/assets/images/import/import9.png "import9")

Elementy zostaną zaimportowane:

![import10](https://ba-pl.github.io/wiki/assets/images/import/import10.png "import10")

# Import całego projektu PLC
Istnieje również możliwość zaimportowania całego projektu PLC z innego rozwiązania. Można to zrobić na dwa sposoby: poprzez import pliku \*.plcproj lub poprzez import pliku \*.tpzip.
## Import pliku \*.plcproj
Plik \*.plcproj jest plikiem tworzonym domyślnie podczas edycji projektu PLC. Aby dodać cały projekt PLC, który powstał przy innym solution należy **na zakładce PLC** w drzewie projektu kliknąć PPM i wybrać **Add Existing Item**:

![import11](https://ba-pl.github.io/wiki/assets/images/import/import11.png "import11")

Następnie należy odszukać interesujący nas plik \*.plcproj i wybrać **Open.**

![import12](https://ba-pl.github.io/wiki/assets/images/import/import12.png "import12")

Następnie trzeba wybrać czy projekt zostanie skopiowany, przeniesiony, czy zmiany dokonywane będą w oryginalnej lokalizacji projektu:

![import13](https://ba-pl.github.io/wiki/assets/images/import/import13.png "import13")

Po zakończeniu kopiowania/przenoszenia projekt pojawi się w drzewku i będzie można go otworzyć i na nim operować:

![import14](https://ba-pl.github.io/wiki/assets/images/import/import14.png "import14")
## Import z archiwum \*.tpzip
Jeżeli dostępny jest plik \*.tpzip (czyli archiwum zawierające projekt PLC), można zaimportować projekt PLC z tegoż archiwum. W tym celu należy **na zakładce PLC w drzewie projektu** kliknąć PPM i wybrać **Add Existing Item:**

![import15](https://ba-pl.github.io/wiki/assets/images/import/import15.png "import15")

Następnie należy odszukać interesujący nas plik \*.tpzip i wybrać Open:

![import16](https://ba-pl.github.io/wiki/assets/images/import/import16.png "import16")

Projekt zostanie zaimportowany i będzie widoczny w drzewku rozwiązania:

![import17](https://ba-pl.github.io/wiki/assets/images/import/import17.png "import17")

# Import całego projektu
Oprócz importowania poszczególnych składowych czy całego projektu PLC można również zaimportować całe projekty utworzone w TwinCAT np. na innym komputerze (TwinCAT Project, HMI Project, Measurement Project itp.). Dokonuje się tego poprzez import pliku \*.tsproj, \*.hmiproj, \*.tcmproj i analogicznych dla innych projektów plików.
## Import pliku \*.tsproj i analogicznych
W celu importu całego projektu z innej lokalizacji należy **w drzewie projektu** kliknąć **PPM na Solution** i wybrać **Add --> Existing project**:

![import18](https://ba-pl.github.io/wiki/assets/images/import/import18.png "import18")

Następnie odszukać interesujący nas plik projektu, wybrać go i klikąć **Open:**

![import19](https://ba-pl.github.io/wiki/assets/images/import/import19.png "import19")

Po zaimportowaniu w solution znajdują się teraz dwa projekty:

![import20](https://ba-pl.github.io/wiki/assets/images/import/import20.png "import20")

## Import pliku \*.tszip.
W celu importu całego projektu z innej lokalizacji należy w **drzewie projektu** kliknąć **PPM na Solution** i wybrać **Add --> Existing project:**

![import21](https://ba-pl.github.io/wiki/assets/images/import/import21.png "import21")

Następnie odszukać interesujący nas plik projektu, wybrać go i klikąć **Open:**

![import22](https://ba-pl.github.io/wiki/assets/images/import/import22.png "import22")

Następnie należy wybrać folder do którego zostanie wypakowane archiwum z projektem PLC i kliknąć **Select Folder:**

![import23](https://ba-pl.github.io/wiki/assets/images/import/import23.png "import23")

Po zaimportowaniu w solution znajdują się teraz dwa projekty:

![import24](https://ba-pl.github.io/wiki/assets/images/import/import24.png "import24")

# Import całego Solution (\*.tnzip)
Istnieje również możliwość zaimportowania całego Solution. Dokonuje się tego poprzez import z archiwum \*.tnzip, czyli archiwum zawierającego spakowane całe rozwiązanie (czyli wszystkie projekty, które zostały do niego dodane).
<br>
W celu importu całego solution należy w głównym oknie TwinCATA wybrać **File --> Open --> Open solution from Archive:**

![import25](https://ba-pl.github.io/wiki/assets/images/import/import25.png "import25")

Następnie odszukać interesujący nas plik projektu, wybrać go i klikąć **Open:**

![import26](https://ba-pl.github.io/wiki/assets/images/import/import26.png "import26")

Następnie należy wybrać folder do którego zostanie wypakowane archiwum z Solution i kliknąć **Select Folder:**

![import27](https://ba-pl.github.io/wiki/assets/images/import/import27.png "import27")

Po wypakowaniu otworzy się uprzednio spakowane solution zawierające jeden lub kilka projektów:

![import28](https://ba-pl.github.io/wiki/assets/images/import/import28.png "import28")

# Metoda drag & drop

## Drag & drop z folderu do projektu
Metoda **drag & drop** (ang. przeciągnij i upuść) jest najszybszym sposobem importu składowych projektu.
Polega ona zgodnie z nazwą na przeciągnięciu elementu z eksploratora plików w systemie Windows bezpośrednio na odpowiedni folder w drzewie projektu, do którego dany element chcemy zaimportować (można zaznaczyć kilka elementów jednocześnie przytrzymując klawisz **Ctrl**):

![import29](https://ba-pl.github.io/wiki/assets/images/import/import29.png "import29")

Elementy zostaną zaimportowane:

![import30](https://ba-pl.github.io/wiki/assets/images/import/import30.png "import30")

## Drag & drop między projektami
Oprócz przeciągania składowych projektu z folderu do Solution, istnieje również możliwość przeciągania składowych pomiędzy dwoma projektami. Jest to najszybszy sposób na wyizolowanie części projektu czy też przekopiowanie konkretnych elementów z jednego projektu do drugiego. Aby tego dokonać, należy w projekcie źródłowym zaznaczyć jeden lub kilka (przytrzymując klawisz **Ctrl**) elementów, a następnie przeciągnąć je i upuścić na wybranym poziomie drzewka projektu w projekcie docelowym:

![import31](https://ba-pl.github.io/wiki/assets/images/import/import31.png "import31")

Elementy zostaną przekopiowane i zaimportowane do projektu docelowego:

![import32](https://ba-pl.github.io/wiki/assets/images/import/import32.png "import32")
