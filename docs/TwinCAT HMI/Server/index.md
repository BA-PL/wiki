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

