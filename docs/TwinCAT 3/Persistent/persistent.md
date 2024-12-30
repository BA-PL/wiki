---
title: Zmienne PERSISTENT 
parent: Zmienne nieulotne 
nav_order: 1
layout: page
---


# Zmienne PERSISTENT 
{: .no_toc }
<h6> Data modyfikacji: 30.12.2024 </h6>
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Zmienne typu Persistent używane są w momencie w którym musimy zapamiętać istotne dla nas wartości zmiennych. W zależności od aplikacji będą to różne zmienne, jednak najczęściej zmienne Persistent stosuje się dla wartości takich jak:
- Ilości wykonanych sztuk czy sumaryczny czas pracy – dla maszyn produkcyjnych
- Nastawy regulatorów PID – dla aplikacji, które wymagają takich regulacji
- Stany i wartości natężenia świateł – w aplikacjach budynkowych
- Inne zmienne – zależnie od aplikacji

Należy pamiętać o tym, że zmienne Persistent zapisywane są na nośniku pamięci, a więc mamy możliwość przeniesienia ich między maszynami. W niektórych przypadkach bardzo może to ułatwić akcje serwisowe (np. w przypadku konieczności wymiany sterownika na maszynie wystarczy że przeniesiemy plik z zapisanymi zmiennymi Persistent i będą miały one w projekcie wartości identyczne jak na „starym” sterowniku).
<br>
Zmienne te zapisują się w momencie zadziałania któregoś z wcześniej wymienionych bloków – a więc albo w momencie żądania zapisu po otrzymaniu zewnętrznego sygnału , albo w momencie wykrycia zaniku zasilania i zadziałania bloku do obsługi 1-second UPS (o ile wybrany tryb działania 1-second UPS to przewiduje). Zostaną zapisane również w momencie zadziałania UPS w zasilaczu CX2100 jeżeli zostanie to odpowiednio skonfigurowane w oprogramowaniu na sterowniku.

# Deklaracja zmiennych PERSISTENT
Zmienne Persistent deklaruje się bezpośrednio poprzez umieszczenie ich na liście VAR PERSISTENT. Taka deklaracja jest dla TwinCAT znakiem, że zmienną należy traktować jako zmienną nieulotną. Zmienne Persistent mogą być deklarowane zarówno jako zmienne globalne, jak i zmienne lokalne, a więc poprawny jest zarówno zapis

![pers1](https://ba-pl.github.io/wiki/assets/images/pers1.png "pers1")

jak ich

![pers2](https://ba-pl.github.io/wiki/assets/images/pers2.png "pers2")

**UWAGA!!! W przypadku deklarowania jako Persistent wartości będących przypisaniem z innych obiektów należy pamiętać o tym, aby jako Persistent zadeklarowany był cały obiekt, a nie tylko zmienna, do której przepisywana jest wartość!**
<br>
<br>
W przypadku deklarowania jako Persistent zmiennej w bloku funkcyjnym musimy pamiętać, że każde wywołanie tego bloku funkcyjnego w kodzie spowoduje, że zapisywać się będzie kolejna „instacja” tej zmiennej na nośniku pamięci.
<br>
W przypadku korzystania ze sterowników z 1-second UPS co do zasady przyjmuje się, aby wielkość zmiennych Persistent w projekcie nie przekraczała 1 MB, ponieważ wraz ze zużyciem kondensatora używanego do podtrzymania w 1-second UPS większa ilość danych może nie zostać zapisana.
<br>
W przypadku integracji sterownika z zewnętrznym UPS ograniczenie co do ilości możliwych danych typu Persistent do zapisu wynika z pojemności UPS, a więc jesteśmy w stanie zapisać tyle danych Persistent, na ile pozwoli nam długość podtrzymania bateryjnego z UPS.
<br>
Ilość zapisywanych zmiennych Persistent możemy kontrolować przez sprawdzanie wielkości plików Port_xxx.bootdata oraz Port_xxx.bootdata-old znajdujących się w lokalizacji **TwinCAT/3.1/Boot/Plc**

![pers3](https://ba-pl.github.io/wiki/assets/images/pers3.png "pers3")

Plik \*.bootdata to aktualnie zapisany plik, który znika w momencie przejścia TwinCAT na urządzeniu w tryb Run, a tworzy się w momencie przejścia w tryb Config, natomiast plik \*.bootdata-old jest to poprzednio zapisany plik \*.bootdata. W momencie przejścia w tryb Run zawartość pliku \*.bootdata przepisywany jest do \*.bootdata-old.


