---
title: Funkcje Checks 
parent: Debugowanie projektu 
nav_order: 1
layout: page
---

# Funkcje Checks 
<br>
<h6> Data modyfikacji: 30.12.2024 </h6>
<br>

Niniejsza instrukcja opisuje ogólną ideę, sposób dodania do projektu, a także sposób użycia funkcji diagnostycznych Checks. Są to funkcje służące do wykrywania i ochrony programu PLC przed błędami programistycznymi mogącymi prowadzić do tzw. Exception Mode, a więc trybu wyjątku, mogącego doprowadzić np. do samoczynnego restartu sterownika. Sytuacje, które rozpoznaje i tymczasowo naprawiają funkcje Checks to: próba dzielenia przez zero, przekroczenie zakresu tablic, a także przekroczenie zakresu liczby znakowej lub bezznakowej.

## Informacje ogólne

Funkcje Checks:
- Służą do sprawdzania np. wystąpienia dzielenia przez 0 lub przekroczenia zakresu tablicy
- Funkcje systemowe (nie trzeba ich wywoływać)
- Nie wolno zmieniać nazwy funkcji i argumentów wejściowych

![ch1](https://ba-pl.github.io/wiki/assets/images/checks/ch1.png "ch1")

## Import funkcji do programu
Pobierz wymagane pliki tutaj:
<br>
<br>
[Download Checks](https://github.com/BA-PL/Diag/archive/refs/heads/main.zip){: .btn .btn-red }

<br>
Aby zaimportować funkcje Checks do projektu PLC, proponujemy wydzielić sobie oddzielny katalog, np. o nazwie Checks:

![ch2](https://ba-pl.github.io/wiki/assets/images/checks/ch2.png "ch2")

Następnie do katalogu dodajemy pliki związane z funkcjami Checks:

![ch3](https://ba-pl.github.io/wiki/assets/images/checks/ch3.png "ch3")

## Używanie funkcji Checks 
Po dodaniu funkcji sprawdzających do projektu wgraj zmiany w projekcie (wymaga to wgrania projektu z trybie **Download**, czyli z zatrzymaniem programu).
<br>
Następnie, sprawdź wartości liczników na liście zmiennych globalnych **GVL_Checks:**

![ch4](https://ba-pl.github.io/wiki/assets/images/checks/ch4.png "ch4")

Jeśli który licznik ma wartość różną od zera, znajdź funkcję odpowiadającą temu działaniu (o takiej samej
nazwie):

![ch5](https://ba-pl.github.io/wiki/assets/images/checks/ch5.png "ch5")

Następnie kliknij na linię, w której następuje inkrementacja licznika (w tym przypadku jest to linia nr 4) i postaw w tym miejscu Breakpoint (wybierz z zakładki Debug lub wciśnij F9) i zaczekaj, aż **program zatrzyma swoje działanie.**

![ch6](https://ba-pl.github.io/wiki/assets/images/checks/ch6.png "ch6")

Znajdź fragment programu, w którym wystąpiła niepożądana akcja. W tym celu wybierz **PLC --> Windows --> Call Stack** i wskaż element w którym była wywołana funkcja sprawdzająca.

![ch7](https://ba-pl.github.io/wiki/assets/images/checks/ch7.png "ch7")

Następnie popraw algorytm i uruchom ponownie program:

![ch8](https://ba-pl.github.io/wiki/assets/images/checks/ch8.png "ch8")

Wyłącz wszystkie breakpointy, aby program PLC nie zatrzymał się w nieodpowiednim momencie:

![ch9](https://ba-pl.github.io/wiki/assets/images/checks/ch9.png "ch9")


![ch10](https://ba-pl.github.io/wiki/assets/images/checks/ch10.png "ch10")

Pamiętaj aby po naprawieniu algorytmu i ponownym uruchomieniu programu wyczyścić okno błędów i wyzerować liczniki naliczone na liście zmiennych globalnych. Pozwoli to uchronić się przed późniejszymi wątpliwościami czy błędy naliczały się, czy nie.

![ch11](https://ba-pl.github.io/wiki/assets/images/checks/ch11.png "ch11")

Po usunięciu wszystkich błędów i przed wgraniem ostatecznej wersji projektu na sterownik, usuń funkcje Checks z projektu. 