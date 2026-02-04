---
title: IoT Communicator
parent: Protokoły komunikacyjne
nav_order: 2
layout: page
---

# IoT Communicator 
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Wykorzystana konfiguracja

W celu opracowania instrukcji posłużono się następującą konfiguracją sprzętową:
- Sterownik C6015-0010 z systemem operacyjnym Windows 10 oraz TwinCAT 3.1.4024.53
- Telefon komórkowy z dostępem do Internetu oraz pobraną aplikacją TwinCAT IoT
- Komputer z uruchomionym brokerem MQTT

<br>
Intrukcja przeprowadza użytkownika krok po kroku poprzez podstawy konfiguracji i uruchomienia przykładowego projektu używającego biblioteki Tc3_IoTCommunicator.
## Opis technologii 

Biblioteka Tc3_IotCommunicator pozwala na wymianę danych pomiędzy programem PLC a brokerem poprzez
protokół komunikacyjny MQTT. Rozwiązanie takie pozwala np. na zdalne odczytywanie zmiennych statusowych
procesu (subscribe mode) lub zadawanie parametrów (publish mode) bez konieczności fizycznego przebywania w
pobliżu sterownika czy komputera, z którego sterownik był programowany. W przykładzie przedstawiona zostanie
komunikacja dla dwóch pokojów, z czego jeden będzie zabezpieczony nazwą użytkownika i hasłem, a drugi nie.

![iot_comm1](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm1.png "iot_comm1")

# Przygotowanie sterownika oraz program PLC
## Uruchomienie brokera MQTT

W tej instrukcji instalacja brokera MQTT zostanie przedstawiona w sposób skrócony. W celu otrzymania
dokładnej instrukcji prosimy o kontakt poprzez skrzynkę mailową **support@beckhoff.pl**.
<br>
<br>
W pierwszej kolejności należy zainstalować broker MQTT (w naszym przypadku będzie to Eclipse Mosquitto), a następnie odblokować port 1883 na urządzeniu serwera oraz klienta. Następnie na urządzeniu, które ma być brokerem przy pomocy linii poleceń uruchamiamy Mosquitto. W dalszej kolejności można przygotować plik konfiguracyjny dla użytkowników (poprawia to bezpieczeństwo połączenia, lecz nie jest konieczne dla poprawnego działania komunikacji). Należy pamiętać aby broker był cały czas uruchomiony podczas komunikacji!
<br>
Dla wersji 2.0.0 oraz nowszych, wymagana jest zmiana domyślnej konfiguracji, aby zezwolić na dostęp innych
urządzeń do brokera. W tym celu należy edytować plik konfiguracyjny, znajdujący się w folderze instalacyjnym, o
nazwie **mosquitto.conf**. Potrzebne parametry:
- listener 1883 0.0.0.0 – deklarujemy port oraz adres po których będzie przebiegała komunikacja (0.0.0.0
oznacza dostęp z dowolnego adresu)
- allow_anonymous true – zezwalamy na połączenia z innych urządzeń

![iot_comm2](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm2.png "iot_comm2")

Aby załadować nową konfigurację, należy zapisać edytowany plik oraz w linii poleceń cmd uruchomić broker:
<br>
**mosquitto -v -c mosquitto.config**

![iot_comm3](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm3.png "iot_comm3")

<br>
Pełna dokumentacja pliku konfiguracyjnego dostępna na stronie [mosquitto](https://mosquitto.org/man/mosquitto-conf-5.html).

## Przygotowanie sterownika

W pierwszej kolejności należy połączyć się ze sterownikiem, a następnie odblokować w firewall port 1883 dla
połączeń przychodzących i wychodzących. Dodatkowa instalacja biblioteki nie jest konieczna, ponieważ biblioteka
Tc3_IotCommunicator jest domyślnie zainstalowana w wersji TwinCAT 3.1.4022 oraz wyższych. W przypadku
uruchamiania brokera MQTT na urządzeniu posiadającym adres IP przydzielany przy pomocy serwera DHCP należy
się upewnić, czy urządzenie mobilne, sterownik i urządzenie na którym uruchomiony jest broker znajdują się w tej
samej podsieci.

## Przygotowanie struktury danych

Należy uruchomić środowisko TwinCAT XAE, a następnie dołączyć do programu PLC biblioteki
Tc3_IotCommunicator oraz Tc3_Module. W następnej kolejności należy utworzyć strukturę danych, która
przesyłana będzie przy pomocy protokołu MQTT, np. jak poniżej:

```
TYPE ST_ProcessData :
STRUCT
    {attribute 'iot.DisplayName' := 'Kitchen Lights'}
    bLamp1 : BOOL;
    {attribute 'iot.DisplayName' := 'Living Room Lights'}
    bLamp2 : BOOL;
    {attribute 'iot.DisplayName' := 'Outside Temperature'}
    {attribute 'iot.ReadOnly' := 'false'}
    {attribute 'iot.Unit' := 'Celsius'}
    {attribute 'iot.MinValue' := '5'}
    {attribute 'iot.MaxValue' := '30'}
    nTemp  : REAL;
END_STRUCT
END_TYPE

```

Atrybut iot.DisplayName odpowiada za wyświetlanie nazwy zmiennej w urządzeniu klienta, atrybut ReadOnly
pozwala na ustawienie braku możliwości zmiany wartości zmiennej, atrybut Unit określa jednostkę w jakiej
wyświetlana jest wartość, a atrybut MinValue i MaxValue pozwala określić zakres wyświetlania zmiennej w
urządzeniu klienta.

## Przygotowanie funkcji do komunikacji 

W celu zapewnienia komunikacji przy pomocy protokołu MQTT używa się bloku funkcyjnego
FB_IotCommunicator dostępnego w bibliotece Tc3_IotCommunicator. Blok ten posiada następujące wejścia:
- sHostName – adres IP lub Hostname brokera MQTT
- nPort – port wykorzystywany do komunikacji (w naszym przypadku port 1883)
- sClientID – opcjonalne wejście dla ustawienia unikalnej nazwy klienta
- sMainTopic – nazwa głównego tematu
- sDeviceName – nazwa pokoju
- sUser – nazwa użytkownika (gdy skonfigurowane w brokerze)
- sPassword – hasło (gdy skonfigurowane w brokerze)
- stTLS – struktura dla komunikacji zabezpieczanej przy pomocy TLS
- bRetain – zmienna dla ustalenia, czy broker ma przechowywać poprzednie wiadomości
- eQoS – zmienna dla „Quality of Service”
<br>
<br>
Wyjścia:
- bError – gdy wystąpi błąd
- hrErrorCode – kod błędu
- eConnectionState – stan komunikacji między klientem I brokerem
- bConnected – TRUE jeśli jest poprawna komunikacja między klientem i brokerem
- fbCommand – wyjscie do ewaluacji otrzymanych danych („komend”)
<br>
<br>
Oraz metody:
- Execute – wywoływana cyklicznie dla utrzymania komunikacji
- SendData – metoda do wysłania danych do brokera
- SendMessage – metoda do wysłania wiadomości do brokera

## Komunikacja bez podawania użytkownika i hasła (Opcja 1) 

W celu zapewnienia podstawowej komunikacji (bez zabezpieczenia przy pomocy podawania nazwy
użytkownika i hasła), konfiguracja wygląda jak na obrazku poniżej (deklarujemy tylko 4 zmienne wejściowe):

```
fbIoT			: FB_IotCommunicator := (
						sHostName		:= '10.201.34.29',
						nPort			:= 1883,
						sMainTopic		:= 'MyMainTopic',
						sDeviceName		:= 'Room One');

```

![iot_comm6x](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm6x.png "iot_comm6x")

Należy pamiętać, że broker MQTT również powinien być uruchomiony bez opcji zakładającej nazwę
użytkownika i hasło dla komunikacji.

## Komunikacja z podawaniem nazwy użytkownika i hasła 

W celu zapewnienia komunikacji bezpieczniejszej (zabezpieczonej nazwą użytkownika i hasłem) poza
zmiennymi zadeklarowanymi w przykładzie powyżej należy zadeklarować również w bloku zmienne sUser oraz
sPassword, jak na przykładzie poniżej (muszą być one zgodne z użytkownikiem i hasłem skonfigurowanymi dla
brokera):

```
fbIoT2			: FB_IotCommunicator := (
						sHostName		:= '10.24.2.38',
						nPort			:= 1883,
						sMainTopic		:= 'MyMainTopic',				
						sDeviceName		:= 'Room Two',
						sUser			:= 'Administrator',
						sPassword		                   := '1');
```

W aplikacji mobilnej należy zaznaczyć opcję „Authentication”, a następnie wpisać nazwę użytkownika i hasło w
oknie, które się pojawi:

![iot_comm8x](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm8x.png "iot_comm8x")

Należy pamiętać, że broker MQTT również powinien być uruchomiony z opcją zakładającą nazwę użytkownika
i hasło dla komunikacji.

## Przygotowanie struktury programu 

W celu zapewnienia cyklicznej komunikacji klienta z brokerem należy co cykl wywoływać metodę Execute dla
funkcji FB_IotCommunicator:

```
 fbIoT.Execute(TRUE);								//keep communication alive
fbIoT2.Execute(TRUE);								//keep communication alive
```

Przesyłanie danych realizowane będzie co 500 ms przy pomocy timera TON, w przypadku gdy wyjście
bConnected bloku będzie w stanie TRUE (komunikacja będzie poprawna):

```
VAR
	stData			: ST_ProcessData;
	stData2			: ST_ProcessData;
END_VAR

timer(IN := NOT timer.Q, PT := T#500MS);						//cyclic message sending

IF bSendMessage THEN								//if TRUE
	bSendMessage := FALSE;							//set to FALSE
	fbIoT.SendMessage(sMessage);							//send message
END_IF

IF fbIoT.bConnected AND timer.Q THEN						//if 500 ms passed and communication ok
    fbIoT.SendData(ADR(stData), SIZEOF(stData));						//send data
END_IF

IF fbIoT2.bConnected AND timer.Q THEN						//if 500 ms passed and communication ok
    fbIoT2.SendData(ADR(stData2), SIZEOF(stData2));					//send data
END_IF
```

Powyższy fragment kodu służy do odczytu informacji z programu PLC np. za pomocą urządzenia mobilnego. Aby
możliwe było nadpisywanie wartości zmiennych z poziomu urządzenia klienta należy zaimplementować fragment
kodu jak poniżej:

```
IF fbIoT.fbCommand.bAvailable THEN							//if command sending available
    IF fbIoT.fbCommand.sVarName = 'bLamp1' THEN					//if Lamp 1
        fbIoT.fbCommand.GetValue(ADR(stData.bLamp1), SIZEOF(stData.bLamp1), E_IotCommunicatorDatatype.type_BOOL);	 //set new value  
ELSIF fbIoT.fbCommand.sVarName = 'bLamp2' THEN					//if Lamp 2
        fbIoT.fbCommand.GetValue(ADR(stData.bLamp2), SIZEOF(stData.bLamp2), E_IotCommunicatorDatatype.type_BOOL);	//set new value  
ELSIF fbIoT.fbCommand.sVarName = 'nTemp' THEN					//if Temp
        fbIoT.fbCommand.GetValue(ADR(stData.nTemp), SIZEOF(stData.nTemp), E_IotCommunicatorDatatype.type_REAL);	//set new value  
    END_IF
    fbIoT.fbCommand.Remove();							//discard command 
END_IF


IF fbIoT2.fbCommand.bAvailable THEN						//if command sending available
    IF fbIoT2.fbCommand.sVarName = 'bLamp1' THEN					//if Lamp 1
        	fbIoT2.fbCommand.GetValue(ADR(stData2.bLamp1), SIZEOF(stData2.bLamp1), E_IotCommunicatorDatatype.type_BOOL); //set new value  
		ELSIF fbIoT2.fbCommand.sVarName = 'bLamp2' THEN				//if Lamp 2
        	fbIoT2.fbCommand.GetValue(ADR(stData2.bLamp2), SIZEOF(stData2.bLamp2), E_IotCommunicatorDatatype.type_BOOL);//set new value  
		ELSIF fbIoT2.fbCommand.sVarName = 'nTemp' THEN				//if Temp
       		 fbIoT2.fbCommand.GetValue(ADR(stData2.nTemp), SIZEOF(stData2.nTemp), E_IotCommunicatorDatatype.type_REAL);	//set new value  
   		END_IF
    fbIoT2.fbCommand.Remove();							//discard command 
END_IF
```

Kod ten pozwala na nadpisanie wartości zmiennej w przypadku gdy dostępna jest możliwość wysłania nowej
komendy do brokera (fbIoT.fbCommand.bAvailable). 

# Aplikacja mobilna

W celu odczytu i zapisywania danych w programie PLC z poziomu telefonu komórkowego należy pobrać i zainstalować aplikację TwinCAT IoT (dostępna w sklepie Google Play oraz AppStore). Następnie w aplikacji należy przejść do zakładki Settings, gdzie dokonujemy konfiguracji urządzenia klienta. W polu Broker Address wpisujemy adres IP lub Hostname brokera, w polu Port wpisujemy port komunikacyjny, w polu Client ID - ID klienta (o ile jest wykorzystywane w aplikacji), w polu Topic nazwę tematu.

![iot_comm12x](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm12x.png "iot_comm12x")

## Uruchomienie aplikacji mobilnej 

Po uruchomieniu programu PLC na sterowniku oraz aplikacji mobilnej otrzymujemy możliwość odczytywania i
zmiany wartości parametrów programu PLC przy pomocy aplikacji mobilnej, jak przedstawiono to na poniższych
obrazkach.

### Uruchomienie bez nazwy użytkownika i hasła 

W pierwszej kolejności uruchomiono komunikację niechronioną nazwą użytkownika i hasłem. Widok działającej
aplikacji przedstawiono na obrazkach poniżej:

![iot_comm13x](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm13x.png "iot_comm13x")

Jak można zauważyć, oba pokoje są w trybie online, mimo że drugi z nich został zadeklarowany jako chroniony
nazwą użytkownika i hasłem. Spowodowane jest to uruchomieniem brokera bez opcji logowania użytkowników.

### Uruchomienie chronione nazwą użytkownika i hasłem 

Następnie uruchomiono komunikację wymagającą zalogowania się przy pomocy nazwy użytkownika i hasła. Konfigurację działającej aplikacji przedstawiono na obrazkach poniżej. Login oraz hasło mogą zostać wprowadzone dopiero po rozwinęciu sekcji Authentication.

![iot_comm14x](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm14x.png "iot_comm14x")

## Dodatkowe funkcje 

### Nadpisywanie wartości 

Opcja nadpisywania wartości dostępna jest po kliknięciu na daną wartość, a następnie pojawi się możliwość
wpisania żądanej wartości.

![iot_comm15x](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm15x.png "iot_comm15x")

### Opcje dodatkowe

W zakładce Settings IoT Communicatora można znaleźć dodatkowe konfiguracje wpływające na działanie aplikacji:

![iot_comm16x](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm16x.png "iot_comm16x")

- Keep Screen On: nie gaś ekranu telefonu
- Lock Application: aby ponownie wejść do aplikacji należy użyć autentykacji telefonu
- Data as default: domyślnie jest otwierana lista pomieszczeń zamiast powiadomień
- Show QR Button: pokaż kod QR do udostępnienia konfiguracji połączenia
- Toggle Booleans: jeżeli jest zaznaczone proste kliknięcie pola boolowskiego przełącza jego stan. Jeżeli nie, należy w drugim kroku po kliknięciu wybrać na który stan chcemy przełączyć.
- Display Booleans as Switch: pokazuj switcha na polach ze zmienną Boolowską
- Display Number Precision: wyświetlaj wartości z podaną precyzją


### Live graph 

Opcja „Live graph” pozwala na monitorowanie wartości zmiennych na wykresie. Aby ją uruchomić, należy
kliknąć ikonę w prawym górnym rogu ekranu, a nastepnie wybrać zmienne które chcemy monitorować i w tym
samym miejscu uruchomić rysowanie wykresu.

![iot_comm17x](https://ba-pl.github.io/wiki/assets/images/IoT/iot_comm17x.png "iot_comm17x")

# Przykładowa aplikacja

Przykładową aplikację możesz pobrać tutaj:
<br>
<br>
[Download IoT Sample](https://github.com/BA-PL/IoTCommunicator/archive/refs/heads/main.zip){: .btn .btn-red }



