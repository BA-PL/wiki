---
title: Dobre praktyki
parent: Projekt TwinCAT
nav_order: 5
layout: page
---


# Poradnik projektu
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

**Zbiór dobrych praktyk, które warto stosować przy tworzeniu projektu w TwinCAT 3**
<br>
Niniejszy dokument zawiera zbiór przydatnych informacji oraz dobrych praktyk w tworzeniu projektu w środowisku TwinCAT 3. Nie jest to kompletna instrukcja obsługi środowiska, a jedynie wskazówki (z drobnym objaśnieniem) skierowane do zaawansowanych użytkowników, w celu utworzenia swoistej „checklisty” działań koniecznych przy nowych projektach w TC3.
<br>
Ze względu na swoją charakterystykę instrukcja ta oparta jest na uproszczeniach oraz skrótach myślowych. W żadnym wypadku nie zastępuje ona dokumentacji technicznej środowiska oraz urządzeń. Zebrane w niej wskazówki dobrane są dla ogółu użytkowników i mogą nie być idealnym rozwiązaniem dla poszczególnych, dedykowanych dla konkretnego projektu, przypadków.

# Przed rozpoczęciem projektu

## (opcjonalne) Wgraj nowy, czysty obraz na urządzenie
Jeśli sterownik, kótry posiadasz, nie jest nowym urządzeniem, pracę warto rozpocząć od aktualizacji obrazu systemu operacyjnego. 
Obraz należy dostosować do posiadanego urządzenia (zgodnie z zamówieniem / dokumentem otrzymanym od odpowiedniego oddziału Beckhoff).
Procedura wymiany obrazu:
- dla [Windows CE](https://ba-pl.github.io/wiki/docs/IPC/Windows%20CE/Przywracanie%20ustawien/)
- dla [Windows 7 / 10](https://ba-pl.github.io/wiki/docs/IPC/BigWindows/bst/) 
## Uruchamiaj wszystko jako administrator
Większość oprogramowania firmy Beckhoff w celu poprawnego działania wymaga uruchamiania jako Administrator zarówno samych programów, jak i ich instalatorów – zagwarantuje to poprawne działanie bez błędów.

![por1](https://ba-pl.github.io/wiki/assets/images/praktyki/por1.png "por1")

