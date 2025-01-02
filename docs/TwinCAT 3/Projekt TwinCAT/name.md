---
title: Zmiana nazwy projektu 
parent: Projekt TwinCAT
nav_order: 6
layout: page
---


# Zmiana nazwy Solution i projektów 
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Poniższa instrukcja opisuje zalecaną metodę zmiany nazwy Solution i składowych projektów TwinCAT. Należy postępować w takiej kolejności, w jakiej ułożone są kolejne rozdziały.

# Zmiana nazwy projektu PLC (.plcproj)

![name1](https://ba-pl.github.io/wiki/assets/images/name/name1.png "name1")

Zmianę nazw projektów można zacząć od PLC. W przykładzie nazwy ‘Project1’ i ‘Project2’ zostaną zmienione odpowiednio na nazwy ‘Pierwszy’ i ‘Drugi’. W celu zmiany nazwy, należy kliknąć PPM na wybrany projekt, wybrać **Rename** i wprowadzić nową nazwę:

![name2](https://ba-pl.github.io/wiki/assets/images/name/name2.png "name2")

Podczas zmiany nazwy pojawi się komunikat:

![name3](https://ba-pl.github.io/wiki/assets/images/name/name3.png "name3")

Oznacza to, że konsekwencją zmiany nazwy jest utrata online change dopóki nie wgramy programu na PLC. Jeśli chcemy dokończyć proces zmiany nazwy, komunikat należy potwierdzić. W efekcie zmieni się nazwa projektu PLC:

![name4](https://ba-pl.github.io/wiki/assets/images/name/name4.png "name4")

# Zmiana nazwy projektu TwinCAT (.tsproj)
Aby zmienić nazwę projektu TwinCAT należy kliknąć PPM i wybrać opcję **Rename**. W opisywanym przykładzie nazwa ‘Test’ została zmieniona na nazwę ‘Przykład’.

![name5](https://ba-pl.github.io/wiki/assets/images/name/name5.png "name5")

Zmiana nazwy powoduje zmianę projektu w drzewku oraz nazwy pliku projektu (.tsproj) w folderze projektu. Aby zmienić również nazwę folderu projektu należy wykonać to ręcznie i poprawić odwołanie do pliku projektu w pliku solucji (.sln), co opisano w dalszych krokach instrukcji.

# Zmiana nazwy solucji (.sln)
Aby zmienić nazwę solucji, podobnie jak poprzednio klikamy PPM i wybieramy opcję **Rename**. Wprowadzamy nową nazwę, a po zatwierdzeniu zmieni się nazwa solucji:

![name6](https://ba-pl.github.io/wiki/assets/images/name/name6.png "name6")

Zmiana nazwy ponownie nie zmienia nazwy folderu solucji lecz elementu w drzewku oraz nazwy pliku solucji (.sln) w folderze solucji. Aby zmienić również nazwę folderu projektu należy wykonać to ręcznie, co opisano w dalszych krokach instrukcji.

#  Zmiana nazwy folderu solucji i projektu TwinCAT
Na początku zapisujemy wszystko – opcja **Save All**, po czym przechodzimy do folderu solucji.

![name7](https://ba-pl.github.io/wiki/assets/images/name/name7.png "name7")

Jeśli nie pamiętamy ścieżki do folderu, można zastosować jedną z poniższych metod:
- Najprościej kliknąć PPM na nazwie solucji i wybrać opcję

![name8](https://ba-pl.github.io/wiki/assets/images/name/name8.png "name8")

- Drugą opcją jest przejście do właściwości solucji, podejrzenie ścieżki do pliku i ręczne przejście do katalogu

![name9](https://ba-pl.github.io/wiki/assets/images/name/name9.png "name9")

![name10](https://ba-pl.github.io/wiki/assets/images/name/name10.png "name10")

**UWAGA!** Na tym etapie konieczne jest zamknięcie solucji, gdyż będziemy edytować pliki projektu!
<br>
<br>
Zacznijmy od zmiany nazwy folderu, w którym znajduje się plik solucji (.sln)

![name11](https://ba-pl.github.io/wiki/assets/images/name/name11.png "name11")

Następnie wchodzimy do folderu i zmieniamy również nazwę folderu zawierającego projekt TwinCAT (.tsproj)

![name12](https://ba-pl.github.io/wiki/assets/images/name/name12.png "name12")

Zalecamy wprowadzić nazwę projektu TwinCAT:

![name13](https://ba-pl.github.io/wiki/assets/images/name/name13.png "name13")

W ostatnim kroku klikamy PPM na plik z rozszerzeniem .sln i edytujemy go np. za pomocą notatnika.

![name14](https://ba-pl.github.io/wiki/assets/images/name/name14.png "name14")

W otwartym pliku należy dokonać zmiany w ścieżce do pliku projektu TwinCAT (.tsproj).
<br>
<br>
Przykładowo z:

![name15](https://ba-pl.github.io/wiki/assets/images/name/name15.png "name15")

Na:

![name16](https://ba-pl.github.io/wiki/assets/images/name/name16.png "name16")

Zapisujemy zmiany w pliku. Po zmianie nazwy pliku solucji otwarcie projektu z listy ostatnich może nie być możliwe. Pojawi się komunikat:

![name17](https://ba-pl.github.io/wiki/assets/images/name/name17.png "name17")

Zatwierdzenie spowoduje usunięcie projektu z listy ostatnio otwieranych. Otwarcie projektu po zmianach przez menu **File --> Open** lub otwarcie pliku solucji (.sln) doda go ponownie do listy.



