---
title: Ustawienia, procesy, logi 
parent: Beckhoff RT Linux® 
nav_order: 3
layout: page
---

# Ustawienia, procesy, logi - TwinCAT Linux
{: .no_toc }
<h6> Data modyfikacji: 30.01.2026 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Przełączanie stanu Config / Run 

Główny proces TwinCATa w Linux to **TcSystemServiceUm**.

Po wpisaniu jego nazwy dostaniemy listę możliwych operacji.

![u1](https://ba-pl.github.io/wiki/assets/images/linux/u1.png "u1")

Jak widać, np. jeśli chcemy przejść do trybu Config, musimy podać **pidfile**.

Znajdziemy go na liście procesów po wpisaniu komendy **ps -aux**:

![u2](https://ba-pl.github.io/wiki/assets/images/linux/u2.png "u2")

Cała komeda do przełączenia w tryb **Config** będzie zatem wyglądała tak:

```
sudo TcSystemServiceUm -c /var/run/TcSystemServiceUm.pid
```

do trybu **Run**:

```
sudo TcSystemServiceUm -r /var/run/TcSystemServiceUm.pid
```

# Zmiana AMS Net ID

Ustawienia adresu AMS znajdują się pliku TcRegistry.xml pod ścieżką **/etc/TwinCAT/3.1**:

![u3](https://ba-pl.github.io/wiki/assets/images/linux/u3.png "u3")

Edytujemy go komendą:

```
sudo nano /etc/TwinCAT/3.1/TcRegistry.xml
```

Wartość AMS wpisujemy w hex w zaznaczonym miejscu i zapisujemy zmiany w pliku przy zamykaniu (Ctrl+X):

![u4](https://ba-pl.github.io/wiki/assets/images/linux/u4.png "u4")

Następnie należy przeładować proces TwinCATa (do zarządzania procesami używamy **systemctl**):

```
sudo systemctl restart TcSystemServiceUm
```

# Zmiana hostname

Informacja o hostname znajduje się pliku hostname w katalogu **/etc**.

Edytujemy go komendą:

```
sudo nano /etc/hostname 
```

Po zmianie robimy restart:

```
sudo reboot
```

# Czas systemowy

Aktualny czas systemowy można sprawdzić komedą:


```
timedatectl
```

![u5](https://ba-pl.github.io/wiki/assets/images/linux/u5.png "u5")

Domyślna strefa czasowa to UTC.

Aby zmienić strefę czasową na UTC+1 można wpisać komendę:

```
sudo timedatectl set-timezone Europe/Warsaw 
```

Po wpisaniu i ponownym sprawdzeniu:

![u6](https://ba-pl.github.io/wiki/assets/images/linux/u6.png "u6")

Za synchronizację czasu (NTP) odpowiada usługa **systemd-timesyncd**.

Można sprawdzić jej status:

```
sudo systemctl status systemd-timesyncd
```

![u7](https://ba-pl.github.io/wiki/assets/images/linux/u7.png "u7")

# Logi

Za obsługę dziennka zdarzeń odpowiada **journalctl**. Po wpisaniu tej komendy dostaniemy nieodfiltrowaną listę logów:

![u8](https://ba-pl.github.io/wiki/assets/images/linux/u8.png "u8")

Nas najczęściej będą interesować zdarzenia związane z TwinCAT, możemy je odfiltrować:

```
sudo journalctl | grep TcSystemServiceUm
```

![u9](https://ba-pl.github.io/wiki/assets/images/linux/u9.png "u9")

## Logi aplikacji

Logi od poszczególnych aplikacji znajdziemy najczęściej w katalogu **/var/log**.

Przykładowo, jeśli są załączone logi dla dodatku OPC-UA, to:

- przechodzimy do lokalizacji

```
cd /var/log/TF6100-OPC-UA
```

- wyświetlamy listę plików komedą ls

- wyświetlamy zawartość wybranego pliku:

```
cat srvTrace.log
```

![u10](https://ba-pl.github.io/wiki/assets/images/linux/u10.png "u10")

# Sprawdzenie wersji systemu

Aby sprawdzić główną wersję systemu, można użyć komendy:

```
cat /etc/debian_version
```

![u11](https://ba-pl.github.io/wiki/assets/images/linux/u11.png "u11")

Jeśli chcemy porównać swój build z buildem ze strony Beckhoff, użyjemy komendy:

```
cat /etc/os-release.d/* 2>/dev/null
```

![u12](https://ba-pl.github.io/wiki/assets/images/linux/u12.png "u12")
