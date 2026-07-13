---
title: Analiza statyczna 
parent: Debugowanie projektu 
nav_order: 3
layout: page
---

# Analiza statyczna i inne opcje 

## Analiza statyczna 
Przy domyślnych ustawieniach środowiska TwinCAT 3 nie są sprawdzane następujące aspekty:
- **Unused variables** – nieużywane zmienne
- **Overlapping memory areas** – nakładające się obszary pamięci
- **Concurrent access** – dostęp do zmiennej z różnych tasków
- **Multiple write acces on output** – nadpisywanie wyjścia z różnych miejsc
- **Multiple usage of name** – użycie tej samej nazwy w różnych miejscach


Jeśli chcemy aktywować wyżej wymienione opcje należy uaktywnić okno właściwości projektu. W tym celu należy kliknąć PPM na nazwie projektu i wybrać **Properties**, a następnie przejść do zakładki **Static Analysis Light** i zaznaczyć interesujące nas opcje.

![as1](https://ba-pl.github.io/wiki/assets/images/checks/as1.png "as1")

## Cross reference 
Aby odnaleźć wszystkie odwołania do danej zmiennej w programie (listę referencji) należy postawić kursor na nazwie zmiennej i kliknąć PPM a następnie wybrać opcję **Find All References**. Pojawi się okno z listą miejsc w projekcie gdzie znajdują się wszystkie odwołania. Po dwukrotnym kliknięciu LPM zostaniemy przeniesieni do odpowiedniego miejsca w projekcie.

![as2](https://ba-pl.github.io/wiki/assets/images/checks/as2.png "as2")

## Check all objects 
Jeśli w projekcie znajduje się kilka programów, bloków funkcyjnych etc. to podczas przybudowania projektu (BUILD --> Rebuild Solution) sprawdzane są tylko te składowe projektu, które są wywołane. 
<br>
Przykład:
<br>
- w projekcie znajdują się dwa programy: MAIN oraz POU. Program POU nie jest używany (nie jest wywołany) ale znajdują się w nim błędy składni. Po przebudowaniu projektu nie pojawią się komunikaty o błędach. Jeśli chcemy sprawdzać również składnię w niewywołanych elementach, należy PPM myszy kliknąć na nazwie projektu i wybrać opcję **Check All Objects.**
<br>

Pojawią się wtedy komunikaty o błędach, które występują w niewywołanych elementach projektu.
![as3](https://ba-pl.github.io/wiki/assets/images/checks/as3.png "as3")


