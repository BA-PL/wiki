---
title: Odblokowanie pulpitu zdalnego 
parent: Windows Embedded Compact
nav_order: 1
layout: page
---


# Odblokowanie pulpitu zdalnego CERHOST 
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Niniejsza dokumentacja opisuje, w jaki sposób odblokować funkcję pulpitu zdalnego dla sterowników z systemem operacyjnym Windows CE. Odblokowania można dokonać na trzy sposoby: poprzez Device Manager dostępny przez przeglądarkę internetową, poprzez wpis w rejestrach oraz poprzez blok funkcyjny wywołany w programie PLC.
<br>
Dostęp poprzez pulpit zdalny do sterowników z Windowsem CE możliwy jest dzięki programowi CERHOST, który można pobrać [tutaj.](https://infosys.beckhoff.com/content/1033/cx51x0_hw/Resources/5047075211.zip)

# Odblokowanie programu CERHOST poprzez przeglądarkę internetową 
Aby odblokować program CERHOST za pomocą przeglądarki internetowej, należy w przeglądarce wpisać https://**adres_IP_sterownika**/config lub http://**adres_IP_sterownika**/config

![cer1](https://ba-pl.github.io/wiki/assets/images/cer1.png "cer1")

Pojawi się okno logowania. Dane do logowania są następujące:
<br>
Dla TwinCAT 3:
- wersja 3.1.4024.0 i nowsze: Username: **Administrator**, Hasło: **1**
- wersje starsze: Username: **webguest** lub **guest**, Hasło: **1**

Dla TwinCAT 2:
- wersja 2.11.2302 i nowsze: Username: **Administrator**, Hasło: **1**
- wersje starsze: Username: **webguest** lub **guest**, Hasło: **1**
<br>

![cer2](https://ba-pl.github.io/wiki/assets/images/cer2.png "cer2")

Po zalogowaniu pojawi się strona menagera urządzenia (tzw. Device Manager):

![cer3](https://ba-pl.github.io/wiki/assets/images/cer3.png "cer3")

Należy wybrać zakładkę *Boot Options (1)*, a następnie w ustawieniach *Remote Display*, przełączyć jego aktywność na *On(2)* i zatwierdzić zmiany(3). Po wykonaniu tej czynności zostaniemy zapytani czy wykonać restart sterownika, co należy potwierdzić.
# Odblokowanie programu CERHOST poprzez wykonanie wpisu do rejestru
Aby odblokować CERHOST tą metodą, należy podłączyć monitor i wykonać wpis do rejestru o nazwie CeRemoteDisplay_Enable (dukrotne kliknicię lewym klawiszem myszy na pliku). Ścieżka dostępu do pliku: **Hard Disk\RegFiles\Samples\Common** 

![cer4](https://ba-pl.github.io/wiki/assets/images/cer4.png "cer4")
# Odblokowanie programu CERHOST za pomocą bloku funkcyjnego
Do odblokowania programu CERHOST został stworzony specjalny blok funkcyjny **FB_CERHOST** znajdujący się w projekcie **Demo**, który można pobrać [tutaj.](https://github.com/BA-PL/Demo/archive/refs/heads/main.zip)
<br>
Po wykonaniu algorytmu bloku funkcyjnego FB_CERHOST nastąpi **automatyczny restart sterownika**.

![cer5](https://ba-pl.github.io/wiki/assets/images/cer5.png "cer5")

**UWAGA!** Nie należy ustawiać stałej wartości TRUE na wejściu *bEnableCERHOST*

![cer5_1](https://ba-pl.github.io/wiki/assets/images/cer5_1.png "cer5_1")

# Zapobieganie zablokowaniu CERHOST
Program CERHOST na sterownikach jest domyślnie zablokowany. Blokada ma za zadanie zapobiegać niepożądanym bądź przypadkowym działaniom. Aby zapobiec jego blokadzie, należy przed pierwszym uruchomieniem (lub po wgraniu nowego obrazu) z folderu zawierającego obraz, usunąć wpis do rejestru o nazwie CeRemoteDisplay_Disable. Wpis ten znajduje się w podfolderze RegFiles, którego pliki wykonują się przy pierwszym uruchomieniu sterownika lub po przywróceniu ustawień fabrycznych. Ścieżka dostępu jest następująca:
<br>
**(katalog główny obrazu) --> RegFiles**

![cer6](https://ba-pl.github.io/wiki/assets/images/cer6.png "cer6")
