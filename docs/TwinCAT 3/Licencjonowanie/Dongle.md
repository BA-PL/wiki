---
title: Dongle
parent: Licencjonowanie
nav_order: 2
layout: page
---


# Licencjonowanie Dongle
<h6> Data modyfikacji: 20.12.2024 </h6>
<br>
<br>
Niniejsza instrukcja opisuje sposób samodzielnego aktywowania licencji na urządzeniu licencyjnym takim jak EL6070 lub klucz USB C9900-L100.

Instrukcja nie opisuje przypadku wgrywania licencji bezpośrednio na sterownik.

## Protokół odbioru

Na początku warto sprawdzić zakupione licencje. Do realizacji procesu licencjonowania będzie potrzebny numer „BADE-IN” (wpisujemy tylko znaki zaznaczone na zielono). Znajdziemy je na Fakturze VAT lub na Potwierdzeniu zamówienia.

![protokol](https://ba-pl.github.io/wiki/assets/images/protokol.png "protokol")


TCxxxx-xx**40** – **40** oznacza platformę sprzętową.

## TwinCAT

Po podłączeniu sterownika skanujemy moduły w poszukiwaniu EL6070 (EtherCAT) lub C9900-L100 (USB).

W przypadku C9900-L100 przy pierwszym skanowaniu potwierdzamy generowanie unikalnego ID modułu.

![ESBID](https://ba-pl.github.io/wiki/assets/images/ESBID.png "ESBID")

Następnie w drzewie projektu klikamy PPM na License i dodajemy nowe urządzenie.

![AddDongle](https://ba-pl.github.io/wiki/assets/images/AddDongle.png "AddDongle")

Pojawi nam się nowa zakładka o nazwie Dongle 1, otwieramy ją i wybieramy wczytany moduł EL6070 lub C9900-L100:

![SearchDongle](https://ba-pl.github.io/wiki/assets/images/SearchDongle.png "SearchDongle")

![el6070](https://ba-pl.github.io/wiki/assets/images/el6070.png "el6070")

Jeśli moduł nie posiada w sobie jeszcze żadnych licencji, to przy pytaniu o aktywację wybieramy *Nie*:

![noStore](https://ba-pl.github.io/wiki/assets/images/noStore.png "noStore")

Jeśli klucz i id się zgadza zobaczymy status *Valid* (może być konieczne wciśnięcie przycisku *Reload Info*):

![DongleStatus](https://ba-pl.github.io/wiki/assets/images/DongleStatus.png "DongleStatus")

Zaznaczamy opcję *Update license cache using dongle content on startup*.

W zakładce *License / Manage Licenses* zaznaczamy *Disable automatic detection of required licenses for project* oraz wybieramy licencje zgodne z zamówieniem (tylko te, które są na zamówieniu).

![listalicencji](https://ba-pl.github.io/wiki/assets/images/listalicencji.png "listalicencji")

Następnie w zakładce *Order information (Runtime)* dopasowujemy licencję do urządzenia docelowego (w tym przypadku Dongle) 

![licensedevicedngle](https://ba-pl.github.io/wiki/assets/images/licensedevicedongle.png "licensedevicedongle")


Wybieramy urządzenie główne, dla którego generowana będzie licencja:

![targetdongle](https://ba-pl.github.io/wiki/assets/images/targetdongle.png "targetdongle")

Niewybrane licencje będą wyszarzone, nie biorą one udziału w dalszej części procesu:

![noactive](https://ba-pl.github.io/wiki/assets/images/noactive.png "noactive")

Następnie wpisujemy numer zamówienia (tylko cyfry z numeru BADE-IN) oraz wybieramy platformę zgodną z zamówieniem.

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

W zakładce **Dongle 1 / License Device** wybieramy **Store License on Dongle**:

![storedongle](https://ba-pl.github.io/wiki/assets/images/storedongle.png "storedongle")

Dodajemy otrzymany plik rejestracyjny:

![storedongle2](https://ba-pl.github.io/wiki/assets/images/storedongle2.png "storedongle2")

![ResponseDownload2](https://ba-pl.github.io/wiki/assets/images/ResponseDownload2.png "ResponseDownload2")

Po prawidłowym wgraniu pliku dostaniemy komunikat z aktualną liczbą plików przechowywanych w module.

![storage](https://ba-pl.github.io/wiki/assets/images/storage.png "storage")

Aby wgrać licencję z modułu do pamięci sterownika wybieramy Cache Dongle Licenses.

![cachelicense](https://ba-pl.github.io/wiki/assets/images/cachelicense.png "cachelicense")

Wygenerowany plik zostanie umieszczony na sterowniku w katalogu instalacyjnym TwinCAT ...\TwinCAT\3.1\Target\License

Aktywujemy konfigurację TwinCATa:

![ActivateConfiguration](https://ba-pl.github.io/wiki/assets/images/ActivateConfiguration.png "ActivateConfiguration")

Licencje są już aktywne:

![validdongle](https://ba-pl.github.io/wiki/assets/images/validdongle.png "validdongle")












