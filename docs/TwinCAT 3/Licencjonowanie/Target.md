---
title: Target
parent: - Licencjonowanie
nav_order: 1
layout: page
---

# Licencjonowanie Target 
<h6> Data modyfikacji: 20.12.2024 </h6>
<br>

Niniejsza  instrukcja  opisuje  sposób  samodzielnego  aktywowania licencji bezpośrednio na sterowniku.

Instrukcja  nie  opisuje  przypadku  wgrywania  licencji  na  moduł EL6070 lub klucz USB C9900-L100.

## Protokół odbioru

Na początku warto sprawdzić zakupione licencje. Do realizacji procesu licencjonowania będzie potrzebny numer „BADE-IN” (wpisujemy tylko znaki zaznaczone na zielono). Znajdziemy je na Fakturze VAT lub na Potwierdzeniu zamówienia.

![protokol](https://ba-pl.github.io/wiki/assets/images/protokol.png "protokol")


TCxxxx-xx**40** – **40** oznacza platformę sprzętową.

## TwinCAT

Mając protokół odbioru, otwieramy Solution i nawiązujemy połączenie ze sterownikiem, na który chcemy wgrać licencje. Następnie otwieramy zakładkę licencyjną.

W zakładce *License / Manage Licenses* zaznaczamy *Disable automatic detection of required licenses for project* oraz wybieramy licencje zgodne z zamówieniem (tylko te, które są na zamówieniu).

![listalicencji](https://ba-pl.github.io/wiki/assets/images/listalicencji.png "listalicencji")

Następnie w zakładce *Order information (Runtime)* dopasowujemy licencję do urządzenia docelowego (dla sterownika – Target, z reguły nie ma potrzeby ustawiać ) 

![licensedevice](https://ba-pl.github.io/wiki/assets/images/licensedevice.png "licensedevice")

Następnie wpisujemy numer zamówienia (tylko cyfry z numeru BADE-IN) oraz wybieramy platformę zgodną z zamówieniem (powinna zostać rozpoznana automatycznie).

![pl](https://ba-pl.github.io/wiki/assets/images/pl.png "pl")


Docelowa platforma zdefiniowana jest w nazwie licencji na zamówieniu np. TC1000-00**40**

![plorder](https://ba-pl.github.io/wiki/assets/images/plorder.png "plorder")


Wybieramy *Generate File* i zapisujemy plik w dowolnym miejscu na komputerze.

![generateFile](https://ba-pl.github.io/wiki/assets/images/generateFile.png "generateFile")


Jeżeli mamy skonfigurowaną pocztę to możemy automatycznie utworzyć maila wybierając:

![SendRequest](https://ba-pl.github.io/wiki/assets/images/SendRequest.png "SendRequest")

Wygenerowany przez nas plik wysyłamy w załączniku na adres **tclicense@beckhoff.com**. Zapytanie jest automatycznie przetwarzane przez serwer. Treść maila nie ma znaczenia.  

![LicenseMail](https://ba-pl.github.io/wiki/assets/images/LicenseMail.png "LicenseMail")

Odpowiedź powinna pojawić się w ciągu kilku minut.

W odpowiedzi otrzymamy plik rejestracyjny – zapisujemy go chwilowo w dowolnym miejscu na swoim komputerze.

W opisie widzimy nazwy wszystkich licencji w pakiecie.

![LicenseResponse](https://ba-pl.github.io/wiki/assets/images/LicenseResponse.png "LicenseResponse")

Dodajemy otrzymany plik rejestracyjny przyciskiem License Response File:

![ResponseDownload](https://ba-pl.github.io/wiki/assets/images/ResponseDownload.png "ResponseDownload")

![ResponseDownload2](https://ba-pl.github.io/wiki/assets/images/ResponseDownload2.png "ResponseDownload2")

Aktywujemy konfigurację TwinCATa:

![ActivateConfiguration](https://ba-pl.github.io/wiki/assets/images/ActivateConfiguration.png "ActivateConfiguration")

Licencje są już aktywne:

![Valid](https://ba-pl.github.io/wiki/assets/images/Valid.png "Valid")








