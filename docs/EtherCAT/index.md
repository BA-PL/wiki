---
title: EtherCAT
nav_order: 3
layout: page
---

# EtherCAT
<h6> Data modyfikacji: 20.12.2024 </h6>

**Cechy podstawowe:**
<br>
- Sieć typu single-master (jedno urządzenie zarządzające siecią)
- Do 65535 urządzeń slave w jednej sieci
- Dowolna topologia (linia, drzewo, gwiazda, ring, topologia mieszana)
- Brak konieczności adresowania urządzeń
- Synchronizacja czasowa dzięki technologii Distributed Clocks o rodzielczości 1 ns

<br>
**Technologie EtherCAT:**
<br>
- Oversampling: technologia pozwalająca na odczytywanie danych w równych odstępach próbek zbieranych częściej niż cykl programu PLC
- XFC (eXtreme Fast Control): technologia pozwalająca na uzyskiwanie czasów cyklu rzędu dziesiątek mikrosekund dzięki szybkim przetwornikom
- EtherCAT P: technologia umożliwiająca przesyłanie sygnału sieci EtherCAT oraz zasilania urządzenia przy pomocy jednego przewodu

![EtherCAT](EtherCAT.png "EtherCAT")



---
