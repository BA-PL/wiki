---
title: VPN dla WinCE 7
parent: Windows Embedded Compact
nav_order: 4
layout: page
---


# Konfiguracja klienta VPN pod Windows CE 7.0
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

**Założenia**

![vpn1](https://ba-pl.github.io/wiki/assets/images/vpn1.png "vpn1")

# Konfiguracja VPN Serwera na Windows 7+

1. Przechodzimy do zakładki ustawień kart sieciowych:

![vpn2](https://ba-pl.github.io/wiki/assets/images/vpn2.png "vpn2")

2. Dodajemy nowe połączenie przychodzące- jeśli przycisk PLIK jest nie widoczny, można spróbować go uaktywnić klikając lewy ALT

![vpn3](https://ba-pl.github.io/wiki/assets/images/vpn3.png "vpn3")

3. Konfigurujemy użytkowników, którzy mają mieć dostęp do zdalnego połączenia- użytkowników VPN. Można dodać nowych. W dalszej części można ich również wyedytować.

![vpn4](https://ba-pl.github.io/wiki/assets/images/vpn4.png "vpn4")

![vpn5](https://ba-pl.github.io/wiki/assets/images/vpn5.png "vpn5")

4. Klikamy zezwalaj na dostęp
5. Zamykamy okienko.
6. Skonfigurowaliśmy VPN Serwer:

![vpn6](https://ba-pl.github.io/wiki/assets/images/vpn6.png "vpn6")

![vpn7](https://ba-pl.github.io/wiki/assets/images/vpn7.png "vpn7")

7. Aby Serwer VPN działał prawidłowo, musi być zapewniony dostęp do portu TCP 1723. Jeżeli serwer działa na laptopie za bramą (Router B), trzeba zapewnić przekierowanie portów (port fowarding).
8. Z poziomu Windowsa CE przechodzimy do ControlPanel:
9. START > Control Panel > CX Configuration > RAS Control
10. Enablujemy RAS Servwer
11. Włączamy połączenie RAS VPN Line 0 (Enable Line)

![vpn8](https://ba-pl.github.io/wiki/assets/images/vpn8.png "vpn8")

12. Zamykamy okno. Przechodzimy do ustawień sieci: Network And Dial-Up Connections
13. Dodajemy nowe połączenie PPTP

![vpn9](https://ba-pl.github.io/wiki/assets/images/vpn9.png "vpn9")

14. Podajemy ZEWNĘTRZNY adres IP do naszego komputera z VPN Serwerem. Można go sprawdzić np. za pomocą strony [moje IP](https://www.myip.cz)

![vpn10](https://ba-pl.github.io/wiki/assets/images/vpn10.png "vpn10")

15. W zakładce Secutity Settings włączamy szyfrowanie danych i autentykację logowania:

![vpn11](https://ba-pl.github.io/wiki/assets/images/vpn11.png "vpn11")

![vpn12](https://ba-pl.github.io/wiki/assets/images/vpn12.png "vpn12")

16. Klikamy Conect
17. Podajemy usera i hasło zdefiniowane w VPN Serwerze
18. Pierwsza próba połączenia może trwać trochę dłużej

![vpn13](https://ba-pl.github.io/wiki/assets/images/vpn13.png "vpn13")

19. Po wdzwonieniu się, na serwerze również widnieje status połączenia:

![vpn14](https://ba-pl.github.io/wiki/assets/images/vpn14.png "vpn14")

![vpn15](https://ba-pl.github.io/wiki/assets/images/vpn15.png "vpn15")

20. Stworzyliśmy VPN.
21. Można odczytać adres VPN clienta (np. cmd > ipconfig /all):

![vpn16](https://ba-pl.github.io/wiki/assets/images/vpn16.png "vpn16")

22. Oraz adres serwera

![vpn17](https://ba-pl.github.io/wiki/assets/images/vpn17.png "vpn17")

23. Aby wyszukać sterownik za pomocą TwinCAT’a trzeba podać ten adres:

![vpn18](https://ba-pl.github.io/wiki/assets/images/vpn18.png "vpn18")

24. Dalsze oprogramowywanie sterownika jest analogiczne jak jednostki będącej w tej samej podsieci.

# Ustawienia statycznego IP

![vpn19](https://ba-pl.github.io/wiki/assets/images/vpn19.png "vpn19")

![vpn20](https://ba-pl.github.io/wiki/assets/images/vpn20.png "vpn20")

# Szyfrowanie autoryzacji połączenia

![vpn21](https://ba-pl.github.io/wiki/assets/images/vpn21.png "vpn21")

Domyślnie połączenie szyfrowane jest za pomocą MS CHAP 2. Ustawienia zaawansowane są dostępne z poziomu rejestrów (opis w linkach).

# Autonawiązywanie połączenia
Autonawiązywanie połączenia możliwe jest do skonfigurowania z poziomu StartUp Managera (opisane w poniższych linkach).

# Pomocne linki
- [RAS Server](https://infosys.beckhoff.com/content/1033/sw_os/2018453387.html?id=1878917756769304675)
- [StartUp Manager](https://infosys.beckhoff.com/english.php?content=../content/1033/sw_os/7139932171.html&id=2917787884096561411)
- [MDP RAS status](https://infosys.beckhoff.com/content/1033/devicemanager/36028797281982731.html?id=779494138443119193) 