---
title: Reporting 
parent: Rozszerzenia 
nav_order: 1
layout: page
---


# Raportowanie
{: .no_toc }
<h6> Data modyfikacji: 29.10.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# O systemie raportowania 

TwinCAT HMI Reporting jest to rozszerzenie darmowe, wchodzace w skład serwera HMI. Pozwala ono na generowanie raportów w formacie HTML lub/i PDF. 

Zawartość raportu jest tworzona poprzez prekonfigurację formatki, która następnie może być zmodyfikowana za pomocą zdarzeń wywoływanych po stronie klienta HMI. 

## Wymagania
W celu uruchomienia dodatku wymagane jest doisntalowanie na komputerze sieciowym obok XAR pakietu `TwinCAT.XAR.ReportingServer`. Wymaga ono do uruchomienia `.NET Desktop Runtime 8`

Samo rozszerzenie można doinstalować poprzez interface graficzny *Package managera*  lub poprzez uruchomienie linii kodu w cmd po stronie IPC z serwerem HMI

```cmd
tcpkg install TwinCAT.XAR.ReportingServer
```

W momenie wykrycia licencji serwera HMI `TF2000` sam *Raporting server* również się automatycznie uruchomi.

## Interface w TwinCAT 3 HMI

Aby wykorzystać rozszerzenie raportujące w projekcie HMI w pierwzej kolejności należy je dodać jako nuget do istniejącego projektu \
[Instrukcje dodania nuget do projektu można znaleźć pod tym linkiem](https://infosys.beckhoff.com/english.php?content=../content/1033/te2000_tc3_hmi_engineering/17786609547.html&id=3274960559827440614)


Po zainstalowaniu pakietu TcHmiReporting, w następnym kroku ustalamy połączenie z Reporting serwerem. Realizujemy dane połączenie poprzez rekonfiguracje w zakładce *TcHmiReporting/Runtimes* 

![Zdjęcie z config](https://ba-pl.github.io/wiki/assets/images/Reporting/image.png)

W zakładce z diagnostyką możemy sprawdzić czy udało się ustalić połączenie z serwerem licencyjnym 
![Error connection with runtime](https://ba-pl.github.io/wiki/assets/images/Reporting/image-1.png)

W przypadku problemów z połączeniem należy sprawdzić:
 - czy licencja TF2000 jest uruchomiona
 - czy TcReportingServer jest włączony
 - uruchomić ponownie usługę TcReportingServer w celu przeładowania licencji
![Reporting Srv](https://ba-pl.github.io/wiki/assets/images/Reporting/image-2.png)

Prawidłowo działące połączenie powinno mieć następujący status 
![ReportingOk](https://ba-pl.github.io/wiki/assets/images/Reporting/image-3.png)

 Po skonfigurowaniu połączenia musimy stworzyć szablon naszego raportu.
 Będzie to główny schemad do którego dopiero będziemy dołączali zawartość

## Szablon raportu 
 ![Szablon raportu](https://ba-pl.github.io/wiki/assets/images/Reporting/image-4.png)


  Sam szablon składa się z następujących sektorów :
   - **Header** - nagłówek dokumentu
   - **StyleSheets** - opcjonalne pliki CSS pozwalające na stylizację dokumentu
   - **ScriptFiles** - pliki z kodem w JavaScript. Pozwalają na dodanie własnych funckjonalności uruchamianych w trakcie generowania raportu. Można również załączyć własne biblioteki
   - **DefaultData** - Pozwala na utworzenie tabelki z danymi które zawsze są widoczne w każdym raporcie
   - **Footer** - stopka dokumentu 
   - **Logo** - Logo wyświetlane w prawym górnym rogu dokumentu. Zalecany format PNG
   - **Publish locations** - miejsca gdzie raporty mają zostać utworzone. Raporty można gromadzić lokalnie na komputerze w wybranej lokalizacji. Możliwe jest również wysyłanie automatyczne raportów na wybrane adresy email. Domyślnie jest możliwa konfiguracja 3 typów raportów
   
![ReportLocation](image-6.png)
  
  - **Signature configuration** -  Możliwe jest utworzenie podpisów cyfrowych do walidacji oryginalności dokumentu. 

 W celu utworzenia najprostrzego raportu zaleca się utworzenie *Headera* oraz określenie *Publish location*

## Uzupełnienie zawartości raportu

![Report format](https://ba-pl.github.io/wiki/assets/images/Reporting/image-7.png)

W celu wygenerowania raportu, poza samym opisem szablonu należy jeszcze zparametryzować co ma zostać przekazane

Mamy możliwość dodania na ten moment 3 typów elementów
- *Tables* - pozwala na umieszczenie dowolnej tabelki. Zaleca się aby zmienna przekazywana do tabeli byłatablicą struktur.
- *Text fields* - jest to proste przekazania pary "Nazwa":"Wartość". Zaleca się jej użycie w przypadku kiedy mamy do przekazania pojedyńczą inforację.
- *HTML Containers* - niestandardowa zawartość jako plik HTML. Przykładowo do załączenia obrazka w svg
  
Same dane można pobrać za pomocą protokołu ADS  z dowolnego sterownika, OPC UA lub można przekazać poprzez utworzenie zmiennej Serwerowej a następnie dynamiczie z poziomu HMI je modyfikować.

Dodatkowymi parametrami które należy mieć na uwadze są parametry na dole listy konfiguracyjnej.

![Reports extended confgi](https://ba-pl.github.io/wiki/assets/images/Reporting/image-8.png)

*Export path* - jest to ścieżka gdzie poszczególne raporty mogą zostać umieszczone. Domyśnie jest to folder z lokalizacją serwera HMI (a dokładniej w jego zawartości .\TcHmiReporting\reports )

*Overwrite reports* - jest prostym przełączeniem że w folderze z raportami, dany typ raportu ma być nadpisywany bez poprzednich iteracji/bez kopii


### Przykład dla pojedyńczej zmiennej
 Przykładowa konfiguracja szablonu
 -  *Header* - 'Sample Header'
 -  *Publish location* - domyślne ustawienia. Format PDF

Nazwa raportu *ReportBasic*
  
Zmienna serwerowa: 

![Server symbol](https://ba-pl.github.io/wiki/assets/images/Reporting/image-9.png)

Konfiguracja pojedyńczego Text field'a:

![Text field](https://ba-pl.github.io/wiki/assets/images/Reporting/image-10.png)

Następnie w celu wygenerowania raportu należy za pomocą zdarzenia w HMI wprowadzić nazwę raportu który chcemy wygenerować jako ciąg string do zmiennej `%s%TcHmiReporting.OrderReport%/s%`. Wprowadzenie wartości można wykonać np poprzez event .onPressed dowolnego elemntu HMI.

![alt text](https://ba-pl.github.io/wiki/assets/images/Reporting/image-11.png)

Aby podejrzeć czy raport się wygenerował można użyć kontrolki FileExplorer. Kontrolka File explorer dodatkowo umożliwia nam utworzenie kopii, zmianę nazwy oraz zdalne pobranie pliku z raportem na nasz lokalny komputer. W celu uzyskania dostępu do folderu z raportami należy odpowiednio wcześniej skonfigurować Virtual directories serwera HMI !!!.

![FileExplorer](image-12.png)

![alt text](https://ba-pl.github.io/wiki/assets/images/Reporting/image-13.png)

Uzyskany raport

![alt text](https://ba-pl.github.io/wiki/assets/images/Reporting/image-14.png)

Raporty mozna również przeglądać bezpośrednio z poziomu HMI. W tym celu należy doinstalować nuget PDFViewer, a następnie przekazać ścieżkę do raportu. \
Samą ścieżkę można przekazywać dynamicznie, wykorzystując kontrolkę filemanager

![SampleFileExporerPDF](https://ba-pl.github.io/wiki/assets/images/Reporting/image-15.png)

### Przykład dla tablicy struktur

W systemie raportowania jest również możliwość wyświetlenia tablicy struktur.
Przykładowo, posiadając następujące dane: \
`arrData : ARRAY[1..100] OF ST_Data;`

```
TYPE ST_Data :
STRUCT
	sName : STRING;
	dtTimestamp : DT;
	iValue : INT;
END_STRUCT
END_TYPE
```

1. Mapujemy całą tablicę po stronie HMI Configuration
2. W ustawieniach TcHmiReporting, dodajemy nowy wpis w reportach.
   ![Table report config](https://ba-pl.github.io/wiki/assets/images/Reporting/image-16.png)

3. W polu symbol przeazujemy odnośnik do całej tablicy
4. Następnie w zakładce kolumns wprowadzamy nazwy zmiennych z tabeli które mają zostać wyrzucone jako kolumny.
W zakładce Title możemy wprowadzić nazwę kolumny, natomiast w column order która będzie to kolumna z kolei
![Table columns config](https://ba-pl.github.io/wiki/assets/images/Reporting/image-17.png)

Możliwe jest również sortowanie po wartości w parametrze `Order by` . Przykładowo wprowadzamy {Nazwa kolumny} oraz ASC/DESC. Np `Values DESC`.

W rezultacie otrzymujemy tabelkę.

![Table](https://ba-pl.github.io/wiki/assets/images/Reporting/image-18.png)
