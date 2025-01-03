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

![vpn1](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn1.png "vpn1")

# Konfiguracja VPN Serwera na Windows 7+

- Przechodzimy do zakładki ustawień kart sieciowych:

![vpn2](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn2.png "vpn2")

- Dodajemy nowe połączenie przychodzące- jeśli przycisk PLIK jest nie widoczny, można spróbować go uaktywnić klikając lewy ALT

![vpn3](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn3.png "vpn3")

- Konfigurujemy użytkowników, którzy mają mieć dostęp do zdalnego połączenia- użytkowników VPN. Można dodać nowych. W dalszej części można ich również wyedytować.

![vpn4](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn4.png "vpn4")

![vpn5](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn5.png "vpn5")

- Klikamy zezwalaj na dostęp
- Zamykamy okienko.
- Skonfigurowaliśmy VPN Serwer:

![vpn6](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn6.png "vpn6")

![vpn7](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn7.png "vpn7")

- Aby Serwer VPN działał prawidłowo, musi być zapewniony dostęp do portu TCP 1723. Jeżeli serwer działa na laptopie za bramą (Router B), trzeba zapewnić przekierowanie portów (port fowarding).
- Z poziomu Windowsa CE przechodzimy do ControlPanel:
- START > Control Panel > CX Configuration > RAS Control
- Enablujemy RAS Servwer
- Włączamy połączenie RAS VPN Line 0 (Enable Line)

![vpn8](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn8.png "vpn8")

- Zamykamy okno. Przechodzimy do ustawień sieci: Network And Dial-Up Connections
- Dodajemy nowe połączenie PPTP

![vpn9](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn9.png "vpn9")

- Podajemy ZEWNĘTRZNY adres IP do naszego komputera z VPN Serwerem. Można go sprawdzić np. za pomocą strony [moje IP](https://www.myip.cz)

![vpn10](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn10.png "vpn10")

- W zakładce Secutity Settings włączamy szyfrowanie danych i autentykację logowania:

![vpn11](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn11.png "vpn11")

![vpn12](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn12.png "vpn12")

- Klikamy Conect
- Podajemy usera i hasło zdefiniowane w VPN Serwerze
- Pierwsza próba połączenia może trwać trochę dłużej

![vpn13](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn13.png "vpn13")

- Po wdzwonieniu się, na serwerze również widnieje status połączenia:

![vpn14](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn14.png "vpn14")

![vpn15](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn15.png "vpn15")

- Stworzyliśmy VPN.
- Można odczytać adres VPN clienta (np. cmd > ipconfig /all):

![vpn16](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn16.png "vpn16")

- Oraz adres serwera

![vpn17](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn17.png "vpn17")

- Aby wyszukać sterownik za pomocą TwinCAT’a trzeba podać ten adres:

![vpn18](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn18.png "vpn18")

- Dalsze oprogramowywanie sterownika jest analogiczne jak jednostki będącej w tej samej podsieci.

# Ustawienia statycznego IP

![vpn19](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn19.png "vpn19")

![vpn20](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn20.png "vpn20")

# Szyfrowanie autoryzacji połączenia

![vpn21](https://ba-pl.github.io/wiki/assets/images/vpnCE/vpn21.png "vpn21")

Domyślnie połączenie szyfrowane jest za pomocą MS CHAP 2. Ustawienia zaawansowane są dostępne z poziomu rejestrów (opis w linkach).

# Autonawiązywanie połączenia
Autonawiązywanie połączenia możliwe jest do skonfigurowania z poziomu StartUp Managera (opisane w poniższych linkach).

# Pomocne linki
- [RAS Server](https://infosys.beckhoff.com/content/1033/sw_os/2018453387.html?id=1878917756769304675)
- [StartUp Manager](https://infosys.beckhoff.com/english.php?content=../content/1033/sw_os/7139932171.html&id=2917787884096561411)
- [MDP RAS status](https://infosys.beckhoff.com/content/1033/devicemanager/36028797281982731.html?id=779494138443119193) 