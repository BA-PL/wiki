---
title: Wersja 4026 
parent: Instrukcja instalacji 
nav_order: 1
layout: page
---


# Instalacja wersji 4026
{: .no_toc }
<h6> Data modyfikacji: 10.06.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Wprowadzennie
TwinCAT 3.1 Build 4026 jest instalowany za pomocą TwinCAT Package Manager. Taki sposób instalacji nie ogranicza TwinCAT’a do trzech wariantów jak w poprzednich wersjach, lecz pozwala instalować, aktualizować i usuwać poszczególne komponenty niezależnie. Użytkownik może określić z jakich komponentów będzie korzystał na komputerze programisty czy sterowniku i zainstalować tylko używane przez niego funkcje.
<br>
<br>
TwinCAT Package Manager jest menedżerem pakietów. Komponenty instalowane są jako pakiety w oparciu o format NuGet. Pakiety są pobierane z tzw. źródeł (feeds). Beckhoff udostępnia użytkownikom źródła Stable i Testing (wersja testowa, wymaga akceptacji dodatkowej licencji). Klienci mogą również konfigurować własne źródła w formie katalogu, lokalizacji sieciowej lub serwera pakietów NuGet. Daje to możliwość udostępniania tym kanałem również innych narzędzi na przykład bibliotek PLC, dodatków do edytora a nawet innych aplikacji.
<br>
<br>
Za pomocą interfejsu TwinCAT Package Manager użytkownik instaluje tzw. workloady, czyli zestawy pakietów które zależnie od wybranego wariantu tworzą funkcjonalność. Przykładowo na komputerze, gdzie instalujemy środowisko programistyczne (Engineering) nie zawsze potrzebujemy pakietów uruchomieniowych (Runtime), te raczej są potrzebne na sterowniku, gdzie znowu środowisko programistyczne często jest zbędne. W zależności od wariantu Package Manager zainstaluje odpowiednie pakiety, oszczędzając zasoby. W skrócie workloady odpowiadają funkcjom instalowanym w poprzednich wersjach TwinCAT’a, dając większą elastyczność.
<br>
<br>
TwinCAT Package Manager składa się z dwóch programów które ze sobą współpracują:

- TwinCAT Package Manager: graficzny menedżer workloadów ułatwiający instalację użytkownikom.
- TcPkg: interfejs wiersza poleceń który jest obsługiwany przez interfejs graficzny do zarządza pakietami.

<br>
Szereg korzyści wynikających z TwinCAT Package Manager wymaga wykonania dodatkowych kroków instalacyjnych na systemach, na których zainstalowano starsze wersje TwinCAT’a. Firma Beckhoff przygotowała narzędzie migracyjne które umożliwia wykonanie przeniesienie obecnej konfiguracji do nowej wersji TwinCAT’a.
<br>
<br>
Niniejsza instrukcja opisuje kroki wykonywane w interfejsie graficznym menedżera pakietów. Oficjalne informacje znajdują się w [Infosys.](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_installation/15698617995.html&id=7523796010185393366)

# Pobieranie i instalacja Package Manager 
Aby pobrać Package Manager przechodzimy na oficjalną [stronę Beckhoff](https://www.beckhoff.com/pl-pl/support/download-finder/) i klikamy w link na kafelku TwinCAT Package Manager (TwinCAT 3.1 Build 4026):

![26t1](https://ba-pl.github.io/wiki/assets/images/4026/26t1.png "26t1")

Przy próbie pobrania pliku **exe** pojawi się prośba o zalogowanie się na konto myBeckhoff. Jeżeli takiego konta nie posiadamy klikamy Register, tworzymy konto i aktywujemy je. Dane logowania do konta myBeckhoff będą potrzebne również przy instalacji. Logujemy się na stronę klikając Log in:

![26t2](https://ba-pl.github.io/wiki/assets/images/4026/26t2x.png "26t2")

Po zalogowaniu klikamy w link, aby pobrać instalator:

![26t3](https://ba-pl.github.io/wiki/assets/images/4026/26t3x.png "26t3")

Uruchamiamy pobrany instalator, zapoznajemy się z warunkami licencji i potwierdzamy zapoznanie się zaznaczając checkbox, następie klikamy **Install**:

![26t4](https://ba-pl.github.io/wiki/assets/images/4026/26t4x.png "26t4")

Po chwili instalator zakończy pracę. Zamykamy go przyciskiem **Close**.

![26t5](https://ba-pl.github.io/wiki/assets/images/4026/26t5x.png "26t5")
## Pierwsze uruchomienie Package Manager
Na pulpicie pojawi się ikonka Package Manager. Uruchamiamy ją jako administrator (prawy przycisk myszki -> Uruchom jako Administrator).

![26t6](https://ba-pl.github.io/wiki/assets/images/4026/26t6.png "26t6")

Po uruchomieniu pojawi się okno z informacją, aby nie instalować plików .exe z Remote Manager ze strony:

![26t6_2](https://ba-pl.github.io/wiki/assets/images/4026/26t6_2.png "26t6_2")

Zatwierdzamy, że zapoznaliśmy się z informacją klikając **Next**. Po zatwierdzeniu należy dodać źródło pakietów (feed), klikając **Add new feed**:

![26t7_1](https://ba-pl.github.io/wiki/assets/images/4026/26t7_1.png "26t7_1")

W polu *feed url* wybieramy z listy źródło zawierające pakiety w wersji stablinej (stable). W pole *User name* i *Password* wprowadzamy dane którymi logowaliśmy się do konta myBeckhoff i klikamy **Save**:

![26t7_2](https://ba-pl.github.io/wiki/assets/images/4026/26t7_2.png "26t7_2")

Przy zapisywaniu może pojawić się monit o przyznanie uprawnień administratora. Zatwierdzamy go.
### Wybór edytora
W dalszym kroku należy wybrać które z edytorów chcemy zainstalować lub zintegrować z TwinCAT:

![26t8_2](https://ba-pl.github.io/wiki/assets/images/4026/26t8_2.png "26t8_2")

W komputerach, na których zainstalowano Visual Studio 2017/2019/2022 okno dodatkowo wyświetli opcje ich integracji.
<br>
<br>
**UWAGA! Na tym etapie potrzebujemy zaznaczyć minimum jeden edytor! Jeśli chcemy mieć możliwość programowania sterowników z buildem 4024 lub starszym należy zaznaczyć instalację TwinCAT XAE Shell lub integracji z Visual Studio 2017/2019. Dla obsługi buildu 4026 i nowszych może być to dowolny edytor, ale zalecamy TwinCAT XAE Shell 64-bit lub integrację z Visual Studio 2022, szczególnie dla dużych projektów.**
<br>
<br>
Po wybraniu integracji klikamy ikonę zapisu (dyskietka) i przechodzimy dalej klikając **Next**. Zainstalowane edytory i integracje z VS można będzie zmienić później w ustawieniach Package Manager.
<br>
Na tym etapie następują drobne różnice w przypadku instalacji 4026 na systemie bez TwinCAT’a i systemie na którym znajdują się starsze wersje TwinCAT’a. W pierwszym przypadku przechodzimy do rozdziału [Ścieżka instalacji](https://ba-pl.github.io/wiki/docs/TwinCAT%203/Instalacja/Instalacja/#%C5%9Bcie%C5%BCka-instalacji). Gdy mamy zainstalowaną starszą wersję TwinCAT, przechodzimy do rozdziału dotyczącego [migracji](https://ba-pl.github.io/wiki/docs/TwinCAT%203/Instalacja/Instalacja/#migracja-starszych-build%C3%B3w-do-package-manager). 

### Ścieżka instalacji 
Ostatnim, opcjonalnym krokiem jest wskazanie ścieżki instalacji. Po zatwierdzeniu zmiana nie będzie możliwa:

![26t8_3](https://ba-pl.github.io/wiki/assets/images/4026/26t8_3.png "26t8_3")

Aby zatwierdzić, klikamy **Finish**.
<br>
<br>
**UWAGA! Nie zalecamy zmiany domyślnej ścieżki instalacji TwinCAT, gdyż może to powodować problemy!**

# Migracja starszych buildów do Package Manager 
W przypadku gdy w systemie jest zainstalowany starsza wersja TwinCAT 3 należy wykonać migrację funkcji do 4026. Jeśli w systemie nie ma starszych wersji TwinCAT’a należy pominąć ten punkt. Gdy chcemy doinstalować Remote Manager do starszych wersji, przechodzimy do rodziału dotyczącego [Remote Managera](https://ba-pl.github.io/wiki/docs/TwinCAT%203/Instalacja/Instalacja/#instalacja-starszych-build%C3%B3w-remote-manage-ilub-twincat-2).
<br>
Procedura migracji polega na wykryciu w systemie poprzednich wersji oraz funkcji TwinCAT, usunięcie ich z zachowaniem ustawień, licencji itp., usunięciu pozostałości z systemu, następnie instalacje w nowym formacie paczek. Klikamy **Yes**. Jeśli system poprosi nas o uprawnienia administratora, przyznajemy je.

![26t9_2](https://ba-pl.github.io/wiki/assets/images/4026/26t9_2.png "26t9_2")

Pierwszym krokiem jest pobranie i instalacja najnowszej wersji skryptu dokonującego migracji:

![26t10_2](https://ba-pl.github.io/wiki/assets/images/4026/26t10_2.png "26t10_2")

Po kliknięciu **Yes** może pojawić się monit o uprawnienia administratora. Zatwierdzamy go. Po chwili rozpocznie się symulacja migracji. Po ukończeniu wyświetli podsumowanie co zostanie usunięte i przeniesione:

![26t11_2](https://ba-pl.github.io/wiki/assets/images/4026/26t11_2.png "26t11_2")

**UWAGA! Nie wszystkie funkcje posiadają wersję w 4026. W przypadku gdy nie pojawi się nazwa w kolumnie workloads, funkcja nie zostanie doinstalowana.**
<br>
<br>
Niektóre funkcje jak np. równoległa instalacja TwinCAT 2 i 3 oraz instalacja starszych Remote Manager wymagają dodatkowych kroków po wykonaniu migracji. Jeśli zgadzamy się na wykonanie migracji przechodzimy dalej klikając **Next**. Po ukończeniu usuwania starszych wersji TwinCAT pojawi się komunikat:

![26t14_2](https://ba-pl.github.io/wiki/assets/images/4026/26t14_2.png "26t14_2")

Zatwierdzamy przyciskiem **OK**, po czym system uruchomi się ponownie. Po restarcie migracja będzie kontynuowana. Pojawi się monit o uprawnienia administratora, zatwierdzamy go.
<br>
Po ukończeniu procedury wyświetli się podsumowanie migracji:

![26t15_2](https://ba-pl.github.io/wiki/assets/images/4026/26t15_2.png "26t15_2")

Zatwierdzamy je klikając **Finish**. Po migracji można już korzystać z TwinCAT 4026. Dodatkowo, aby programować sterowniki z starszymi wersjami TwinCAT’a należy przejść do rozdziału dotyczącego [Remote Managera](https://ba-pl.github.io/wiki/docs/TwinCAT%203/Instalacja/Instalacja/#instalacja-starszych-build%C3%B3w-remote-manage-ilub-twincat-2).
# Instalacja TwinCAT i funkcji
W głównym oknie znajduje się pole wyboru, które pozwala określić co chcemy zainstalować:

![26t16_2](https://ba-pl.github.io/wiki/assets/images/4026/26t16_2.png "26t16_2")

- Engineering: narzędzie programistyczne
- Runtime: środowisko uruchomieniowe
- Engineering + Runtime: narzędzie + środowisko
<br>

Engineering + Runtime pozwala programować sterowniki i symulować program lokalnie. Najczęściej korzystamy z tej opcji.
<br>
<br>
Aby tylko programować sterowniki z naszego komputera możemy wybrać sam pakiet Engineering.
<br>
<br>
Na sterowniku zawsze wybieramy Runtime, czasem Engineering + Runtime, co pozwoli dodatkowo programować sterownik lokalnie, bez konieczności instalacji środowiska na innym komputerze.
<br>
<br>
Po wybraniu odpowiedniej opcji zaznaczamy workloady Engineering i Runtime w TwinCAT Standard, klikając na ikonę pobierania (na ikonie widać również wersję):

![26t17_2](https://ba-pl.github.io/wiki/assets/images/4026/26t17_2.png "26t17_2")

Podobnie możemy zaznaczyć funkcje TwinCAT’a które mają zostać zainstalowane.
<br>
Ikona w prawym górnym rogu zasygnalizuje nam ilość zmian. Można ją kliknąć:

![26t17_3](https://ba-pl.github.io/wiki/assets/images/4026/26t17_3.png "26t17_3")

Po prawej stronie pojawi się widok podsumowujący z listą produktów, które zostaną zainstalowane, odinstalowane lub zaktualizowane w systemie:

![26t17_4](https://ba-pl.github.io/wiki/assets/images/4026/26t17_4.png "26t17_4")

Aby rozpocząć instalację klikamy **Apply modifications** na dole podsumowania:

![26t17_5](https://ba-pl.github.io/wiki/assets/images/4026/26t17_5.png "26t17_5")

Potwierdzamy instalację klikając **YES**:

![26t17_6](https://ba-pl.github.io/wiki/assets/images/4026/26t17_6.png "26t17_6")

Instalator może poprosić o podwyższenie uprawnień. Po potwierdzeniu rozpocznie się pobieranie i instalacja środowiska:

![26t17_7](https://ba-pl.github.io/wiki/assets/images/4026/26t17_7.png "26t17_7")

Instalacja może trwać dłuższą chwilę. Podczas instalacji nie należy uruchamiać innych instalatorów, gdyż mogą zakłócić instalację środowiska.
<br>
Po instalacji okno wyświetli się komunikat:

![26t17_8](https://ba-pl.github.io/wiki/assets/images/4026/26t17_8.png "26t17_8")

oraz podsumowanie instalacji:

![26t17_9](https://ba-pl.github.io/wiki/assets/images/4026/26t17_9.png "26t17_9")

Sprawdzamy czy wszystkie paczki zostały zainstalowane poprawnie i zamykamy okno przyciskiem **CLOSE**. Jeśli podczas instalacji pojawią się błędy prosimy o przesłanie informacji na *support@beckhoff.pl*
# Modyfikowanie instalacji 
Aby dodać lub usunąć funkcje należy ponownie uruchomić Package Manager (prawy przycisk myszy -> Uruchom jako Administrator):

![26t20](https://ba-pl.github.io/wiki/assets/images/4026/26t20.png "26t20")

W zakładce *Browse*  możemy dodać nowe funkcje zaznaczając elementy i klikając **Apply modifications**:

![26t21_2](https://ba-pl.github.io/wiki/assets/images/4026/26t21_2.png "26t21_2")

Zakładka *Installed* wyświetla listę zainstalowanych funkcji. Możemy je usuwać klikając na danym elemencie ikonę kosza:

![26t21_3](https://ba-pl.github.io/wiki/assets/images/4026/26t21_3.png "26t21_3")

po czym standardowo w zakładce podsumowania klikamy **Apply modifications.**
<br>
<br>
Zakładka *Updates* odblokuje się w gdy pojawią się nowe wersje funkcji. Można w niej zaktualizować funkcje TwinCAT, klikając analogicznie jak w przypadku instalacji funkcji.

# Instalacja starszych buildów (Remote Manage) i/lub TwinCAT 2

Po instalacji/migracji można doinstalować starsze wersje TwinCAT. Aby to wykonać należy dodać kolejny feed (źródło) w ustawieniach TwinCAT Package Manager.
<br>
<br>
Klikamy ikonę ustawień w lewym dolnym rogu:

![26t34_1](https://ba-pl.github.io/wiki/assets/images/4026/26t34_1.png "26t34_1")

W *Feeds* dodajemy *Beckhoff Outdated Feed*:

![26t34_2](https://ba-pl.github.io/wiki/assets/images/4026/26t34_2.png "26t34_2")

![26t34_3](https://ba-pl.github.io/wiki/assets/images/4026/26t34_3.png "26t34_3")

Wprowadzamy poświadczenia, zapisujemy zmiany klikając **Save**. Akceptujemy disclaimer klikając **YES**. Jeśli pojawi się monit o uprawnienia administratora, akceptujemy go:

![26t34_4](https://ba-pl.github.io/wiki/assets/images/4026/26t34_4.png "26t34_4")

W efekcie na liście powinny być dostępne 2 feedy:

![26t34_5](https://ba-pl.github.io/wiki/assets/images/4026/26t34_5.png "26t34_5")

Zamykamy okno konfiguracji, czekamy aż odświeży się lista pakietów. Domyślnie dalej wyświetlane są pakiety z Beckhoff Stable Feed. Aby włączyć wyświetlanie wszystkich pakietów klikamy po lewej stronie:

![26t34_6](https://ba-pl.github.io/wiki/assets/images/4026/26t34_6.png "26t34_6")

## Instalacja Remote Manager dla TwinCAT 3
W zakładce *Browse* klikamy na nazwę **Engineering - TwinCAT Standard Remote Manager**. Po prawej stronie pojawi się opis funkcji. Klikamy **Versions**. Wyświetli się lista wszystkich dostępnych wersji, gdzie dodajemy do instalacji starsze wersje TwinCAT:

![26t34_7](https://ba-pl.github.io/wiki/assets/images/4026/26t34_7.png "26t34_7")

Wybieramy żądaną wersję, a następnie standardowo zatwierdzamy modyfikacje.

## Instalacja TwinCAT 2
W celu instalacji TwinCAT 2 musimy przełączyć z widoku workloadów (funkcji) na widok pojedynczych pakietów:

![26t34_8](https://ba-pl.github.io/wiki/assets/images/4026/26t34_8.png "26t34_8")

Na liście znajdziemy dwa pakiety:

![26t34_9](https://ba-pl.github.io/wiki/assets/images/4026/26t34_9.png "26t34_9")

•	Pakiet TwinCAT.XAE.Tc2Engineering zainstaluje narzędzie inżynierskie TwinCAT 2.

•	Pakiet TwinCAT.XAE.PLC.Tc2ProjectConverter doda do 4024 i starszych buildów możliwość konwertowania projektów z TC2.
<br>
Instalujemy potrzebne pakiety. Po instalacji warto przywrócić standardowy widok workload’ów klikając:

![26t34_10](https://ba-pl.github.io/wiki/assets/images/4026/26t34_10.png "26t34_10")
# Odinstalowywanie TwinCAT
Aby odinstalować TwinCAT należy najpierw odinstalować wszystkie zainstalowane pakiety, następnie odinstalować aplikację Beckhoff TwinCAT Package Manager z panelu sterowania. Najprościej wykonać to uruchamiając CMD z uprawnieniami Administratora i wpisując polecenie:

![26t27_2](https://ba-pl.github.io/wiki/assets/images/4026/26t27_2.png "26t27_2")

Po czym potwierdzamy operację, wpisując Y:

![26t27_3](https://ba-pl.github.io/wiki/assets/images/4026/26t27_3.png "26t27_3")

Po ukończeniu operacji można usunąć Beckhoff TwinCAT Package Manager z komputera:

![26t27_4](https://ba-pl.github.io/wiki/assets/images/4026/26t27_4.png "26t27_4")

**UWAGA! Odinstalowanie aplikacji Beckhoff TwinCAT Package Manager bez odinstalowania wszystkich pakietów pozostawi pliki w systemie i może spowodować brak możliwości instalacji TwinCAT!**

# Dodatek - konfiguracja symulacji na VM
W konfiguracji maszyny wirtualnej należy przydzielić minimum 2 rdzenie. W tym celu zamykamy maszynę i klikamy (na przykładzie VirtualBox, analogicznie w innym oprogramowaniu):

![26t28](https://ba-pl.github.io/wiki/assets/images/4026/26t28x.png "26t28")

![26t29](https://ba-pl.github.io/wiki/assets/images/4026/26t29x.png "26t29")

Po instalacji tworzymy projekt TwinCAT XAE Project:

![26t30](https://ba-pl.github.io/wiki/assets/images/4026/26t30x.png "26t30")

okno tworzenia projektu może nieco różnić się dla TcXaeShell64 i Visual Studio 2019/2022:

![26t31](https://ba-pl.github.io/wiki/assets/images/4026/26t31x.png "26t31")

W projekcie przechodzimy do SYSTEM -> RealTime:

![26t32](https://ba-pl.github.io/wiki/assets/images/4026/26t32x.png "26t32")

i wykonujemy kroki jak poniżej, ustawiając minimum 1 rdzeń izolowany:

![26t33](https://ba-pl.github.io/wiki/assets/images/4026/26t33x.png "26t33")

Po restarcie upewniamy się że w projekcie jest zaznaczony tylko rdzeń Isolated, aktywujemy konfigurację i sprawdzamy czy PLC uruchamia się.



