---
title: Obciążenie sterownika
parent: Debugowanie projektu 
nav_order: 2
layout: page
---

# Sprawdzanie obciążenia sterownika
<br>
<h6> Data modyfikacji: 30.12.2024 </h6>
<br>

Aby sprawdzić aktualne obciążenie Run-Time sterownika, należy w drzewie projektu przejść do **System --> Real-Time**, następnie wybrać tam zakładkę **Online**. Obciążenie sterownika będzie tam przedstawione w postaci wykresu rysowanego w czasie rzeczywistym.

![rt1](https://ba-pl.github.io/wiki/assets/images/checks/rt1.png "rt1")

Nibieska linia wykresu nie może przekraczać zielonej, poziomej linii limitu.

## Sprawdzanie czasu wykonywania tasku
Aby sprawdzić aktualny czas wykonywania tasku, należy w drzewie projektu przejść do **System --> Tasks --> Nasz wybrany Task**, a następnie wybrać tam zakładkę **Online**. Aktualny czas wykonywania tasku w stosunku do maksymalnego możliwego czasu przedstawiony będzie w postaci wykresu rysowanego w czasie rzeczywistym. Dostępny będzie tam też licznik **Exceed Counter**, który zlicza ilość przekroczeń czasu wykonywania tasku. W poprawnie działającym projekcie licznik ten powinien wskazywać wartość 0. Każde przekroczenie czasu wykonywania skutkuje utraceniem działania programu w czasie rzeczywistym.

![rt2](https://ba-pl.github.io/wiki/assets/images/checks/rt2.png "rt2")

### Sprawdzanie czasu wykonywania tasku z poziomu programu PLC

Czas wykonywania tasku (oraz dodatkowe informacje) można sprawdzić także z poziomu programu PLC. Służy do tego specjalny blok funkcyjny FB_TaskInfo.
<br>
Blok możesz pobrać tutaj:
<br>
<br>
[Download](https://github.com/BA-PL/Diag/archive/refs/heads/main.zip){: .btn .btn-red }
<br>

 Blok ten należy wywołać **w każdym tasku który chcemy monitorować**. Po wywołaniu bloku w zakładce online można sprawdzić informacje o tasku takie jak np.
- nazwa tasku i priorytet
- najdłuższy czas wykonania
- średni czas wykonania
- łączny czas pracy
- czas wykonania określonego cyklu, i inne...

![rt3](https://ba-pl.github.io/wiki/assets/images/checks/rt3.png "rt3")

