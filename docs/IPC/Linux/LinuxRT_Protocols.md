---
title: Wybrane protokoły komunikacyjne  
parent: Beckhoff RT Linux® 
nav_order: 5
layout: page
---

# Wybrane protokoły komunikacyjne
{: .no_toc }
<h6> Data modyfikacji: 03.02.2026 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# EtherCAT Automation Protocol

Założenia:
- komunikacja na zasadzie Publisher-Subscriber za pomocą EtherCAT Automation Protocol
- wymiana danych pomiędzy
	- CX8290
	- CX9240 z opcją M910 (dodatkowy interfejs sieciowy)

Przygotowanie:
- na obu sterownikach jest zainstalowany tc31-xar-um 
- opcjonalnie, dla interfejsu tctap0 ustawiamy statyczny adres IP, zgodnie z instrukcją [tutaj](https://infosys.beckhoff.com/english.php?content=../content/1033/beckhoff_rt_linux/18089353227.html)

![eap1](https://ba-pl.github.io/wiki/assets/images/linux/eap1.png "eap1")

## CX9240 Publisher

Po utworzeniu projektu i połączeniu się ze sterownikiem, skanujemy w Config Mode urządzenia.

Ważne, aby zeskanować intrfejs CCAT:

![eap2](https://ba-pl.github.io/wiki/assets/images/linux/eap2.png "eap2")

Następnie dodajemy EAP:

![eap3](https://ba-pl.github.io/wiki/assets/images/linux/eap3.png "eap3")

W oknie, które się pojawy wybieramy opcję **Slot 1 (MAC-DMA) Switched**:

![eap4](https://ba-pl.github.io/wiki/assets/images/linux/eap4.png "eap4")

Jest to ten interfejs:

![eap4a](https://ba-pl.github.io/wiki/assets/images/linux/eap4a.png "eap4a")

Po tych zmianach, zakładka adapter powinna wyglądać tak:

![eap5](https://ba-pl.github.io/wiki/assets/images/linux/eap5.png "eap5")

Następnie dodajemy dane do konfiguracji EAP, w przykładzie będzie publikowana jedna zmienna typu UDINT:

![eap6](https://ba-pl.github.io/wiki/assets/images/linux/eap6.png "eap6")

![eap7](https://ba-pl.github.io/wiki/assets/images/linux/eap7.png "eap7")

Następnie do pola **VarData** linkujemy przygotowaną wcześniej w programie PLC zmienną:

![eap8](https://ba-pl.github.io/wiki/assets/images/linux/eap8.png "eap8")

Aktywujemy konfigurację i uruchamiamy program.
W tym momencie w zakładce *Online* dla **VarData** powinniśmy widzieć wartość zmiennej z PLC:

![eap9](https://ba-pl.github.io/wiki/assets/images/linux/eap9.png "eap9")


## CX8290 Subscriber

Po utworzeniu projektu i połączeniu się ze sterownikiem, skanujemy w Config Mode urządzenia.

Ważne, aby zeskanować intrfejs CCAT:

![eap10](https://ba-pl.github.io/wiki/assets/images/linux/eap10.png "eap10")

Następnie dodajemy EAP:

![eap3](https://ba-pl.github.io/wiki/assets/images/linux/eap3.png "eap3")

W oknie, które się pojawy wybieramy opcję **Slot 1 (MAC-DMA) Switched**:

![eap4](https://ba-pl.github.io/wiki/assets/images/linux/eap4.png "eap4")

Jest to ten interfejs:

![eap11](https://ba-pl.github.io/wiki/assets/images/linux/eap11.png "eap11")

Po tych zmianach, zakładka adapter powinna wyglądać tak:

![eap12](https://ba-pl.github.io/wiki/assets/images/linux/eap12.png "eap12")

Następnie dodajemy dane do konfiguracji EAP element **Subscriber**:

![eap13](https://ba-pl.github.io/wiki/assets/images/linux/eap13.png "eap13")

Do elementu **Subscriber** dodajemy kolejny element i jeśli mamy połączenie sieciowe pomiędzy sterownikami, możemy wybrać opcję **Browse for Computer** i wybrać adres Publishera:

![eap14](https://ba-pl.github.io/wiki/assets/images/linux/eap14.png "eap14")

Powinna pojawić się lista zmiennych, w przykładzie 1 zmienna typu UDINT. Zaznaczamy ją i dodajemy do konfiguracji przyciskiem **OK**:

![eap15](https://ba-pl.github.io/wiki/assets/images/linux/eap15.png "eap15")

Pojawi się komunikat:

![eap16](https://ba-pl.github.io/wiki/assets/images/linux/eap16.png "eap16")

który możemy potwierdzić.
<br>
Do pola **VarData** linkujemy przygotowaną wcześniej w programie PLC zmienną. 

![eap17](https://ba-pl.github.io/wiki/assets/images/linux/eap17.png "eap17")

Aktywujemy konfigurację i wgrywamy aplikację. Wartość zmiennej powinna się aktualizować zarówno w zakładce *Online* zmiennej **VarData** jak i w programie PLC:

![eap18](https://ba-pl.github.io/wiki/assets/images/linux/eap18.png "eap18")



