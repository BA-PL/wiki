---
title: Taski i priorytety
parent: Projekt TwinCAT
nav_order: 3
layout: page
---


# Domyślne ustawienia tasków i ich priorytetów
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

W celu sprawdzenia domyślnego ustawiania priorytetów do projektu dodano następujące elementy:
- Projekt NC zawierający
	- NC-Task 1 SAF
	- 4 osie
- Dwa projekty PLC
	- każdy zawierający wizualizację Target Visualization i Web Visualization
	- jeden zawierający 2 taski, jeden zawierający 3 taski

# Domyślne ustawienie priorytetów 
Domyślne przyznanie priorytetów wygląda następująco:

![prio1](https://ba-pl.github.io/wiki/assets/images/prio1.png "prio1")

Uwagi:
- Kolejne taski PLC otrzymują kolejne numery priorytetów poczynając od priorytetu nr 20, w kolejności dodawania ich do projektu (im później dodany Task tym niższy ma priorytet).
- Task VISU_TASK jest wspólny dla obu projektów PLC.

# Instrukcja dodawania tasków
Podczas dodawania tasków, należy zwracać uwagę na priotytety. Dobrą praktyką podczas dodawania tasków jest robienie tego poprzez kliknięcie PPM na nazwie projektu PLC i wybranie **Add --> Referenced Task...** W oknie dialogowym, w polu **Name**, należy wpisać nazwę nowego tasku i wybrać przycisk **Create New Task**. Dodany task pojawi się w oknie **Available Tasks**, należy go w tym oknie zaznaczyć i wybrać **Open**.

![prio2](https://ba-pl.github.io/wiki/assets/images/prio2.png "prio2")

Ustawienia priorytetów można sprawdzić, wybierając z drzewa projektu **Real-Time**, a następnie zakładkę **Priorities**. Aby dokonywać zmian ustawień tasków, należy wybrać odpowiedni task w drzewie projektu, w zakładce **Tasks**.

![prio3](https://ba-pl.github.io/wiki/assets/images/prio3.png "prio3")

# Dodatkowe informacje

Dodatkowe informacje dostępne są [tutaj.](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_system/818865035.html)
<br>
Zakładka *Priorities* w *Real-Time* w drzewku projektu (zdjęcie za: infosys.beckhoff.com):

![prio4](https://ba-pl.github.io/wiki/assets/images/prio4.png "prio4")