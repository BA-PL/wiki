---
title: Instalacja dodtaków TF (TF1810)
parent: Beckhoff RT Linux® 
nav_order: 2
layout: page
---

# Instalacja dodatków TF (na przykładzie TF1810)
{: .no_toc }
<h6> Data modyfikacji: 27.11.2025 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Odszuakanie nazwy i instalacja dodatku

Jeśli mamy już skonfigurowany dostęp do APT - [patrz tutaj](https://ba-pl.github.io/wiki/docs/IPC/Linux/linuxBase/#pod%C5%82%C4%85czenie-i-instalacja-twincat) - zacznie od komendy update'u:

```
sudo apt update
```

Następnie wyszukujemy pełną nazwę interesującego nas dodatku, można wpisać np:

```
sudo apt search TF18
```

![linux5](https://ba-pl.github.io/wiki/assets/images/linux/linux5.png "linux5")

Nazwy można teraz użyć do komendy **install**:

```
sudo apt install tf1810-plc-hmi-web
```

![linux6](https://ba-pl.github.io/wiki/assets/images/linux/linux6.png "linux6")

Po instalacji dodatku konfigurujemy firewall (specyficznie dla TF1810). Wpisujemy komendę:

```
sudo nano /etc/nftables-bhf.conf
```

I dodajemy wpis na końcu pliku:

```
table inet filter {
  chain input {
    # Allow HTTP
    tcp dport 42341 accept
  }
}
```

![linux7](https://ba-pl.github.io/wiki/assets/images/linux/linux7.png "linux7")

Po wprowadzeniu zmian zapisujemy je skrótem **Ctrl+O**, zatwierdzamy klawiszem **Enter**, a następnie zamykamy edytor skrótem **Ctrl+X**.
<br>
Wykonujemy restart:

```
sudo reboot
```

## Instalacja przeglądarki do lokalnego wyświetlania wizualizacji TF1810 

Po restarcie i ponownym połączeniu ssh, instalujemy klienta TF1200 (przeglądarka do lokalnego wyświetlania wizualizacji):

```
sudo apt install tf1200-ui-client
```

![linux8](https://ba-pl.github.io/wiki/assets/images/linux/linux8.png "linux8")

Następnie dodajemy użytkownika dla automatycznego startu wizualizacji w systemie (powinien być inny niż Administrator) komendą:

```
sudo /etc/TwinCAT/Functions/TF1200-UI-Client/scripts/setup-full.sh --user=TF1200 --autologin –autostart 
```

![linux9](https://ba-pl.github.io/wiki/assets/images/linux/linux9.png "linux9")

Wykonujemy restart:

```
sudo reboot
```

Na tym etapie, jeśli mamy projekt TwinCAT z wizualizacją i dodanym elementem WebVisu (zdjęcie poniżej):

![linux10](https://ba-pl.github.io/wiki/assets/images/linux/linux10.png "linux10")

możemy wgrać ten projekt aktywując konfigurację (pamiętając o opcji Autostart Bootprojet oraz trybie uruchamiania TwinCAT), oraz aktywujmey licencję (do testów może być to licencja trail).
<br>
Po wykoananiu wcześniejszych kroków i ponownym połączeniu ssh należy edytować plik z ustawienia TF1200, podając TF1810 jako stronę startową. Wpisujemy komendę:

```
sudo nano /home/TF1200/.config/TF1200-UI-Client/config.json
```

Otworzy się plik w którym edytujemy parametr start url:

![linux11](https://ba-pl.github.io/wiki/assets/images/linux/linux11.png "linux11")
 
```
"startUrl": "http://127.0.0.1:42341/Tc3PlcHmiWeb/Port_851/Visu/webvisu.htm"
```
 
Nazwa pliku .htm powinna być zgodna z nazwą w projekcie:
 
![linux12](https://ba-pl.github.io/wiki/assets/images/linux/linux12.png "linux12")
  
Po restarcie sterownika wizualizacja powinna się wyświetlić automatycznie na podłączonym do niego ekranie.