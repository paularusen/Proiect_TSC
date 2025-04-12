## Descrierea produsului

  Proiectul este centrat pe microcontrolerul ```ESP32-C6```, care, datorită versatilității sale, suportă comunicații Wi‑Fi 6, 
Bluetooth și IEEE 802.15.4, alături de multiple interfețe periferice. Designul integrează un e‑paper display, un modul SD 
card, memorie flash externă, un modul RTC și interfețe de comunicație (```USB```, ```UART```, ```I2C```). Fiecare interfață este atribuită 
unor pini specifici pentru a asigura o separare clară a funcțiilor, evitând conflictele și maximizând performanța și
eficiența energetică.

## Diagrama bloc

![Diagrama_bloc](https://github.com/user-attachments/assets/3096d9e9-f64c-494c-ba9d-e89c5e1f668f)




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

## Bill of Materials
| Componentă | Distribuitor | Datasheet |
| :---      | :---     | :---      |
| ESP32 C6| https://eu.mouser.com/ProductDetail/Espressif-Systems/ESP32-C6-WROOM-1U-N8?qs=1Kr7Jg1SGW%2FzPU4G%252ByMwkA%3D%3D | https://eu.mouser.com/datasheet/2/891/Espressif_Systems_7_11_2023_esp32_c6_wroom_1_wroom-3236277.pdf |
| BUTTON | https://www.digikey.ro/en/products/detail/bourns-inc/7914S-1-000E/2537971?s=N4IgTCBcDaIOwE4CMAWAygWiRgDHgoiALoC%2BQA | https://www.bourns.com/docs/Product-Datasheets/7914.pdf |
| USB-C connector| https://eu.mouser.com/ProductDetail/Same-Sky/UJC-H-G-SMT-2-P6-TR?qs=IKkN%2F947nfApFV8T6rOqww%3D%3D |https://eu.mouser.com/datasheet/2/1628/ujc_h_g_smt_2_p6_tr-3511211.pdf |
| USBLC6-2SC6Y | https://eu.mouser.com/ProductDetail/STMicroelectronics/USBLC6-2SC6Y?qs=gNDSiZmRJS%2FOgDexvXkdow%3D%3D | https://eu.mouser.com/datasheet/2/389/usblc6_2sc6y-1852505.pdf |
| DMG2305UX-7 | https://eu.mouser.com/ProductDetail/Diodes-Incorporated/DMG2305UX-7?qs=L1DZKBg7t5F%2FNBHrjfxC%252Bg%3D%3D | https://www.diodes.com/assets/Datasheets/DMG2305UX.pdf |
| XC6220A331MR-G | https://eu.mouser.com/ProductDetail/Torex-Semiconductor/XC6220A331MR-G?qs=AsjdqWjXhJ8ZSWznL1J0gg%3D%3D | https://eu.mouser.com/datasheet/2/760/xc6220-3371556.pdf |
| SD0805S020S1R0 | https://eu.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D | https://eu.mouser.com/datasheet/2/40/schottky-3165252.pdf |
| 112A-TAAR-R03 | https://store.comet.srl.ro/Catalogue/Product/43497/ | https://store.comet.bg/download-file.php?id=27596 |
| 68uH inductance | https://eu.mouser.com/ProductDetail/Vishay-Dale/IFDC5050JZER680M?qs=iLKYxzqNS745SePZd8tBoA%3D%3D | https://www.vishay.com/docs/34635/ifdc5050jz.pdf |
| SI1308EDL-T1-GE3 | https://eu.mouser.com/ProductDetail/Vishay-Semiconductors/SI1308EDL-T1-GE3?qs=bX1%252BNvsK%2FBramh9tgpOaEw%3D%3D | https://www.vishay.com/docs/63399/si1308edl.pdf |
| MBR0530 | https://eu.mouser.com/ProductDetail/Micro-Commercial-Components-MCC/MBR0530-TP?qs=KFo7JewZbUECRHkxGanrdg%3D%3D | https://eu.mouser.com/datasheet/2/258/MBR0520_MBR0580_SOD123_-2492194.pdf |
| MCP73831 | https://eu.mouser.com/ProductDetail/Microchip-Technology/MCP73831-2ACI-MC?qs=hH%252BOa0VZEiBneYTVdpuVdg%3D%3D | https://eu.mouser.com/datasheet/2/268/MCP73831_Family_Data_Sheet_DS20001984H-3441711.pdf |
| BME688 | https://eu.mouser.com/ProductDetail/Bosch-Sensortec/BME688?qs=IS%252B4QmGtzzqQoVDscqwx3A%3D%3D | https://eu.mouser.com/datasheet/2/783/bst_bme688_fl000-2307034.pdf |
| FH34SRJ-24S-0.5SH(99) | https://eu.mouser.com/ProductDetail/Hirose-Connector/FH34SRJ-24S-0.5SH99?qs=vcbW%252B4%252BSTIpKBl5ap9J8Fw%3D%3D | https://eu.mouser.com/datasheet/2/185/FH34SRJ_24S_0_5SH_99__CL0580_1255_6_99_2DDrawing_0-1615044.pdf |
| BD5229G-TR | https://eu.mouser.com/ProductDetail/ROHM-Semiconductor/BD5229G-TR?qs=4kLU8WoGk0vvnhrrYwdszw%3D%3D | https://fscdn.rohm.com/en/products/databook/datasheet/ic/power/voltage_detector/bd52xxg-e.pdf |
| MAX17048G+T10 | https://eu.mouser.com/ProductDetail/Analog-Devices-Maxim-Integrated/MAX17048G%2bT10?qs=D7PJwyCwLAoGnnn8jEPRBQ%3D%3D | https://eu.mouser.com/datasheet/2/609/MAX17048_MAX17049-3469099.pdf |
| DS3231SN# | https://eu.mouser.com/ProductDetail/Analog-Devices-Maxim-Integrated/DS3231SN?qs=1eQvB6Dk1vhUlr8%2FOrV0Fw%3D%3D | https://eu.mouser.com/datasheet/2/609/DS3231-3421123.pdf |
| CPH3225A | https://eu.mouser.com/ProductDetail/Seiko-Semiconductors/CPH3225A?qs=3etwrb1wR%252BhUOph6lAO7eg%3D%3D | https://eu.mouser.com/datasheet/2/360/Seiko_Instruments_MicroBattery_E_20230330_2024Jan_-3561061.pdf |
| PFMF.050.1 | https://eu.mouser.com/ProductDetail/Schurter/PFMF.050.2?qs=1auRipcfynCums5v1iucSA%3D%3D | https://eu.mouser.com/datasheet/2/358/typ_PFMF-1275918.pdf |
| Qwiic | https://eu.mouser.com/ProductDetail/Adafruit/4208?qs=PzGy0jfpSMtbScLbr0L5dw%3D%3D | https://learn.adafruit.com/introducing-adafruit-stemma-qt/technical-specs |
| W25Q512JVEIQ | https://eu.mouser.com/ProductDetail/Winbond/W25Q512JVEIQ?qs=l7cgNqFNU1jw6svr3at6tA%3D%3D | https://eu.mouser.com/datasheet/2/949/Winbond_W25Q512JV_Datasheet-3240039.pdf |
| PGB1010603MR | https://eu.mouser.com/ProductDetail/Littelfuse/PGB1010603MR?qs=gu7KAQ731URLg4GSnNNN7Q%3D%3D | https://www.littelfuse.com/assetdocs/pulseguard-esd-suppressors-pgb1-datasheet?assetguid=8a337998-d54d-466b-be4e-dc5bcd1f9321 |
| TANT Capacitor | https://eu.mouser.com/ProductDetail/KEMET/T491B107M006AT?qs=U312oeP%2FpiHOgyk6KO2m0g%3D%3D | https://www.vishay.com/docs/40002/293d.pdf |
| Capacitor 0.1uF | https://eu.mouser.com/ProductDetail/KYOCERA-AVX/KGF21BR71H104KT?qs=Jm2GQyTW%2FbjYyKCvVWAEOw%3D%3D | https://eu.mouser.com/datasheet/2/40/KGF-3223650.pdf |
| Capacitor 1uF | https://eu.mouser.com/ProductDetail/TAIYO-YUDEN/MAASL105CC7105MFCA01?qs=HFfMDpzxxd1Fn%2FInbJA7vw%3D%3D | https://eu.mouser.com/datasheet/2/396/TAIYO_YUDEN_04_27_2024_c_mlcc_A_e1-3451516.pdf |
| Capacitor 4.7uF | https://eu.mouser.com/ProductDetail/KYOCERA-AVX/CM05X5R475M16AH?qs=doiCPypUmgFT1xquZKhWtQ%3D%3D | https://eu.mouser.com/datasheet/2/40/Kyocera_AVX_Components_CM025_CM05_CM105_CM21_E217K-2932876.pdf |
| Capacitor 10 uF | https://eu.mouser.com/ProductDetail/TDK/CGA3EDX7T1A106M080AU?qs=ZcfC38r4PovL%2FBNBUzzBFw%3D%3D | https://product.tdk.com/system/files/dam/doc/product/capacitor/ceramic/mlcc/catalog/mlcc_automotive_general_en.pdf |
| Capacitor 100nF | https://eu.mouser.com/ProductDetail/KYOCERA-AVX/KAM21BR71H104JT?qs=Jm2GQyTW%2Fbic6Zk4McEt6w%3D%3D | https://eu.mouser.com/datasheet/2/40/AutoMLCCKAM-3216307.pdf |
| Resistor 0.47 | https://eu.mouser.com/ProductDetail/SEI-Stackpole/RNCL1210FT50L0?qs=17ckDYBRdelVLOJ%252BDFjlUw%3D%3D | https://eu.mouser.com/datasheet/2/385/sei_rncl-3223524.pdf |
| Resistor 2.2 | https://eu.mouser.com/ProductDetail/YAGEO/RT1206FRE072R2L?qs=XhSZopxZ3H6S0nYZD83pAA%3D%3D | https://eu.mouser.com/datasheet/2/447/PYu_RT_1_to_0_01_RoHS_L_15-3461507.pdf |
| Resistor 15 | https://eu.mouser.com/ProductDetail/Panasonic/ERA-6AHD150V?qs=MNPzkKEzRtQip8ZeekU%252BRw%3D%3D | https://industrial.panasonic.com/cdbs/www-data/pdf/RDM0000/AOA0000C307.pdf |
Resistor 200 | https://eu.mouser.com/ProductDetail/TE-Connectivity-Holsworthy/RP73C2B200RFTDF?qs=n4i9pByFsMR0dWGpf721CA%3D%3D | https://eu.mouser.com/datasheet/2/418/10/ENG_DS_1773272_M1-1588495.pdf |
| Resistor 2k | https://eu.mouser.com/ProductDetail/Panasonic/ERA-3AED202V?qs=yocZuyCaXdM2cBmjcGkIyA%3D%3D | https://industrial.panasonic.com/cdbs/www-data/pdf/RDM0000/AOA0000C307.pdf |
| Resistor 5k1 | https://eu.mouser.com/ProductDetail/Vishay-Beyschlag/MCS04020C5101FE000?qs=wTZ%2FFzl837YsKPLPRIXUbg%3D%3D | https://www.vishay.com/docs/28705/mcx0x0xpro.pdf |
| Resistor 10k | https://eu.mouser.com/ProductDetail/SEI-Stackpole/RNWA0612BTE10K0?qs=IKkN%2F947nfD62pDKWE9ZTQ%3D%3D | https://eu.mouser.com/datasheet/2/385/SEI_RNWA-3473840.pdf |
| Resistor 100K | https://eu.mouser.com/ProductDetail/Panasonic/ERA-3AED104V?qs=MNPzkKEzRtQIWcmUWL4kjg%3D%3D | https://industrial.panasonic.com/cdbs/www-data/pdf/RDM0000/AOA0000C307.pdf |
| E-Paper Display | https://eu.mouser.com/ProductDetail/Microtips-Technology/MT-DEPG0750BNU590F1?qs=DPoM0jnrROVRMMTX0WzK%252Bw%3D%3D| https://eu.mouser.com/datasheet/2/271/MT_DEPG0750BNU590F1_V2_7-1894282.pdf |
| LP584174 Battery| https://www.tme.eu/en/details/accu-lp584174_cl/rechargeable-batteries/cellevia-batteries/l584174/ | https://www.tme.eu/Document/e0683d8c34e6d878124489f71bffb6ee/cel0014.pdf |


## Observații
- Am modificat footprint-ul componentei J2, prin mutarea pad-urilor GND, VUSB și B8 cu câțiva mm spre stânga, pentru a corecta eroarea ”SMD Hole”.
