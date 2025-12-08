---
title: Event Viewer
parent: Windows 7/10/11 
nav_order: 2
layout: page
---

# Event Viewer
{: .no_toc }
<h6> Data modyfikacji: 06.10.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }


1. TOC
{:toc}

**Event Viewer** w TwinCAT to narzędzie do podglądu zdarzeń systemowych, w tym zdarzeń wygenerowanych przez komponenty Beckhoffa.  
Pokazuje komunikaty z PLC, I/O, NC i samego systemu Windows.  
Można tam sprawdzić informacje, ostrzeżenia i błędy – wszystko z dokładnym czasem. Przydaje się do diagnozy problemów, np. gdy coś nie startuje albo sterownik wyrzuca błąd.

<br>
Dzięki filtrom łatwo znaleźć konkretne zdarzenie i zobaczyć, co działo się w danym momencie.  
Jest to jedno z kluczwych narzędzi diagnostyczych, natywnie występujący w systemach Big Windows

# Połączenie zdalne 

Sterowniki Beckhoffa z pełnymi wersjami systemowymi posiadają możlwiość podłączenia za pomocą pulpitu zdalnego.  
W Celu utworzenia połaczenia, wymagane jest:
- Adres IP Sterownika
- Login
- Hasło

W pierwszej klejności uruchamiamy pulpit zdalny

![RemoteDesktopIcon](https://ba-pl.github.io/wiki/assets/images/EV/RemoteDesktop_Icon.png "RemoteDesktopIcon")

Wprowadzamy adres IP sterownika

![RemoteDesktop_Credentials](https://ba-pl.github.io/wiki/assets/images/EV/RemoteDesktop_Credentials.png "RemoteDesktop_Credentials")

Następnie:
- wybieramy pozycję **więcej opcji**
- zmieniamy opcję na **użyj innego konta**
- wprowadzamy login i hasło do sterownika
  - domyślnie *Administrator* oraz *1*
  
![RemoteDesktop_Access](https://ba-pl.github.io/wiki/assets/images/EV/RemoteDesktop_Access.png "RemoteDesktop_Access")

Po wprowadzemiu poprawnych danych od połączenia akceptujemy certyfikat szyfrujący połączenie.

![RemoteDesktop_Cert](https://ba-pl.github.io/wiki/assets/images/EV/RemoteDesktop_Cert.png "RemoteDesktop_Cert")

I po przejściu powyższego procesu jesteśmy połączeni do sterownika 

# Event Viewer
## Uruchomienie Event Viewer'a

Po uzyskaniu dostępu do pulpitu sterownika, Event Viewer'a możemy znaleźć przeglądając aplikacje pod ikoną SysTray TwinCAT'a

![EventViewer_Shortut](https://ba-pl.github.io/wiki/assets/images/EV/EventViewer_Shortcut.png "EventViewer_Shortut")

Logi związane z systemami Beckhoff'a są umieszczane w folderze *Windows Logs > Application* 

![EventViewer_Tree](https://ba-pl.github.io/wiki/assets/images/EV/EventViewer_Tree.png "EventViewer_Tree")

## Przykładowa diagnostyka

Przeglądając listę zdarzeń często mozemy się natknąć na eventy z przedrostkiem Tc*
<br>
Zawierają one informacje na temat przejść, maszyn stanów, uruchomień różnych usług i w zależności od ważności danego komuniaktu możemy oczytać czy dana usługa zachowuje się zgodnie z oczekiwaniami.


![EventViewer_DiagScreen](https://ba-pl.github.io/wiki/assets/images/EV/EventViewer_DiagScreen.png "EventViewer_DiagScreen")

Na podstawie wiadomości, zawartej w danym zdarzeniu możemy często we własnym zakresie zdiagnozować samą usługę, lub przekazać później w rozmowie informację do wsparcia technicznego w celu ułatwienia zdalnej diagnsotki maszyny.

## Exportowanie zdarzeń

Zdarza się że dział wsparcia technicznego zarząda informacji na temat historii zdarzeń danej maszyny w celu dalszej diagnsotyki.  
Aby udostępnić listę zdarzeń należy:
- na pulpicie sterownika (Zdalnie, panel dotykowy lub monitor+klawiatura+myszka)
- uruchomić aplikację Event Viewer
- przejść do sekcji *Windows Logs > Application* 
- z prawej strony wybieramy opcje *Save All Events As...*
- wygenerowany plik zapisujemy pod dowolną nazwą

![EventViewer_SaveAllEvents](https://ba-pl.github.io/wiki/assets/images/EV/EventViewer_SaveAllEvents.png "EventViewer_SaveAllEvents")

Operację wykonać najlepiej dla zakładek Application, System oraz Security.

![EV_tabs](https://ba-pl.github.io/wiki/assets/images/EV/EV_tabs.png "EV_tabs") 

Następnie te pliki wysyłamy jako załącznik na **support@beckhoff.pl**.
 


