---
title: Pulpit zdalny CERHOST 
parent: Windows Embedded Compact 
nav_order: 1
layout: page
---


# CE Remote Host - CERHOST 
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# CERHOST - wprowadzenie

Dostęp poprzez pulpit zdalny do sterowników z Windowsem CE możliwy jest dzięki programowi CERHOST, który można pobrać [tutaj.](https://infosys.beckhoff.com/content/1033/cx51x0_hw/Resources/5047075211.zip)

![cer0](https://ba-pl.github.io/wiki/assets/images/cerhost/cer0.png "cer0")

Jeśli podczas próby połączenia pojawi się komunikat błędu **Can't connect** oznacza to, że dostęp zdalny za pomocą CERHOST jest na sterowniku zablokowany (domyślne ustawienie). Można go odblokować jedną z metod opisanych w kolejnych rozdziałach.
Po udanym połączeniu zobaczymy pulpit sterownika:

![ce1](https://ba-pl.github.io/wiki/assets/images/cerhost/ce1.png "ce1")

# Odblokowanie CERHOST

## Odblokowanie programu CERHOST poprzez przeglądarkę internetową 
Aby odblokować program CERHOST za pomocą przeglądarki internetowej, należy w przeglądarce wpisać http(s)://**adres_IP_sterownika**/config lub http(s)://**hostname_sterownika**/config

![cer1](https://ba-pl.github.io/wiki/assets/images/cerhost/cer1.png "cer1")

Pojawi się okno logowania. Dane do logowania są następujące:
<br>
Dla TwinCAT 3:
- wersja 3.1.4024.0 i nowsze: Username: **Administrator**, Hasło: **1**
- wersje starsze: Username: **webguest** lub **guest**, Hasło: **1**

Dla TwinCAT 2:
- wersja 2.11.2302 i nowsze: Username: **Administrator**, Hasło: **1**
- wersje starsze: Username: **webguest** lub **guest**, Hasło: **1**
<br>

![cer2](https://ba-pl.github.io/wiki/assets/images/cerhost/cer2.png "cer2")

Po zalogowaniu pojawi się strona menagera urządzenia (tzw. Device Manager):

![cer3x](https://ba-pl.github.io/wiki/assets/images/cerhost/cer3x.png "cer3x")

Należy wybrać zakładkę *Boot Options (1)*, a następnie w ustawieniach *Remote Display*, przełączyć jego aktywność na *On(2)* i zatwierdzić zmiany(3). Po wykonaniu tej czynności zostaniemy zapytani czy wykonać restart sterownika, co należy potwierdzić.
## Odblokowanie programu CERHOST poprzez wykonanie wpisu do rejestru
Aby odblokować CERHOST tą metodą, należy podłączyć monitor i wykonać wpis do rejestru o nazwie CeRemoteDisplay_Enable (dwukrotne kliknięcie lewym klawiszem myszy na pliku). Ścieżka dostępu do pliku: **Hard Disk\RegFiles\Samples\Common** 

![cer4](https://ba-pl.github.io/wiki/assets/images/cerhost/cer4.png "cer4")
## Odblokowanie programu CERHOST za pomocą bloku funkcyjnego
Do odblokowania programu CERHOST został stworzony specjalny blok funkcyjny **FB_CERHOST** znajdujący się w projekcie **Demo**, który można pobrać [tutaj.](https://github.com/BA-PL/Demo-TC3/archive/refs/heads/main.zip)
<br>
Po wykonaniu algorytmu bloku funkcyjnego FB_CERHOST nastąpi **automatyczny restart sterownika**.

![cer5](https://ba-pl.github.io/wiki/assets/images/cerhost/cer5.png "cer5")

**UWAGA!** Nie należy ustawiać stałej wartości TRUE na wejściu *bEnableCERHOST*

![cer5_1](https://ba-pl.github.io/wiki/assets/images/cerhost/cer5_1.png "cer5_1")

## Zapobieganie zablokowaniu CERHOST
Program CERHOST na sterownikach jest domyślnie zablokowany. Blokada ma za zadanie zapobiegać niepożądanym bądź przypadkowym działaniom. Aby zapobiec jego blokadzie, należy przed pierwszym uruchomieniem (lub po wgraniu nowego obrazu) z folderu zawierającego obraz, usunąć wpis do rejestru o nazwie **CeRemoteDisplay_Disable**. Wpis ten znajduje się w podfolderze RegFiles, którego pliki wykonują się przy pierwszym uruchomieniu sterownika lub po przywróceniu ustawień fabrycznych. Ścieżka dostępu jest następująca:
<br>
**(katalog główny obrazu) --> RegFiles**

![cer6](https://ba-pl.github.io/wiki/assets/images/cerhost/cer6.png "cer6")
