---
title: TcHmiAlarm
parent: Rozszerzenia 
nav_order: 2
layout: page
---


# TcHmiAlarm
{: .no_toc }
<h6> Data modyfikacji: 14.01.2026 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Obsługa zdarzeń/alarmów w TwinCAT HMI

## EventLogger
Do obsługi zdarzeń w TwinCAT można wykorzystać dwa różne mechanizmy.
<br>
Pierwszy z nich to EventLogger - rozbudowana technologia do logowania tego, co dzieje się w naszej aplikacji, wymaga definicji eventów w projekcie XAE oraz ich obsługi z poziomu kodu PLC. Efekt finalny może zostać wyświetlony za pomocą kontrolki EventGrid w TwinCAT HMI.
<br>
Kompletny opis tej technologii znajduje się [tutaj.](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_eventlogger/4278559115.html)
<br>
Warto również zapoznać się z webinariami:
- oficjalny webinar Beckhoff Niemcy: [link](https://www.beckhoff.com/pl-pl/support/webinars/twincat-3-eventlogger-event-based-communications-from-the-twincat-runtime-system.html)
- oficjalny webinar Beckhoff Polska: [link](https://www.youtube.com/watch?v=Xvqesx6IzNM)

## TcHmiAlarm

Jeśli nie potrzebujemy rozbudowanego systemu zarządzania zdarzeniami, a potrzebujemy jedynie wyświetlać alarmy jak reakcja na zdefiniowany warunek (np. wartość zmiennej przekroczyła określony limit), można wykorzystać dodatek TcHmiAlarm.
<br>
Jest to dodatek zaimplementowany bezpośrednio w TwinCAT HMI, nie wymaga programowania a jedynie konfiguracji:
- warunku wystąpienia zdarzenia
- treści komunikatu, który ma się wtedy pojawić 

 Poniższa instrukcja opisuje konfigurację TcHmiAlarm krok po kroku.
 

# Konfiguracja

## Założenia 

1. Utworzony projekt TwinCAT HMI
2. Aktywne połączenie (route) z urządzniem, na którym znajduje się działający program PLC. Z tego programu bądą odczytywane wartości zmiennych, od których następnie będą tworzone warunki wystąpienia alarmów.

Przykład powstał na podstawie TwinCAT 3.1.4026.20 oraz TwinCAT HMI 1.14.11.0.

## Instalacja pakietu NuGet 

W drzewie projektu rozwijamy katalog *References*, a następnie klikamy prawym przyciskiem myszy na pozycji *Packages* i wybieramy opcję **Manage NuGet Packages**:

![1](https://ba-pl.github.io/wiki/assets/images/alarm/1.png "1")

W otwartym oknie przechodzimy do zakładki Browse **(1)**, ustawiamy źródło na Beckhoff Offline Packages **(2)** i odszukujemy dodatek dotyczący alarmów **(3)**:

![2](https://ba-pl.github.io/wiki/assets/images/alarm/2.png "2")

Klikamy przycisk **Install** (1) i czekamy na informację o zakończonej instalacji **(2)**:

![3](https://ba-pl.github.io/wiki/assets/images/alarm/3.png "3")


## Mapowanie zmiennych

Zmienna, które mają być wykorzystane do utworzenia alarmów, muszą być najpierw zmapowane.
<br>
W tym celu otwieramy dowolny ekran wizulizacji, a następnie okno **TwinCAT HMI Configuration**. 
<br>
W zakładce *All symbols* możemy następnie zmapować wymagane zmienne:

![4](https://ba-pl.github.io/wiki/assets/images/alarm/4.png "4")

## Tworzenie treści alarmów

Treść (tekst) alarmów, które będą wyświetlane, tworzy się podobnie jak klucze językowe do tłumaczeń.
<br>
W pierwszym kroku należy zaimportować klucze pakietu TcHmiAlarm. W tym celu klimay prawym przyciskiem myszy na katalogu **Localization** i wybieramy opcję **Add new item**:

![5](https://ba-pl.github.io/wiki/assets/images/alarm/5.png "5")

W oknie, które się pojawi, wybieramy opcję **Import localization** i klimay **Add**:

![6](https://ba-pl.github.io/wiki/assets/images/alarm/6.png "6")

Zaznaczamy rozszerzenie *TcHmiAlarm* oraz startowy język i zatwierdzamy wybór:

![7](https://ba-pl.github.io/wiki/assets/images/alarm/7.png "7")

Pojawi się lista kluczy dedykowana do alarmów.

![8](https://ba-pl.github.io/wiki/assets/images/alarm/8.png "8")

Jesli chcemy dodać kolejny język, ponownie klikmamy  prawym przyciskiem myszy na katalogu **Localization** i wybieramy opcję **Add new item**, ale w wyskakującym oknie wybieramy tym razem opcję **Localization**:

![9](https://ba-pl.github.io/wiki/assets/images/alarm/9.png "9")

W kolejnym oknie, w polu *Locale*, wybieramy z listy język i ponownie zaznaczamy rozszerzenie TcHmiAlarm:

![10](https://ba-pl.github.io/wiki/assets/images/alarm/10.png "10")

Początkowo teksty będą w języku angielskim, ale możemy je przetłumaczyć czy też wyeksportować do Excel i oddać do tłumaczenia.

![11](https://ba-pl.github.io/wiki/assets/images/alarm/11.png "11")

W plikach językowych TcHmiAlarm, możemy teraz tworzyć swoje alarmy. 
<br>
Tworzymy zatem:
- swój pierwszy klucz (nazwa klucza bez spacji oraz znaków specjalnych): **A_GeneralError**,
- treść kryjącą się pod danym kluczem (docelowa treść alarmu): **General device failure**

![12](https://ba-pl.github.io/wiki/assets/images/alarm/12.png "12")

Na tej samej zasadzie możemy tworzyć kolejne alarmy. W przykładzie został dodany jeszcze jeden alarm:

![13](https://ba-pl.github.io/wiki/assets/images/alarm/13.png "13")

## Tworzenie warunku wystąpienia alarmu

Po utworzeniu tekstów, można zdefiniować kiedy dany alarm ma wystąpić.
<br>
W tym celu wracamy do okna *TwinCAT HMI Configuration* do sekcji zmapowanych zmiennych.
<br>
Klikamy prawym przyciskiem myszy na zmiennej, dla której chcemy utworzyć alarm (w przykładzie zmienna typu BOOL) i wybieramy opcję **New Alarm Settings**:

![14](https://ba-pl.github.io/wiki/assets/images/alarm/14.png "14")

W okienku klikamy przycisk **Add condition** i konfigurujemy dokładny warunek. W przykładzie alarm ma się pojawić jeśli zmienna **bError** przyjmie wartość TRUE:

![15](https://ba-pl.github.io/wiki/assets/images/alarm/15.png "15")

Tekst, który ma się wyświetlić dla tego alarmu, wybieramy w polu **Text (key)**:

![16](https://ba-pl.github.io/wiki/assets/images/alarm/16.png "16")

W przykładzie wybrano klucz **A_GeneralError (1)**. Poniżej ustawiamy poziom ważności zdarzenia **(2)** oraz np. czy ma to wymagać potwierdzenia **(3)**:

![17](https://ba-pl.github.io/wiki/assets/images/alarm/17.png "17")

W przykładzie skonfigurowano również alarm dla zmiennej rTemperture (REAL):

![18](https://ba-pl.github.io/wiki/assets/images/alarm/18.png "18")

## Wyświetlanie alarmów 

Do wyświetlania aktualnych oraz historycznych zdarzeń służy wbudowana kontrolka **Event Grid**.
Wystarczy dodać ją do jednego z ekranów wizualizacji, nie wymaga specjlanej konfiguracji, warto jednak skonfigurować odpowiednie filtrowanie w zależności od aplikacji.

![19](https://ba-pl.github.io/wiki/assets/images/alarm/19.png "19")

W przypadku przykładu zostaną odfiltrowane alarmy pochodzące z EventLoggera, zostawimy jedynie te pochądzące od dodatku TcHmiAlarm:

![20](https://ba-pl.github.io/wiki/assets/images/alarm/20.png "20")

Po uruchomieniu widoku *LiveView* oraz:
- ustawieniu w programie PLC zmiennej **bError** na **TRUE**
- lub zmiana wartości zmiennej **rTemperture** na wartość powyżej **26**
powinno spowodować pojawienie się alarmów:

![21](https://ba-pl.github.io/wiki/assets/images/alarm/21.png "21")


