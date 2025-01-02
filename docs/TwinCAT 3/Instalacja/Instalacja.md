---
title: Wersja 4026 
parent: Instrukcja instalacji 
nav_order: 1
layout: page
---


# Instalacja wersji 4026
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

TwinCAT 3.1 Build 4026 jest instalowany za pomocą TwinCAT Package Manager. Taki sposób instalacji nie ogranicza TwinCAT’a do trzech wariantów jak w poprzednich wersjach, lecz pozwala instalować, aktualizować i usuwać poszczególne komponenty niezależnie. Użytkownik może określić z jakich komponentów będzie korzystał na komputerze programisty czy sterowniku i zainstalować tylko używane przez niego komponenty.
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
Oficjalne informacje znajdują się w [Infosys.](https://infosys.beckhoff.com)

# Pobieranie i instalacja Package Manager 
Aby pobrać Package Manager przechodzimy na oficjalną [stronę Beckhoff](https://www.beckhoff.com/pl-pl/support/download-finder/) i klikamy w link na kafelku TwinCAT Package Manager (TwinCAT 3.1 Build 4026):

![26t1](https://ba-pl.github.io/wiki/assets/images/26t1.png "26t1")

Pojawi się prośba o zalogowanie się na konto myBeckhoff. Jeżeli takiego konta nie posiadamy klikamy Register, tworzymy konto i aktywujemy je. Dane logowania do konta myBeckhoff będą potrzebne również przy instalacji. Logujemy się na stronę klikając Log in:

![26t2](https://ba-pl.github.io/wiki/assets/images/26t2.png "26t2")

Po zalogowaniu klikamy w link, aby pobrać instalator:

![26t3](https://ba-pl.github.io/wiki/assets/images/26t3.png "26t3")

Uruchamiamy pobrany instalator, zapoznajemy się z warunkami licencji potwierdzamy zapoznanie się zaznaczając checkbox, klikamy Install:

![26t4](https://ba-pl.github.io/wiki/assets/images/26t4.png "26t4")

Po chwili instalator zakończy pracę. Zamykamy go przyciskiem Close

![26t5](https://ba-pl.github.io/wiki/assets/images/26t5.png "26t5")
## Konfiguracja Package Manager
Na pulpicie pojawi się ikonka Package Manager. Uruchamiamy ją jako administrator (prawy przycisk myszki -> Uruchom jako Administrator).

![26t6](https://ba-pl.github.io/wiki/assets/images/26t6.png "26t6")

W oknie Package Manager należy dodać źródło pakietów (feed). Domyślnie pojawi się źródło zawierające pakiety w wersji stablinej (stable). W pole User name i Password wprowadzamy dane logowania konta myBeckhoff i klikamy Save

![26t7](https://ba-pl.github.io/wiki/assets/images/26t7.png "26t7")

Jeśli pojawia się kod błędu 451 należy zalogować się na stronę beckhoff.pl i zaakceptować warunki na stronie
## Wybór edytora
W dalszym kroku należy wybrać edytor, który chcemy zainstalować lub zintegrować TwinCAT:

![26t8](https://ba-pl.github.io/wiki/assets/images/26t8.png "26t8")
W komputerach, na których zainstalowano Visual Studio 2019/2022 okno dodatkowo wyświetli opcje UseVS2019 i UseVS2022.
<br>
Jeśli chcemy mieć możliwość programowania sterowników z buildem 4024 lub starszym warto na tym etapie dodatkowo zaznaczyć checkbox UseTcXaeShell i/lub UseVS2019. Integrację z edytorem Visual Studio można również wykonać później, po instalacji środowiska w ustawieniach Package Manager.
<br>
Po wybraniu z którymi edytorami ma integrować się TwinCAT przechodzimy dalej klikając Next.
# Migracja wersji 4024 do Package Manager 
W przypadku gdy w systemie jest zainstalowany starsza wersja TwinCAT 3 należy wykonać migrację funkcji do nowej wersji. Jeśli w systemie nie ma starszych wersji TwinCAT’a należy przejść do kolejnego rozdziału.
<br>
Procedura migracji polega na wykryciu w systemie poprzednich wersji oraz funkcji TwinCAT, usunięcie ich z zachowaniem ustawień, licencji itp., usunięciu pozostałości z systemu, następnie instalacje w nowym formacie paczek. Pierwszym krokiem jest pobranie i instalacja najnowszej wersji skryptu dokonującego migracji:

![26t9](https://ba-pl.github.io/wiki/assets/images/26t9.png "26t9")

Po kliknięciu Yes pojawi się okno instalujące narzędzie migracji. Gdy instalacja zostanie ukończona należy zamknąć okno klikając Ok:

![26t10](https://ba-pl.github.io/wiki/assets/images/26t10.png "26t10")

Kolejnym krokiem jest symulacja migracji, czyli podsumowanie co zostanie usunięte i przeniesione:

![26t11](https://ba-pl.github.io/wiki/assets/images/26t11.png "26t11")

Klikamy OK, po czym nastąpi skanowanie systemu i wyświetlenie listy funkcji i odpowiednich paczek/workload’ów którymi zostaną zastąpione:

![26t12](https://ba-pl.github.io/wiki/assets/images/26t12.png "26t12")

**UWAGA! Na ten moment nie wszystkie funkcje z 4024 posiadają stabilną wersję w 4026. W takim przypadku instalator wyświetli ikonę X, a funkcja nie zostanie doinstalowana.**
<br>
<br>
Można dodać źródło testowe (testing feed), w którym znajduje się więcej paczek. Są to wersje testowe, które wymagają akceptacji dodatkowych warunków.

![26t13](https://ba-pl.github.io/wiki/assets/images/26t13.png "26t13")

Niektóre funkcje jak np. równoległa instalacja TwinCAT 2 i 3 nie są wspierane. W takim przypadku instalator wyświetli ikonę ostrzeżenia:

![26t14](https://ba-pl.github.io/wiki/assets/images/26t14.png "26t14")

Oznacza to że migracja usunie TwinCAT 2 z systemu i nie będzie możliwości jego instalacji równolegle z działaniem TwinCAT 3 build 4026.
<br>
Jeśli zgadzamy się na wykonanie migracji przechodzimy dalej klikając Next i uruchamiamy migrację klikając Yes.

![26t15](https://ba-pl.github.io/wiki/assets/images/26t15.png "26t15")

Po migracji, aby nadal programować sterowniki z 4024 należy przejść do punktu 4 i doinstalować TwinCAT Standard Remote Manager.
# Instalacja TwinCAT i funkcji
W głównym oknie znajduje się pole wyboru, które pozwala określić co chcemy zainstalować:

![26t16](https://ba-pl.github.io/wiki/assets/images/26t16.png "26t16")

- Engineering: narzędzie programistyczne
- Runtime: środowisko uruchomieniowe
- Engineering + Runtime: narzędzie + środowisko
<br>

Engineering + Runtime pozwala programować sterowniki i symulować program lokalnie. Najczęściej korzystamy z tej opcji.
<br>
Aby tylko programować sterowniki z naszego komputera możemy wybrać sam pakiet Engineering.
<br>
Na sterowniku wybieramy Runtime lub Engineering + Runtime, co pozwoli dodatkowo programować sterownik lokalnie, bez konieczności instalacji środowiska na innym komputerze.
<br>
Po wybraniu odpowiedniej opcji zaznaczamy workload TwinCAT Standard:

![26t17](https://ba-pl.github.io/wiki/assets/images/26t17.png "26t17")

Podobnie możemy zaznaczyć funkcje TwinCAT’a które mają zostać zainstalowane.
<br>
Widok podsumowujący po prawej stronie pokazuje listę paczek, które zostaną zainstalowane w systemie.
<br>
Jeśli potrzebujemy programować sterowniki z buildem 4024 należy zaznaczyć TwinCAT Standard Remote Manager i w wybieramy odpowiedni build.
<br>
W każdym momencie można wrócić do tego okna i dodać lub usunąć funkcje z TwinCAT’a. Aby rozpocząć instalację klikamy Install.
<br>
Instalator może poprosić o podwyższenie uprawnień. Po potwierdzeniu rozpocznie się pobieranie i instalacja środowiska:

![26t18](https://ba-pl.github.io/wiki/assets/images/26t18.png "26t18")

Instalacja może trwać około 40 minut. Podczas instalacji nie należy uruchamiać innych instalatorów, gdyż mogą zakłócić instalację środowiska.
Po instalacji okno wyświetli podsumowanie:

![26t19](https://ba-pl.github.io/wiki/assets/images/26t19.png "26t19")

Sprawdzamy czy wszystkie paczki zostały zainstalowane poprawnie i zamykamy okno. Jeśli podczas instalacji pojawią się błędy prosimy o przesłanie informacji na support@beckhoff.pl
# Modyfikowanie instalacji 
Aby dodać lub usunąć funkcje należy ponownie uruchomić Package Manager (prawy przycisk myszy -> Uruchom jako Administrator):

![26t20](https://ba-pl.github.io/wiki/assets/images/26t20.png "26t20")

Zakładka Available możemy dodać nowe funkcje zaznaczając elementy i klikając Install

![26t21](https://ba-pl.github.io/wiki/assets/images/26t21.png "26t21")

Zakładka Installed wyświetla listę zainstalowanych pakietów:

![26t22](https://ba-pl.github.io/wiki/assets/images/26t22.png "26t22")

Element z ikonką zaznaczenia w prostokącie informuje, że jest dostępna jego nowsza wersja:

![26t23](https://ba-pl.github.io/wiki/assets/images/26t23.png "26t23")

Klikając na konkretny element możemy zadecydować co z nim zrobić:
- zaktualizować

![26t24](https://ba-pl.github.io/wiki/assets/images/26t24.png "26t24")

- pozostawić bez zmian

![26t25](https://ba-pl.github.io/wiki/assets/images/26t25.png "26t25")

- odinstalować 

![26t26](https://ba-pl.github.io/wiki/assets/images/26t26.png "26t26")

<br>
Po wybraniu co chcemy zrobić po prawej stronie pojawi się podsumowanie z podziałem na kategorie Install/Upgrade/Uninstall.
<br>
Aby zastosować zmiany klikamy przycisk Install/Upgrade/Uninstall na dole podsumowania.

# Odinstalowywanie TwinCAT
Aby odinstalować TwinCAT należy najpierw odinstalować wszystkie zainstalowane pakiety, następnie odinstalować aplikację Beckhoff TwinCAT Package Manager z panelu sterowania:

![26t27](https://ba-pl.github.io/wiki/assets/images/26t27.png "26t27")

**UWAGA! Odinstalowanie aplikacji Beckhoff TwinCAT Package Manager bez odinstalowania wszystkich pakietów pozostawi pliki w systemie i może spowodować brak możliwości korzystania z TwinCAT.**

# Dodatek - konfiguracja symulacji na VM
W konfiguracji maszyny wirtualnej należy przydzielić minimum 2 rdzenie. W tym celu zamykamy maszynę i klikamy (na przykładzie VirtualBox, analogicznie w innym oprogramowaniu):

![26t28](https://ba-pl.github.io/wiki/assets/images/26t28.png "26t28")

![26t29](https://ba-pl.github.io/wiki/assets/images/26t29.png "26t29")

Po instalacji tworzymy projekt TwinCAT XAE Project:

![26t30](https://ba-pl.github.io/wiki/assets/images/26t30.png "26t30")

okno tworzenia projektu może nieco różnić się dla TcXaeShell64 i Visual Studio 2019/2022:

![26t31](https://ba-pl.github.io/wiki/assets/images/26t31.png "26t31")

W projekcie przechodzimy do SYSTEM -> RealTime:

![26t32](https://ba-pl.github.io/wiki/assets/images/26t32.png "26t32")

i wykonujemy kroki jak poniżej, ustawiając minimum 1 rdzeń izolowany:

![26t33](https://ba-pl.github.io/wiki/assets/images/26t33.png "26t33")

Po restarcie upewniamy się że w projekcie jest zaznaczony tylko rdzeń Isolated, aktywujemy konfigurację i sprawdzamy czy PLC uruchamia się.



