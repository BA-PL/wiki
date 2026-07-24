---
title: Połączenie i instalacja TwinCAT 
parent: Beckhoff RT Linux® 
nav_order: 1
layout: page
---

# Beckhoff RT Linux® - instalacja  
{: .no_toc }
<h6> Data modyfikacji: 27.11.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Instalacja systemu na sterowniku (jeśli nie jest zainstalowany fabrycznie)

Instalacja w przystępny sposób opisana jest w [Infosys.](https://infosys.beckhoff.com/english.php?content=../content/1033/beckhoff_rt_linux/19984093195.html) 

# Podłączenie i instalacja TwinCAT

Sterownik można skonfigurować lokalnie, korzystając z podłączonego monitora i klawiatury lub za pomocą ssh (Power Shell), zdalnie. Do połączenia ssh należy najpierw uzyskać adres IP. Różne metody są opisane [tutaj.](https://infosys.beckhoff.com/english.php?content=../content/1033/beckhoff_rt_linux/17350407691.html)
<br>

Dlasza część instrukcji będzie opierać się na konfiguracji sterownika z poziomu Power Shell i połączenia ssh.
Po uzyskaniu adresu IP otiweramy Power Shell jako administrator i wpisujemy komendę:

```
ssh Administrator@Adres_IP
```

![linux1](https://ba-pl.github.io/wiki/assets/images/linux/linux1.png "linux1")

Pierwszym krok po uruchomieniu systemu i połączeniu który wykonujemy raz, jest skonfigurowanie dostępu do APT (Advanced Package Tool), który jest systemem zarządzania pakietami i znajdziemy w nim m.in. instalator TwinCATa.
<br>
Wpisujemy komendę:

```
sudo nano /etc/apt/auth.conf.d/bhf.conf
```

Otworzy się pusty plik, który należy uzupełnić swoimi danymi logowania do konta myBeckhoff (strona www Beckhoff). 

```
machine deb.beckhoff.com
login example@mail.com
password xyz123

machine deb-mirror.beckhoff.com
login example@mail.com
password xyz123
```

![linux2](https://ba-pl.github.io/wiki/assets/images/linux/linux2.png "linux2")

Po wprowadzeniu zmian zapisujemy je skrótem **Ctrl+O**, zatwierdzamy klawiszem **Enter**, a następnie zamykamy edytor skrótem **Ctrl+X**.
<br>

Kolejną komendą jest update zasobów APT, robimy to komendą:

```
sudo apt update
```

**UWAGA!!**
**Aby komenda wykonała się poprawnie, na sterowniku wymagane jest połączenie z internetem.**
**Jeśli nie możesz wpiąć sterownika bezpośrednio do sieci, przejdź** [tutaj](https://ba-pl.github.io/wiki/docs/IPC/Linux/linuxBase.html#dodatek---brak-dost%C4%99pu-do-internetu-na-sterowniku). 
 

![linux3](https://ba-pl.github.io/wiki/assets/images/linux/linux3.png "linux3")

Aby zainstalować **TwinCATa**, wpisujemy komendę:

```
sudo apt install tc31-xar-um
```

![linux4](https://ba-pl.github.io/wiki/assets/images/linux/linux4.png "linux4")

Jeśli instalacja wymaga restartu, wykonujemy go komendą:

```
sudo reboot
```

Po tych czynnościach dioda TC na sterowniku powinna się zaświecić (na niebiesko).

## Dodatek - brak dostępu do Internetu na sterowniku

Jeśli nie ma możliwości wpięcia sterownika bezpośrednio do sieci, można udostępnić sieć ze swojego komputera.
<br>
<br>
Założenia:
- laptop z dostępem do Internetu po wifi
- sterownik podpięty kablem do laptopa 1:1 

<br>
Wchodzimy w ustawienia kart sieciowych i na karcie wifi wchodzimy w właściwości:

![i1](https://ba-pl.github.io/wiki/assets/images/linux/i1.png "i1")

Następnie w zakładce udostępniania aktywujemy udostępnianie i wybieramy z listy kartę sieciową na której jest podłączony sterownik.

![i2](https://ba-pl.github.io/wiki/assets/images/linux/i2.png "i2")

Po chwili (do ok. minuty) na karcie siecowej sterownika powienien pojawiać się adres IP z puli 192.168.x.x:

![i3](https://ba-pl.github.io/wiki/assets/images/linux/i3.png "i3")

Sterownik ma teraz dostęp do sieci poprzez laptop. 