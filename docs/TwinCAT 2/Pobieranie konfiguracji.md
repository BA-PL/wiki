---
title: Pobieranie konfiguracji (System Manager)
parent: TwinCAT 2
nav_order: 1
layout: page
---


# Pobieranie konfiguracji sprzętowej sterownika za pomocą TwinCAT System Manager 
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Poniższa instrukcja prezentuje w jaki sposób pobrać konfigurację sprzętową ze sterownika wyposażonego w TwinCAT 2.
Instrukcja w żaden sposób nie zastępuje szkolenia i nie może być tak traktowana, ponieważ może zawierać informacje i skróty myślowe niejasne dla osoby, która tego szkolenia nie przeszła.

# Pobranie i instalacja TwinCAT 2
Link do pobrania oporgramowania:
- [wersja 32bit](https://www.beckhoff.com/pl-pl/support/download-finder/search-result/?download_group=97248943&download_item=645885953)
- [wersja 64bit](https://www.beckhoff.com/pl-pl/support/download-finder/search-result/?download_group=97248943&download_item=645997772)

Oprogramowanie instalujemy jako *Administrator*. 
<br>
<br>
**UWAGA!** Jeśli posiadasz już na swoim komputerze  środowisko TwinCAT 3 (wersja<=3.1.4024.x), postępuj zgodnie z instrukcją znajdującą się [tutaj.](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_installation/179471755.html)

# Połączenie ze sterownikiem
W celu połączenia ze sterownikiem wyposażonym w TwinCAT 2 należy uruchomić środowisko **TwinCAT System Manager:**

![tsm1](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm1.png "tsm1")

Przy pierwszym użyciu środowisko może wczytywać się nieco dłużej. Na dole powinien pojawić się pasek postępu wczytywania opisów urządzeń *Rebuilding EtherCAT Devices description cache*. 
<br>
Jeśli nie jest to pierwse użycie narzędzia, przed wykonaniem kolejnych kroków warto jest otworzyć nową, pustą konfigurację:

![tsm0](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm0.png "tsm0")

Następnie w zakładce **System** wybrać opcję **Choose Target**:

![tsm2](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm2.png "tsm2")

Następnie w oknie, które się pojawi, wybieramy **Search (Ethernet)** i wyszukujemy sterownik. 

![tsm3](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm3.png "tsm3")

W kolejnym oknie mamy dwie możliwości:
- Wybór **Broadcast Search** i przeszukanie sieci w poszukiwaniu sterowników (1)
- Bezpośrednie wpisanie adresu IP sterownika, jeśli go znamy (2)

![tsm4](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm4.png "tsm4")

Po wyszukaniu sterownika można przejść do dodania do niego połączenia (Route), wybierając sterownik z listy i klikając przycisk **Add Route**:

![tsm5](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm5.png "tsm5")

W oknie, które się pojawi wpisujemy hasło (domyślnie jest to „1”), jednak może ono zostać zmienione przez użytkownika) i wybieramy OK. W efekcie w polu *Connected* powinien się pojawić się znak „X” (jak na zrzucie ekranu powyżej). Połączenie zostało dodane, można więc wyjść z okna klikając **Close**.
<br>
<br>
Następnie w oknie do którego wracamy, należy wybrać sterownik, do którego dodano połączenie  - poprzez dwukrotne kliknięcie na jego pozycji na liście – i następnie można już przystąpić do procedury pobrania konfiguracji sprzętowej.

![tsm6](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm6.png "tsm6")

# Pobranie konfiguracji sprzętowej 
W celu pobrania konfiguracji powinny zostać najpierw wykonane kroki z poprzedniego rozdziału - połączenie ze sterownikiem powinno być nawiązane i sterownik powinien zostać również wybrany jako Target System.
<br>
W prawym dolnym rogu powinna być widoczna nazwa sterownika oraz obok nazwy, status TwinCATa. 
<br>
Status TwinCATa musi sygnalizować albo stan pracu RUN (zielony kolor) lub stan konfiguracyjny CONFIG (niebieski kolor). 
<br>
Nie może tam znajdować się kolor czerowny (STOP), szary (INACTIVE) lub żółty (TIMEOUT). 

![tsmx1](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsmx1.png "tsmx1")

Następnie w celu pobrania konfiguracji sprzętowej należy wybrać przycisk File --> Open From Target:

![tsm7](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm7.png "tsm7")

Po wybraniu tego przycisku powinna załadować się konfiguracja sprzętowa:

![tsm8](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm8.png "tsm8")

**Jeżeli po pobraniu pojawi się pytanie o update Ams Net ID, należy wybrać opcję „Nie”, aby nie utraciły się referencje do fizycznych urządzeń w projekcie.**

Dobrym zwyczajem jest natychmiastowe zapisanie pobranej konfiguracji, z górnego menu wybierając File --> Save As… (zapisujemy pod taką nazwą, jaka została zaczytana przy pobieraniu, widoczna w lewym górnym rogu). 

![tsm9](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm9.png "tsm9")

# Możliwe problemy 

## Problemy z połączeniem 
Najczęściej występującym w czasie pobierania konfiguracji sprzętowej błędem jest błąd występujący w jednym z pierwszych kroków instrukcji (połączenie ze sterownikiem), prezentujący się następująco:

![tsm10](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm10.png "tsm10")

Oznacza on najprawdopodobniej, że użyte zostały złe poświadczenia. Należy użyć poprawnych poświadczeń (użytkownika i hasła).

## Brak lub uszkodzony plik konfiguracji

Jeśli po wykonaniu komendy *Open from Target* otrzymujemy pustą konfigurację (nic nie pojawia się w drzewku):

![tsm11](https://ba-pl.github.io/wiki/assets/images/SystemManager/tsm11.png "tsm11")

oznacza to, że sterownik nie posiada aplikacji lub jest problem z plikiem **CurrentConfig.tsm** znajdującym się na sterowniku. W takim przypadku pobranie konfiguracji nie będzie raczej możliwe.
<br>
Zalecamy kontakt z działem support - **support@beckhoff.pl**