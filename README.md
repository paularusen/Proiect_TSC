  Proiectul este centrat pe microcontrolerul ```ESP32-C6```, care, datorită versatilității sale, suportă comunicații Wi‑Fi 6, 
Bluetooth și IEEE 802.15.4, alături de multiple interfețe periferice. Designul integrează un e‑paper display, un modul SD 
card, memorie flash externă, un modul RTC și interfețe de comunicație (```USB```, ```UART```, ```I2C```). Fiecare interfață este atribuită 
unor pini specifici pentru a asigura o separare clară a funcțiilor, evitând conflictele și maximizând performanța și
eficiența energetică.


## Descrierea componentelor și a pinilor utilizați

### ESP32-C6 – Microcontrolerul Central
Rol: Coordonează toate operațiunile sistemului și gestionează comunicațiile cu toate modulele periferice.
Pini atribuiți:

	- GND: 1 si 28
	- 3V3: 2
	- RESET: 3
	- SS_SD: 4
	- EPD_DC: 5
	- SCK: 6
	- MOSI: 7
	- INT_RTC: 8
	- 32KHZ: 9
	- GPIO8: 10
	- EPD_CS: 11
	- FLASH_CS: 12
	- USB_D-: 13
	- USB_D+: 14
	- IO/BOOT: 15
	- RTC_RST: 16
	- I2C_PW: 17
	- EPD_3V3_C: 18
	- SDA: 19
	- SCL: 20
	- EPD_RST: 21
	- IO/CHANGE: 23
	- RX: 24
	- TX: 25
	- EPD_BUSY: 26
	- MISO: 27

Rolul fiecărui pin va fi detaliat în cadrul componentelor, în următoarele rânduri.

### E‑Paper Display
Rol: Afișare informații într-un mod foarte eficient energetic, potrivit pentru aplicații de tip IoT sau afișaje statice.
Pini atribuiți:

	- EPD_DC: Selectează între modurile de comandă și de date pentru e-paper.
	- EPD_CS: Chip Select – activează comunicația cu e‑paper-ul.
	- EPD_RST: Linie de reset, esențială la inițializare pentru afișaj.
	- EPD_BUSY: Semnal de status ce indică dacă afișajul este ocupat (prevenind suprascrierea datelor).
	- EPD_3V3_C: Linia de alimentare dedicată, asigurând o tensiune stabilă și izolată pentru e‑paper.

### Modul SD Card
Rol: Stocare de date suplimentare (fișiere de sistem, date utilizator etc.)
Pini atribuiți:

	- SS_SD: Chip select specific pentru SD Card, activând modulul numai atunci când este necesar.
	- SCK: Linia de ceas pentru comunicațiile SPI.
	- MOSI: Linia de ieșire de date dinspre ESP32 către SD Card.
	- MISO: Linia de intrare de date, pentru citirea informațiilor de la SD Card.

### Memorie Flash Externă
Rol: Stocarea firmware-ului sau a datelor critice ce necesită o capacitate suplimentară față de memoria internă.
Pini atribuiți:

	- FLASH_CS: Chip select pentru memorie flash, pentru a activa comunicația doar atunci când este necesar.

### Modul RTC (Real-Time Clock)
Rol: Asigură o cronometrare precisă, utilă pentru funcții de alarmă, logare a datelor sau sincronizare de timp în 
aplicații IoT.
Pini atribuiți:

	- INT_RTC: Întrerupere de la RTC, notificând microcontrolerul despre evenimente (ex. alarmă).
	- RTC_RST: Linia de reset pentru modulul RTC, asigurând reinițializarea corectă a ceasului.
	- 32KHZ: Furnizează un semnal de 32 kHz, esențial pentru funcționarea precisă a RTC-ului.

### Senzorul de mediu (ex.: BME688):
Rol: Măsoară parametri esențiali precum temperatura, umiditatea, presiunea atmosferică și compoziția gazelor din mediul 
înconjurător.
Comunicarea se realizează prin magistrala I2C, utilizând SDA(linia de date I2C), SCL(linia de ceas I2C) și I2C_PW.

### Interfața I2C
Rol: Comunicare cu senzori, module de monitorizare a bateriei, eventual RTC suplimentar sau alte periferice cu consum 
redus.
Pini atribuiți:

	- SDA: Linia de date pentru comunicația I2C.
	- SCL: Linia de ceas pentru I2C.
	- I2C_PW: Poate fi folosit pentru a activa sau alimenta circuitele periferice I2C (ex. senzori sau monitor de baterie).

### Interfața USB
Rol: Programare, transfer de date sau alimentare în anumite condiții.
Pini atribuiți:

	- USB_D- și USB_D+: Linii diferențiale pentru comunicația USB.

### Interfața UART
Rol: Comunicații seriale pentru debugging, logare sau control extern.
Pini atribuiți:

	- RX: Recepție UART.
	- TX: Transmisie UART.

Pinii speciali precum IO/BOOT și IO/CHAGE permit configurarea modului de pornire și monitorizarea încărcării, iar GP0IO8 
și GOL asigură flexibilitatea pentru extinderi viitoare.
	IO/BOOT: utilizat la pornirea sistemului pentru selectarea modului de boot. Această funcție este esențială pentru a 
intra în modul de programare sau diagnosticare, conform cerințelor de firmware.
	IO/CHAGE: dedicat monitorizării stării de încărcare a sistemului. Acest pin poate fi legat la un circuit de detecție al 
încărcării bateriei sau indicator LED pentru a semnala starea de încărcare.
	GP0IO8: un GPIO general care poate fi folosit pentru extinderi ulterioare (ex. conectarea unui senzor adițional, trigger 
de eveniment etc.).
	Pin 22: funcționalitatea acestui pin poate fi folosită ca ieșire generală pentru semnale de control, cum ar fi activarea 
unor relee, indicatori LED suplimentari sau semnale de sincronizare pentru alte module.


## Consumul de Energie și comunicațiile

### Consumul microcontrolerului:
	- ESP32-C6 dispune de moduri de economisire a energiei (deep sleep) și este optimizat pentru aplicații IoT. Astfel, în 
condiții de standby consumul poate fi redus la câțiva microamperi.

### SPI și I2C:
	- Utilizarea magistralelor SPI (pentru SD Card, flash extern și e‑paper display) și I2C (pentru senzori și monitorizare 
baterie) permite un transfer de date rapid și eficient, dar și o separare logică care contribuie la reducerea 
interferențelor.

### USB și UART:
	- Aceste interfețe sunt folosite mai ales pentru debugging și programare. Deși USB poate implica un consum mai mare când 
este activ, acesta este activ doar în timpul programării sau transferurilor de date.

### Alimentare:
	- Pinul de 3V3) este utilizat pentru alimentarea ESP32-C6 și a modulelor critice, iar alocarea separată pentru EPD 
(EPD_3V3_C) asigură o tensiune stabilă pentru afișajul e-paper.

## Note
- În fișierul Diagrama_bloc.png se gășește o diagramă bloc cu toate componentele proiectului și cum sunt ele legate.
- În folderul Manufacturing se aflâ fișierul BOM al proiectului, Proiect_TSC_2025_NEW v41.csv, cu toate componentele 
folosite.

## Observații
- Am modificat footprint-ul componentei J2, prin mutarea pad-urilor GND, VUSB și B8 cu câțiva mm spre stânga, pentru a corecta eroarea ”SMD Hole”.
