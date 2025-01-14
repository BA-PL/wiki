---
title: ESTOP - Quick Start
nav_order: 1
layout: page
parent: TwinSAFE
---

TwinSAFE EStop
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Wstęp 
Niniejsza instrukcja przedstawia metodologię tworzenia funkcji bezpieczeństwa zatrzymania awaryjnego przy użyciu modułu bezpiecznej logiki EL6910 oraz modułów bezpiecznych wejść EL1904 i bezpiecznych wyjść EL2904.
<br>
Instrukcja skupia się przede wszystkim na konfiguracji projektu TwinSAFE, zastosowaniu bloku SafeEstop oraz sposobie powiązania logiki z wejściami i wyjściami (zarówno bezpiecznymi jak i PLC).
<br>
Blok safeEstop odpowiedzialny jest za realizację funkcji zatrzymania awaryjnego na podstawie bezpiecznych sygnałów wejściowych. 

![safety1](safety1.png "Safety1")

Wejścia bloku to:
1.	Wejście restart odpowiedzialne za przywrócenie maszyny do stanu pracy.
2.	Wejścia sygnałów decydujących o bezpiecznym zatrzymaniu. W przypadku niniejszej instrukcji połączone one zostaną z przyciskiem zatrzymania awaryjnego NC+NO. Wejścia bloku funkcyjnego mogą zostać skonfigurowane do pracy z sygnałami NO i NC zarówno jedno lub dwukanałowej. W przypadku pracy dwukanałowej możliwe jest ustawienie czasu maksymalnej zwłoki synchronizacji sygnałów, co pozwala na sprawdzanie poprawności działania przycisku.
3.	Wejście EDM odpowiedzialne za monitorowanie pracy urządzenia sterowanego sygnałem EStopOut/EStopDelOut (na przykład przekaźnika) w momencie przejścia w tryb pracy/ wyjścia ze stanu bezpiecznego. Dokładny opis działania wejścia znaleźć można w dokumentacji „TwinSAFE logic FB”
4.	Parametr Delay Time określający czas zwłoki przed zmianą stanu wyjścia EStopDelOut (przydatne przy zatrzymaniu w kategorii ). Przykładem użycia może być przekazanie sygnału EStopOut PLC w celu zatrzymania procesu, zaś sygnał EStopDelOut odpowiedzialny będzie za jego awaryjne zatrzymanie po określonym, zgodnym w normami prawnymi, czasie krytycznym.
5.	Wyjścia bezpieczne.
6.	Wyjście sygnalizacji błędu załączane w przypadku przejścia bloku funkcyjnego do stanu ERROR – powodem przejścia do tego stanu może być między innymi przekroczenie czasu synchronicznego zadziałania wejść lub niezgodny ze stanem wyjścia bezpiecznego sygnał EDM w momencie przejścia bloku w tryb pracy.

# Przygotowanie sprzętowe – przykładowe stanowisko
Przed uruchomieniem sterownika sprawdzić należy ustawienie przełączników DipSwitch znajdujących się na lewej stronie każdego z modułów bezpieczeństwa. Przełączniki te odpowiedzialne są za bezpieczną adresację poszczególnych modułów, a informacje te niezbędne będą do prawidłowej konfiguracji projektu safety.
   
![safety2](safety2.png "Safety2")

W przypadku występowania w aplikacji więcej niż jednego modułu tego samego modelu przełączniki te pozwalają na jednoznaczną identyfikację konfigurowanego urządzenia. Należy również upewnić się, że każdy moduł występujący w aplikacji ma unikalny adres. Adres 0 (ustawienie fabryczne) nie jest prawidłowym bezpiecznym adresem.

# Przygotowanie projektu safety pod aplikację
Pierwszym krokiem jest utworzenie nowego Solution z projektem TwinCAT XAE.

![safety3](safety3.png "Safety3")

Po utworzeniu projektu i połączeniu ze sterownikiem zeskanować należy moduły rozszerzeń. Krok ten wykonać należy przed przystąpieniem do tworzenia projektu safety. 

![safety4](safety4.png "Safety4")

Następnie utworzyć należy nowy Task odpowiedzialny za obsługę  urządzeń safety. W konfiguracji mastera EtherCAT należy utworzyć i przyporządkować moduły bezpieczeństwa do Sync Unitów powiązanych z bezpieczną logiką (w przykładzie Safe_Logic) oraz obsługą bezpiecznych wejść/wyjść (w przykładzie Safe_IO). Utworzone Sync Unity przyporządkować należy do utworzonego wcześniej Tasku odpowiedzialnego za safety (w przykładzie SafetyTask).
<br>
Czynność tą należy wykonać w stosunku do każdego z wykorzystywanych w aplikacji modułów safety (w przykładzie EL6910 odpowiedzialny za logikę oraz EL1904 i 2904 odpowiedziane za bezpieczne wejścia/wyjścia).

 ![safety5](safety5.png "Safety5")
 
## Dodawanie nowego projektu Safety
Kolejnym krokiem utworzenie jest nowego projektu w zakładce SAFETY okna Solution Explorer

![safety6](safety6.png "Safety6")

Następnie dodać należy nowy projekt TwinCAT Safety.

![safety7](safety7.png "Safety7")

W kreatorze wpisać należy podstawowe dane projektu, takie jak Autor i Nazwa wewnętrzna projektu.

![safety8](safety8.png "Safety8")

##	Konfiguracja sprzętowa urządzeń safety
Następnie należy przejść do wyboru urządzenia (Target System) z odpowiedniej zakładce Solution Explorer (1). 

![safety9](safety9.png "Safety9")

W otwartym oknie wybrać należy model sterownika (2) oraz powiązać go z fizycznym urządzeniem (3), które chcemy konfigurować. W przypadku  niniejszego przykładu modułem odpowiedzialnym za logikę jest EL6910.

![safety10](safety10.png "Safety10")

Zaznaczyć należy opcje widoczne po prawej stronie ekranu. Ramką zaznaczono również numer seryjny urządzenia, którego podanie wymagane będzie podczas wgrywania projektu do sterownika. Na tym etapie zweryfikować należy również adres sprzętowy dodawanego urządzenia.

![safety11](safety11.png "Safety11")

Opcje zaznaczone czerwonymi strzałkami odpowiedzialne są za utworzenie w module logicznym dodatkowego Process Image (zmienne wysyłane co cykl w ramce EtherCAT) z diagnostyką połączeń. Zmienne o standardowych nazwach przyjmą wówczas nazwy z naszej aplikacji.
<br>
Dane te mogą (nie muszą) być używane TYLKO jako dane diagnostyczne po stronie PLC. Mamy możliwość łatwego linkowania stanów modułów, stanu jego wejść czy nawet diagnozy Safe FB (po odpowiedniej dalszej konfiguracji).
<br>
Kolejnym krokiem jest dodanie modułów bezpiecznych wejść (EL1904) i wyjść (EL2904) w zakładce Alias Devices. W prezentowanym przykładzie wykorzystywana jest jedna grupa, ale na potrzeby bardziej rozbudowanych aplikacji zastosować można więcej grup odpowiedzialnych na przykład za zabezpieczenie oddzielnych stref lub maszyn objętych aplikacją. Takie rozwiązanie pozwala na pracę części grup w przypadku przejścia innych w stan bezpieczny lub stan błędu. 

![safety12](safety12.png "Safety12")

Jeżeli konfiguracja sprzętowa została wcześniej prawidłowo zeskanowana wystarczy zaznaczyć urządzenia, z których korzystać będziemy w projekcie.

![safety13](safety13.png "Safety13")

Po powiązaniu urządzeń fizycznych otrzymamy wiadomość potwierdzającą.
 
![safety14](safety14.png "Safety14")

