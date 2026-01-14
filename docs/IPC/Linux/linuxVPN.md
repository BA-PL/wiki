---
title: Tailscale VPN
parent: Beckhoff RT Linux® 
nav_order: 3
layout: page
---

# Tailscale VPN na Linux  
{: .no_toc }
<h6> Data modyfikacji: 14.01.2026 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Wstep
Jako alternatywę dla klasycznych systemów VPN w środowiskach Linux (w tym na sterownikach PLC ARM64) można zastosować rozwiązanie Tailscale. Umożliwia ono bezpieczny zdalny dostęp do urządzeń bez konieczności konfiguracji port forwardingu lub posiadania publicznego adresu IP, a zarządzanie połączeniami i dostępami realizowane jest centralnie poprzez panel administracyjny.

# Zalety Tailscale
Zalety w porównaniu do klasycznego systemu VPN przedstawiają się następująco:
- **Brak wymogu port forwardingu na routerze i posiadania publicznego IP** - Tailscale nie wymaga wystawiania żadnych portów na routerze ani posiadania publicznego adresu IP po stronie klienta lub sieci docelowej. Połączenia inicjowane są wychodząco, dzięki czemu rozwiązanie działa poprawnie również w sieciach za NAT, CGNAT oraz w środowiskach z restrykcyjnymi zaporami sieciowymi.
- **Optymalny routing** - Tailscale zawsze próbuje zestawić bezpośrednie, end-to-end szyfrowane połączenie pomiędzy klientem a siecią/hostem docelowym. W przypadku gdy nie jest możliwe połączenie bezpośrednie, pakiety przekazywane są przez pośrednie, szyfrowane serwery Tailscale (tzw. "Relay"),
- **Subnet routing** - Tailscale pozwala na udostępnienie całych podsieci IP za pomocą tzw. Subnet Routera. Dzięki temu możliwy jest dostęp do urządzeń, które nie obsługują instalacji klienta VPN (np. sterowniki PLC, panele HMI, kamery IP), bez konieczności ingerencji w ich konfigurację sieciową lub zmian w istniejącej architekturze OT,
- **Zasada Zero-Trust** - Dodawanie nowych urządzeń do sieci Tailscale oraz zmiany parametrów routingu (np. gateway, subnet router) wymagają jawnego zatwierdzenia w panelu administratora. Każde urządzenie jest jednoznacznie identyfikowane i uwierzytelniane, a dostęp nie jest przyznawany domyślnie na poziomie całej sieci,
- **ACL i kontrola dostępu** - Tailscale pozwala na precyzyjne określanie kto, z jakich kont, ma jakie uprawnienia w sieci Tailscale,
- **Automatyczne przełączanie połączeń** - Tailscale automatycznie rekonfiguruje trasę pakietów przy zmianie sieci. Pozwala to zarówno na szybkie połączenie w przypadku zmiany sieci na laptopie, ale również pozwala na szybkie przełączenie sterownika PLC w inną sieć z zachowaniem pełnej funkcjonalności,
- **Diagnostyka** - Tailscale udostępnia rozbudowane narzędzia diagnostyczne zarówno z poziomu panelu administratora, jak i wiersza poleceń. Umożliwia to sprawdzanie, czy połączenie jest bezpośrednie czy realizowane przez relay, analizę opóźnień w sieci oraz monitorowanie aktualnego statusu urządzeń, co znacząco ułatwia troubleshooting.

# Cennik
Tailscale to oprogramowanie płatne na **użytek komercyjny**.

Model licencjonowania opiera się na:
- liczbie użytkowników,
- liczbie urządzeń przypisanych do kont,
- oraz dostępnych funkcjach (np. ACL, subnet routing, integracje z systemami SSO).

Przykładowe plany:

![1](https://ba-pl.github.io/wiki/assets/images/tail/1.png "1")

Starter:
- 6 USD za każdego użytkownika (adres e-mail) w sieci,
- maksymalnie 100 urządzeń w sieci + dodatkowe 10 urządzeń na każdego użytkownika (istnieje możliwość zwiększenia limitu urządzeń za dodatkową opłatą miesięczną),
- Split tunneling – ruch do zasobów spoza sieci Tailscale nie jest tunelowany przez VPN,
- Czytelne nazwy DNS dla urządzeń i usług (human-readable DNS names) dzięki funkcji MagicDNS,
- Polityki dostępu na poziomie sieci (ACL) – ograniczona funkcjonalność w porównaniu do wyższych planów.

Premium:
- 18 USD za każdego użytkownika (adres e-mail) w sieci,
- maksymalnie 100 urządzeń w sieci + dodatkowe 20 urządzeń na każdego użytkownika,
- Automatyczne zarządzanie kluczami oraz bezpieczny dostęp administracyjny poprzez Tailscale SSH,
- Zaawansowane mechanizmy udostępniania usług poprzez Tailscale Funnel (publikacja usług z kontrolą dostępu),
- Polityki dostępu zależne od tożsamości użytkownika (identity-aware access policies),
- Polityki MDM (Mobile Device Management) umożliwiające wymuszanie konfiguracji i zgodności urządzeń,
- Audyt konfiguracji oraz logowanie przepływów sieciowych, ułatwiające analizę bezpieczeństwa i diagnostykę.

# Instalacja i uruchomienie na Linuxie

Poniższa instalacja i konfiguracja zawiera podstawowe połączenie ze sterownikiem przez Tailscale - po jej wykonaniu, możliwe będzie połączenie się ze sterownikiem po ADS celem wgrania programu, dostępem do terminala przez ssh itp.

![2](https://ba-pl.github.io/wiki/assets/images/tail/2.png "2")

- Pobieramy i instalujemy za pomocą połączenia metody `curl` i `sh` program tailscale:

 ```curl -fsSL https://tailscale.com/install.sh | sh```
 
![3](https://ba-pl.github.io/wiki/assets/images/tail/3.png "3")

- Uruchamiamy aplikację tailscale - po wpisaniu poniższej komendy, tailscale zostaje także dodany do autouruchamiania się w systemie:

```sudo tailscale up```

![4](https://ba-pl.github.io/wiki/assets/images/tail/4.png "4")

Po wpisaniu komendy wygenerowywany jest unikatowy link do zarejestrowania urządzenia na naszym koncie - otwieramy go na dowolnym urządzeniu, na którym jesteśmy zalogowani na nasze konto tailscale i akceptujemy dodanie urządzenia do naszej sieci VPN.

![5](https://ba-pl.github.io/wiki/assets/images/tail/5.png "5")

Tailscale posiada system użytkowników - sterownika PLC nie trzeba podpinać na ten sam adres email, który stanowi nasz główny adres mailowy. Poprzez zakładkę login.tailscale.com/admin/users zapraszać można konta z innymi adresami mailowymi do naszej sieci Tailscale i nadawać im uprawnienia. Pozwala to na większą swobodę w definiowaniu, jak połączone są nasze sterowniki PLC z dostępem zdalnym i co są w stanie robić "między sobą".

![6](https://ba-pl.github.io/wiki/assets/images/tail/6.png "6")

Po wejściu w link i zaakceptowaniu urządzenia do naszej sieci Tailscale, podstawowa konfiguracja została zakończona - sterownik powinien być widoczny w sieci Tailscale, powinna być również możliwość łączenia się z nim poprzez np. ssh. Dalsze kroki konfiguracji należy dostosować do swojej architektury systemu i tego, jak wyglądają interakcje pomiędzy urządzeniami w sieci Tailscale. 

- Status usługi tailscale w systemie Linux możemy sprawdzić poprzez komendę:

``` systemctl status tailscaled.service```

![7](https://ba-pl.github.io/wiki/assets/images/tail/7.png "7")

IP urządzeń podpiętych do naszej sieci Tailscale możemy sprawdzić poprzez stronę www:

![8](https://ba-pl.github.io/wiki/assets/images/tail/8.png "8")

lub poprzez komendę bezpośrednio na sterowniku z Linuxem:

``` tailscale status```

![9](https://ba-pl.github.io/wiki/assets/images/tail/9.png "9")

# Konfiguracja PLC jako gateway VPN do innych urzadzen w sieci

![10](https://ba-pl.github.io/wiki/assets/images/tail/10.png "10")

Tailscale posiada funkcjonalność `advertise routes`, która pozwala nam na dostęp zdalny do innych sterowników w danej sieci lokalnej, gdzie jeden z nich tworzy nam gateway "na świat".
Przykładowo, mamy sterownik CX9240, do którego podpinamy na port X000 kabel z wyjściem "na świat", a X001 mamy podpięte do wydzielonej sieci lokalnej, w której znajdują się inne sterowniki. Odpowiednia konfiguracja Tailscale pozwoli nam na łączenie się z pozostałymi sterownikami poprzez następujące kroki:

- Tworzymy plik w folderze /etc/sysctl.d który zezwala na przekazywanie pakietów dalej przez sterownik.  Przechodzimy do folderu:

```
cd /etc/sysctl.d
```
 
Tworzymy nowy plik i otwieramy przez edytor nano:

```
sudo nano 90-ipv4-forward.conf
```

Następnie, wpisujemy w środku pliku

```
net.ipv4.ip_forward=1
```

zapisujemy Ctrl+S, wychodzimy Ctrl+X.

Następnie wczytujemy nowo dodany parametr poprzez komendę:

```
sudo systemctl restart systemd-sysctl
```

- Na linuxie wpisujemy komendę do uruchamiania tailscale wraz z pulą adresów IP które będą dostępne przez ten sterownik. Jako przykład, drugi port CX9240 ma IP 192.168.50.15 oraz chce stanowić gateway dla urządzeń o puli IP 192.168.50.XXX

```
sudo tailscale up --advertise-routes=192.168.50.0/24
```

![11](https://ba-pl.github.io/wiki/assets/images/tail/11.png "11")

Wyskoczył warning o braku forwardowania IPv6, ponieważ wyżej ustawiliśmy jedynie forwardowanie IPv4. Najważniejsze, aby nie było warningu ```warning: ip forwarding is disabled subnet routing/exit nodes will not work```

- W panelu admina Tailscale przy sterowniku pojawi się informacja, aby zatwierdzić przekazywanie pakietów przez sterownik PLC:

![12](https://ba-pl.github.io/wiki/assets/images/tail/12.png "12")

Z prawej strony przechodzimy do opcji Edit route settings i akceptujemy:

![13](https://ba-pl.github.io/wiki/assets/images/tail/13.png "13")

- Tworzymy reguły firewalla, aby poprawnie przekazywać pakiety:

```
sudo nano /etc/nftables.conf.d/90-tailscale.rules
```

A następnie wypełniamy odpowiednio plik:

```
table ip nat {
    chain postrouting {
        type nat hook postrouting priority 100;
        oif "end1" masquerade
    }
}

table inet filter {
    chain forward {
        iif "tailscale0" oif "end1" accept
        iif "end1" oif "tailscale0" accept
    }
}
```

![14](https://ba-pl.github.io/wiki/assets/images/tail/14.png "14")

> **Uwaga**
> 
> Przy przedstawionej konfiguracji pliku, sterownik z zainstalowanym Tailscale **nie stanowi zapory sieciowej** dla reszty urządzeń w sieci lokalnej. Oznacza to, że ruch przechodzący przez sterownik (z laptopa przez Tailscale do sieci LAN) jest przekazywany zgodnie z regułami NAT i forwardingu, a dostęp do konkretnych usług na urządzeniach w sieci LAN zależy od ustawień firewalli tych urządzeń. Można to porównać do wpięcia naszego urządzenia bezpośrednio w sieć lokalną za pomocą kabla ethernet.

zapisujemy Ctrl+S, wychodzimy Ctrl+X.
Przeładowujemy firewalla poprzez komendę:

```
sudo systemctl reload nftables
```

Następnie możemy testować na własnym laptopie, czy widzimy inne urządzenia w sieci poprzez transfer pakietów przez sterownik z Linuxem.

# Przydatne tipy i komendy

Na stronie https://tailscale.com/kb/1080/cli znajduje się opis komend, jakie można wpisywać, aby wchodzić w interakcje z naszą usługą Tailscale.

- Domyślnie, dostęp do sieci Tailscale dodawany jest do danego urządzenia na okres 6 miesięcy w celach bezpieczeństwa. Jeśli chcemy mieć dostęp do sterownika PLC nawet po dodaniu go bardzo dawno temu, to warto znieść ten wymóg poprzez opcję `Disable key expiry`.

![15](https://ba-pl.github.io/wiki/assets/images/tail/15.png "15")




