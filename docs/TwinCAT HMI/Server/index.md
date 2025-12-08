---
title: Konfiguracja serwera
nav_order: 2
layout: page
parent: TwinCAT HMI
---

# Konfiguracja serwera i publikacja aplikacji 
{: .no_toc }
<h6> Data modyfikacji: 4.12.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Wersja 1.12 

## Instalacja 
Zaczynamy od pobrania i instalacji wersji 1.12 (wersja dla osób pracujących z narzędziem inżynierskim TwinCAT XAE Shell do wersji 4024 włącznie):
- na stronie [Beckhoff](https://www.beckhoff.com/pl-pl/) wyszukujemy frazę **TF2000**
- pobieramy instalator

![s1a](s1a.png "s1a")

- instalujemy produkt na urządzeniu docelowym (czyli tym, które ma hostować aplikację HMI; może być to inne urządzenie niż to, które poźniej będzie wizualizację wyśwetlać)
- po instalacji na pasku systemowym pojawi się ikona serwera

![s2](s2.png "s2")

## Generowanie licencji

Aby móc skonfigurować serwer HMI, musi być aktywna licencja TF2000 (może być to licencja trial).
Należy więc otworzyć dowolny projekt TwinCAT, nawiązać połączenie ze sterownkiem na którym znajduje się HMI server (wymagany XAR TwinCAT) i wygenerować licencję.

![s3](s3.png "s3")

![s4](s4.png "s4")

Po wygenerowaniu licencji należy przeładować stan TwinCATa (np. ponownie do Run lub Config), aby status licencji był poprawny.

![s5a](s5a.png "s5a")

## Utworzenie hasła

Przed rozpoczęciem docelowej konfiguracji serwera, należy go zrestartować:

![s6](s6.png "s6")

Następnie otwieramy panel konfiguracyjny:

![s7](s7.png "s7")

Za pierwszym razem należy ustawić hasło, które będzie potem używane do zmiany konfiguracji jak i publikacji projektu:

![s8](s8.png "s8")

Powinna otworzyć się strona konfiguracyjna - dalsze ustawienia robimy wedle potrzeb. 

![s9a](s9a.png "s9a")

## Dodanie wyjątku do zapory sieciowej

Aby serwer był widoczny dla innych urządzeń (m.in. dla komputera z narzędziem inżynierskim), należy dodać wyjątek do zapory.
Robimy to w krokach:
- otwieramy Panel Sterowania 

![s10](s10.png "s10")

- wyszukujemy ustawienia zapory i wybieramy opcję **Allow an app through Windows Firewalll**

![s11](s11.png "s11")

- klikamy przycisk **Allow another app**

![s12](s12.png "s12")

- następnie **Browse**

![s13](s13.png "s13")

- wskazujemy plik **TcHmiServer.exe** z lokalizacji **C:\TwinCAT\Functions\TF2000-HMI-Server**

![s14a](s14a.png "s14a")

Zatwierdzamy zmiany. 

## Publikowanie aplikacji 

Aby przesłać gotową aplikację z narzędzia inżynierskiego na docelowy serwer, klikamy PPM na nazwie projektu i wybieramy opcję *Publish to TwinCAT HMI Server*:

![s15a](s15a.png "s15a")

Nadajemy nazwę profilu, pod którą będą zapisane dane serwera:

![s16](s16.png "s16")

Następnie, wyszkujeemy dostępne w sieci serwery:

![s17a](s17a.png "s17a")

Wybieramy serwer z listy (sterownik):

![s18](s18.png "s18")

Uzupełniamy hasło i sprawdzamy połączenie:

![s19a](s19a.png "s19a")

Powienien pojawić się komunikat:

![s20](s20.png "s20")

W następnym kroku wykonujemy operację *Publish*:

![s21_1](s21_1.png "s21_1")

Jeśli pojawi się ostrzeżenie jak poniżej:

![s21a](s21a.png "s21a")

to można anualować proces, zmienić platfromę na TwinCAT HMI:

![s21b2](s21b2.png "s21b2")

i ponowić publikację. 
Status publikacji można obserwować w oknie *Output*:

![s22a](s22a.png "s22a")

Po zakończonej publikacji można otworzyć wizualizację na kilka sposobów:
- z zewnętrznego urządzenia podając adres: https://adresIP_serwera:1020

![s23](s23.png "s23")

- lokalnie 
	- na urządzeniu z serwerem: http://127.0.0.1:1010
	- kilkając na ikonę serwera i opcję *Start Page*
	
![s24](s24.png "s24")	

# Wersja 1.14

## Instalacja 

Instalujemy produkt na urządzeniu docelowym (czyli tym, które ma hostować aplikację HMI; może być to inne urządzenie niż to, które poźniej będzie wizualizację wyśwetlać).
<br>
Jeśli na sterowniku znjaduje się Runtime w wersji 4026 oraz Package Manager, instalujemy za pomocą Package Managera workload TF2000:

![s25](s25.png "s25")	

Jeśli na sterowniku jest Runtime 4024 i chcielibyśmy używać serwera w wersji 1.14, można zainstalować paczkę standalone (narzędzie inżynierskie HMI na naszym komputerze do tworzenie projektów musi mieć wtedy TwinCATa 4026 i HMI 1.14):
Paczkę można pobrać ze [strony:](https://www.beckhoff.com/pl-pl/support/download-finder/search-result/?download_group=168440581&download_item=840622873)

![s26](s26.png "s26")	

Niezależnie od wybranej metody, po instalacji na pasku systemowym pojawi się ikona serwera:

![s27](s27.png "s27")	

## Generowanie licencji

Aby móc skonfigurować serwer HMI, musi być aktywna licencja TF2000 (może być to licencja trial).
Należy więc otworzyć dowolny projekt TwinCAT, nawiązać połączenie ze sterownkiem na którym znajduje się HMI server (wymagany XAR TwinCAT) i wygenerować licencję.

![s3](s3.png "s3")

![s4](s4.png "s4")

Po wygenerowaniu licencji należy przeładować stan TwinCATa (np. ponownie do Run lub Config), aby status licencji był poprawny.

![s5a](s5a.png "s5a")

## Utowrzenie instancji serwera

Aby utworzyć nową instację serwera (musi istnieć przynajmniej jedna), klikamy na ikonę serwera i wybieramy opcję *Service Configuration*:

![s28](s28.png "s28")

Uzupełanimy nazwę, porty oraz hasło dla serwera, a następnie zatwierdzamy zmiany przyciskiem *Add*:

![s29](s29.png "s29")

Instancja powinna pojawić się w górnej części strony:

![s30](s30.png "s30")

## Dodanie wyjątku do zapory sieciowej

Aby serwer był widoczny dla innych urządzeń (m.in. dla komputera z narzędziem inżynierskim), należy dodać wyjątek do zapory.
Robimy to w krokach:
- otwieramy Panel Sterowania 

![s10](s10.png "s10")

- wyszukujemy ustawienia zapory i wybieramy opcję **Allow an app through Windows Firewalll**

![s11](s11.png "s11")

- klikamy przycisk **Allow another app**

![s12](s12.png "s12")

- następnie **Browse**

![s13](s13.png "s13")

- wskazujemy plik **TcHmiServer.exe** z lokalizacji **C:\Program Files (x86)\Beckhoff\TwinCAT\Functions\TF2000-HMI-Server**

![s31](s31.png "s31")

Zatwierdzamy zmiany.
<br>
Na koniec robimy restart serwera:
 
![s32](s32.png "s32")

## Publikowanie aplikacji 

Aby przesłać gotową aplikację z narzędzia inżynierskiego na docelowy serwer, klikamy PPM na nazwie projektu i wybieramy opcję *Publish to TwinCAT HMI Server*:

![s15a](s15a.png "s15a")

Nadajemy nazwę profilu, pod którą będą zapisane dane serwera:

![s16](s16.png "s16")

Następnie, wyszkujeemy dostępne w sieci serwery:

![s17a](s17a.png "s17a")

Wybieramy serwer z listy (sterownik):

![sa14](sa14.png "sa14")

Uzupełniamy hasło i sprawdzamy połączenie:

![s19a](s19a.png "s19a")

Powienien pojawić się komunikat:

![s20](s20.png "s20")

W następnym kroku wykonujemy operację *Publish*:

![s21_1](s21_1.png "s21_1")

Jeśli pojawi się ostrzeżenie jak poniżej:

![s21a](s21a.png "s21a")

to można anualować proces, zmienić platfromę na TwinCAT HMI:

![s21b2](s21b2.png "s21b2")

i ponowić publikację. 
Status publikacji można obserwować w oknie *Output*:

![s22a](s22a.png "s22a")

Po zakończonej publikacji można otworzyć wizualizację na kilka sposobów:
- z zewnętrznego urządzenia podając adres: https://adresIP_serwera:1020

![s23](s23.png "s23")

- lokalnie 
	- na urządzeniu z serwerem: http://127.0.0.1:1010
	- kilkając na ikonę serwera i opcję *Start Page*
	
![s24](s24.png "s24")	