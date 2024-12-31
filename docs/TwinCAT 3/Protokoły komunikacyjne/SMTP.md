---
title: SMTP
parent: - Protokoły komunikacyjne
nav_order: 6
layout: page
---


# SMTP
{: .no_toc }
<h6> Data modyfikacji: 20.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Niniejsza instrukcja omawia sposób korzystania z serwera TwinCAT SMS/SMTP w celu przesyłania wiadomości e-mail bezpośrednio ze sterownika PLC za pomocą bloków funkcyjnych. Serwer ten umożliwia również przesyłanie załączników plików, formatów HTML oraz ustawianie priorytetów wiadomości. Obsługa STARTTLS / SSL umożliwia skonfigurowanie szyfrowanej komunikacji e-mail.

# Instalacja
W celu wykorzystania możliwości przesyłania wiadomości lub e-maili należy w pierwszej kolejności pobrać i zainstalować na komputerze/sterowniku dodatek TF6350. Znajduję się on na stronie www.beckhoff.com.

![smtp1](https://ba-pl.github.io/wiki/assets/images/smtp1.png "smtp1")

# Dodanie bibliotek
Do projektu należy dodać odpowiednią bibliotekę. W oknie Solution Explorer wejdź w zakładkę PLC -> References i wybierz Add library:

![smtp2](https://ba-pl.github.io/wiki/assets/images/smtp2.png "smtp2")

Znajdź i dodaj bibliotekę Tc2_Smtp:

![smtp3](https://ba-pl.github.io/wiki/assets/images/smtp3.png "smtp3")

Po dodaniu biblioteki i przy późniejszej próbie wgrania aplikacji, należy aktywować licencję (do testów będzie do 7-dniowa licencja trial).

# Konfiguracja poczty 
TwinCAT SMTP może komunikować się z zewnętrznymi dostawcami poczty. Do wysyłania wiadomości e-mail należy utworzyć konto pocztowe, w poniższym przykładzie użyto poczty gmail, ze względu na darmowy dostęp.

## Tworzenie konta 
Należy wejść na stronę https://accounts.google.com/signup. Pojawi się okno dialogowe, w którym należy wypełnić wymagane pola i przejść dalej i Utworzyć konto.

## Konfiguracja ustawień 
Po utworzeniu konta wejdź w **Ustawienia**: 

![smtp4](https://ba-pl.github.io/wiki/assets/images/smtp4.png "smtp4")

Kliknij **Zobacz wszystkie ustawienia**

![smtp5](https://ba-pl.github.io/wiki/assets/images/smtp5.png "smtp5")

Przejdź do zakładki **Przekazywanie i POP/IMAP**, w polu **Dostęp IMAP** wybierz opcje **Włącz IMAP**

![smtp6](https://ba-pl.github.io/wiki/assets/images/smtp6.png "smtp6")

Następnie przejdź na koniec strony i wybierz opcje **Zapisz zmiany**

## Włączenie weryfikacji dwuetapowej
Wejdź w ustawienia konta, wybierz **Aplikacje Google** i kliknij na **Konto Google**

![smtp7](https://ba-pl.github.io/wiki/assets/images/smtp7.png "smtp7")

Po otwarciu okna wybierz zakładkę **Bezpieczeństwo** znajdującą się na liście po Lewej stronie i Włącz weryfikacje dwuetapową

![smtp8](https://ba-pl.github.io/wiki/assets/images/smtp8.png "smtp8")

## Utworzenie hasła do aplikacji 
Po włączeniu weryfikacji dwuetapowej ponownie kliknij na pole weryfikacji:

![smtp9](https://ba-pl.github.io/wiki/assets/images/smtp9.png "smtp9")

Po otworzeniu nowego okna zjedź na dół i odszukaj **Hasła do aplikacji** następnie wybierz tą opcje

![smtp10](https://ba-pl.github.io/wiki/assets/images/smtp10.png "smtp10")

Rozwiń okno **Wybierz aplikacje**:

![smtp11](https://ba-pl.github.io/wiki/assets/images/smtp11.png "smtp11")

Wybierz **Inna Opcja**:

![smtp12](https://ba-pl.github.io/wiki/assets/images/smtp12.png "smtp12")

Wpisz dowolną Nazwę i kliknij **Wygeneruj**:

![smtp13](https://ba-pl.github.io/wiki/assets/images/smtp13.png "smtp13")

W oknie uzyskasz hasło, które będzie nam niezbędne do funkcjonowania naszego programu:

![smtp14](https://ba-pl.github.io/wiki/assets/images/smtp14.png "smtp14")

# Bloki funkcyjne

![smtp15](https://ba-pl.github.io/wiki/assets/images/smtp15.png "smtp15")

![smtp16](https://ba-pl.github.io/wiki/assets/images/smtp16.png "smtp16")

![smtp17](https://ba-pl.github.io/wiki/assets/images/smtp17.png "smtp17")

Blok FB_SMTPV3_FULL różni się od bloku FB_SMTPV3 dodatkowymi wejściami:

![smtp18](https://ba-pl.github.io/wiki/assets/images/smtp18.png "smtp18")

![smtp19](https://ba-pl.github.io/wiki/assets/images/smtp19.png "smtp19")

# Przykładowe programy 
**Przykład 1**

Poniżej przedstawiono program wykorzystując blok FB_SMTPV3 w celu wysłania wiadomości.

![smtp20](https://ba-pl.github.io/wiki/assets/images/smtp20.png "smtp20")

Poniżej przedstawiono zdjęcie ukazujące otrzymany e-mail:

![smtp21](https://ba-pl.github.io/wiki/assets/images/smtp21.png "smtp21")

**Przykład 2**

Poniżej przedstawiono program wykorzystując blok FB_SMTPV3_FULL w celu wysłania wiadomości z załącznikami.

![smtp22](https://ba-pl.github.io/wiki/assets/images/smtp22.png "smtp22")

![smtp23](https://ba-pl.github.io/wiki/assets/images/smtp23.png "smtp23")




