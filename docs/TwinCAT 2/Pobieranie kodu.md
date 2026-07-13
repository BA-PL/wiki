---
title: Pobieranie kodu źródłowego (PLC Control)
parent: TwinCAT 2
nav_order: 2
layout: page
---


# Pobieranie kodu żródłowego ze sterownika za pomocą TwinCAT PLC Control
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Poniższa instrukcja prezentuje w jaki sposób pobrać kod źródłowy ze sterownika wyposażonego w TwinCAT 2 a także pokazuje, jakie problemy może napotkać użytkownik i jaka może być ich przyczyna.
Instrukcja w żaden sposób nie zastępuje szkolenia i nie może być tak traktowana, ponieważ może zawierać informacje i skróty myślowe niejasne dla osoby, która tego szkolenia nie przeszła.

# Pobieranie kodu źródłowego
Aby móc pobrać kod źródłowy należy najpierw nawiązać ze sterownikiem połączenie (w TwinCAT System Manager). Następnie uruchamiamy środowisko TwinCAT PLC Control:

![plc1](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc1.png "plc1")

Następnie wybrać z górnego paska opcję File --> Open, a z okna które się pojawi opcję **PLC**:

![plc2](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc2.png "plc2")

![plc3](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc3.png "plc3")

Kolejnym krokiem jest wybranie odpowiedniego typu urządzenia:

![plc4](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc4.png "plc4")

- **PC or CX (x86)**: opcja używana do połączenie wszystkich komputerów klasy PC oraz sterowników z architekturą procesora x86 np. sterowniki z serii CX20xx, CX5xxx, panele z serii CP22xx, wszystkie komputery.
- **CX (ARM)**: opcja używana do połączenia urządzeń z architekturą procesora ARM np. z serii CX8xxx, CX9xxx lub panele CP66xx.
- **BCxx50 or BX via AMS**: opcja może być używana do sterowników z serii BCxx50, BCxx20 oraz BX.
- **BCxx50 or BX via serial**: opcja używana do łączenia ze sterownikami przez port szeregowy. Niezalecany w nowych aplikacjach. Opcja może być używana do sterowników z serii BCxx50, BCxx20 oraz BX.
- **BC via serial i BC via AMS**: analogicznie jak wyżej dla sterowników BCxx00
<br>

oraz dalej wskazujemy Runtime (czyli numer portu PLC, na którym uruchomiona jest aplikacja). Zazwyczaj jest to Runtime 1 (port 801).
<br>
<br>
**Należy pamiętać o tym, że na sterowniku może być uruchomionych kilka projektów, a z każdego Runtime projekt należy pobrać oddzielnie!**

![plc5](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc5.png "plc5")

Po poprawnym wykonaniu komendy kody źródłowe zostaną pobrane:

![plc6](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc6.png "plc6")

**Uwaga!** Plik pobiera się jako **Untitled (bez nazwy)**, należy więc go zapisać poprzez wybranie *File --> Save As…*, najlepiej pod nazwą taką, z jaką jest zlinkowany w zakładce PLC w System Manager!

![plc7_1](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc7_1.png "plc7_1")

# Możliwe problemy 

## Brak kodu
Najczęstszym błędem jest błąd mówiący o tym, że operacja pobrania nie może zostać wykonana poprawnie:

![plc8](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc8.png "plc8")

Co dalej prowadzi do błędu:

![plc9](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc9.png "plc9")

W tym przypadku pobranie kodu nie będzie możliwe, jako że kopia kodów źródłowych nie została wgrana na sterownik wraz z aplikacją. Na sterowniku znajduje się wtedy jedynie skompilowana wersja aplikacji. 

## Nieodpowiedni typ sterownika
W przypadku wybrania nieodpowiedniego typu sterownika pojawi się błąd:

![plc10](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc10.png "plc10")

Oznacza to, że system TwinCAT rozpoznał, że wybrany został w trakcie procedury pobierania kodu, zły typ urządzenia. Należy zamknąć środowisko i powtórzyć operację od początku, wybierając poprawny typ sterownika na tym etapie:

![plc4](https://ba-pl.github.io/wiki/assets/images/PLCControl/plc4.png "plc4")

## Hasło do aplikacji
Zdarza się, że aplikacja znajdująca się na sterowniku ma założone hasło (nadane przez pierwotnego autora apliakcji PLC - Beckhoff nie jest w stanie go usunąć). Wtedy nie będziemy w stanie w pełni odczytać/edytować kodów źródłowych bez znajomości tego hasła. 

# Dodatek: Opis plików w folderze TwinCAT\Boot

Każdy sterownik wyposażony w TwinCAT 2 przechowuje całą aplikację oraz plik konfiguracyjny w ściśle określonym miejscu, jakim jest folder TwinCAT\Boot. Poniżej przedstawiono opis plików znajdujących się w tym folderze.

![boot1](https://ba-pl.github.io/wiki/assets/images/PLCControl/boot1.png "boot1")

- Plik CurrentConfig.TSM: zawiera konfiugrację sprzętową, którą następnie po pobraniu można otworzyć w TwinCAT System Manager
- Pliki CurrentConfigX.bin: pliki binarne powstałe w momencie kompilacji aplikacji
- Pliki z rozszerzeniem \*.wbp: pliki zawierające: 
	- projekt bootowalny (TCPLC_**P**):  brak tego pliku będzie powodować że aplikacja nie uruchomi się automatycznie wraz ze sterownikiem
	- kody źródłowe (TCPLC_**S**): brak tego pliku uniemożliwi pobranie kodów źródłowych
	- zmienne Persistent (TCPLC_**T**): brak tego pliku nie załaduje poprawnie zmiennych nieulotnych typu Persistent 
	- zmienne Retain (TCPLC_**R**): brak tego pliku nie załaduje poprawnie zmiennych nieulotnych typu Retain
	
<br>

Opis poszczególnych plików na zrzucie poniżej:

![boot2](https://ba-pl.github.io/wiki/assets/images/PLCControl/boot2.png "boot2")