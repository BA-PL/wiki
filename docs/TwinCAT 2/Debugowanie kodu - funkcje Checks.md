---
title: Debugowanie kodu - funkcje Checks
parent: TwinCAT 2
nav_order: 2
layout: page
---


# Funkcje Checks w TwinCAT 2
{: .no_toc }
<h6> Data modyfikacji: 16.06.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Poniższa instrukcja prezentuje w jaki sposób namierzyć, za pomocą funckcji Checks, błędy programistyczne w projekcie PLC. Funckje wykrywają błędy takie jak: dzielenie przez zero, przekraczanie zakresu zmiennej, odwołanie do nieistniejącego indeksu tablicy.
# Warunki
Kiedy i jak używać funkcji Checks:
	- używać się ich powinno tylko podczas testowania aplikacji
	- funkcje systemowe (nie trzeba ich wywoływać)
	- nie wolno zmieniać nazwy funkcji i argumentów wejściowych 
	
# Instrukcja postępowania

1. Pobieramy plik .exp z funkcjami, przyciskiem poniżej: 
<br>
<br>
[Download Checks](https://github.com/BA-PL/Checks-Diag-TC2/archive/refs/heads/main.zip){: .btn .btn-red }
<br>
<br>
2. Importujemy funkcje do projektu w TwinCAT PLC Control za pomocą opcji **Project --> Import**: 

![chtc2](https://ba-pl.github.io/wiki/assets/images/tc2Checks/chtc2.png "chtc2")

Widok po zaimportowaniu:

![chtc2_2](https://ba-pl.github.io/wiki/assets/images/tc2Checks/chtc2_2.png "chtc2_2")

3. Wgrywanmy program na sterownik

4. W trybie podglądu online, otwieramy listę zmiennych globalnych **Checks** i sprawdzamy, czy któryś z liczników się inkrementuje:

![chtc2_3](https://ba-pl.github.io/wiki/assets/images/tc2Checks/chtc2_3.png "chtc2_3")

5. W zakładce POUs szukamy funkcji odpowiadającej licznikowi (ta sama nazwa):

![chtc2_4](https://ba-pl.github.io/wiki/assets/images/tc2Checks/chtc2_4.png "chtc2_4")

6. Klikamy na numer linii, w której widać inkrementowanie się licznika (na zdjęciu powyżej, linia numer 3). W ten sposób aktywujemy breakpoint - aplikacja się zatrzyma. Alternatywnie można to zrobić z górnego menu rozwijanego opcją **Online-->Toggle breakpoint**:

![chtc2_5](https://ba-pl.github.io/wiki/assets/images/tc2Checks/chtc2_5.png "chtc2_5")

7. Znajdź fragment programu, w którm wystąpił błąd. W tym celu wybieramy z górnego menu **Online-->Show call stack..**. Pojawi się okno z odwołaniami, jako druga pozycja na liście jest interesujący na element. Zanznaczamy go i klimay **Go to**:

![chtc2_6](https://ba-pl.github.io/wiki/assets/images/tc2Checks/chtc2_6.png "chtc2_6")

8. Poprawiamy algorytm:

![chtc2_7](https://ba-pl.github.io/wiki/assets/images/tc2Checks/chtc2_7.png "chtc2_7")

9. Uruchamiamy program ponownie (**Online-->Run**). Jeśli licznik nie zatrzymał się to oznacza, że gdzieś jeszcze występuje błąd. Można ponownie ustawić Breakpoint, a jeśli nie został usunięty, to program zatrzyma się ponowinie (aby usunąć breakpoint, ponownie klinij na dany numer linii kodu, pole BP powinno być szare).

![chtc2_8](https://ba-pl.github.io/wiki/assets/images/tc2Checks/chtc2_8.png "chtc2_8")

**UWAGA!! Może być tak, że jeden z liczników Checks się inkrementuje, a w miejscu ustawienie Breakpointu program się nie zatrzymuje. W takim wypadku błąd występuje w tasku, który nie jest w tym momencie debugowany. Debugowany może być jednocześnie tylko jeden task. To, który task jest aktualnie debugowany można zmienić w trybie online w zakładce Task Configuration**




