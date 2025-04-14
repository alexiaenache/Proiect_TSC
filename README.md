# 📖 OpenBook – E-Reader cu ESP32-C6

**-TSC-**  
Alexia-Ioana Enache – 331CA

---

## 🧠 Arhitectură generală

Sistemul reprezintă un cititor digital inteligent, construit în jurul microcontrollerului **ESP32-C6**, optimizat pentru consum redus și interacțiune minimă din partea utilizatorului, având ca scop principal afișarea eficientă de conținut pe un ecran E-Ink.

---

## 🧩 Componente principale și funcționalitate

### 1. ESP32-C6 – Unitatea centrală de control
- **Funcție:** Coordonează toate componentele, gestionează afișajul și comunicația.
- **Caracteristici cheie:**
  - Procesor RISC-V performant.
  - Suport Wi-Fi 6 (802.11ax) pentru actualizări sau funcționalitate în rețea.
  - Moduri avansate de economisire a energiei (deep sleep, modem sleep).

---



### 2. Afișaj E-Ink
- **Tip:** Ecran monocrom (ex. 2.9"), optimizat pentru citire.
- **Control:** Prin interfață SPI, cu linii dedicate de comandă (CS, DC, RST, BUSY).
- **Beneficii:** Consum de curent extrem de redus între refresh-uri – ideal pentru aplicații de tip e-paper.

---

### 3. Senzor de mediu – BME688
- **Scop:** Monitorizare temperatură, umiditate, presiune și calitatea aerului.
- **Conectivitate:** I2C (implicit) sau SPI (alternativ).
- **Consum:** µA în stand-by, câțiva mA în funcționare activă.

---

### 4. Ceas în timp real – DS3231
- **Funcționalitate:** Menține ora exactă, ideal pentru time-stamping sau notificări programate.
- **Interfață:** I2C + pin de alarmă (INT/SQW).
- **Consum:** Sub 2 µA în menținerea ceasului pe baterie.

---

### 5. Memorie SPI – W25Q512 (64Mbit NOR Flash)
- **Rol:** Depozitare pentru fișiere, cărți, date configurabile.
- **Protocol:** SPI, suportând frecvențe mari pentru transfer rapid.
- **Eficiență energetică:** Consumă doar în timpul operațiunilor active.

---

### 6. Sistem de alimentare
- **USB-C:** Intrare de 5V pentru alimentare și comunicație.
- **Încărcare baterie:** MCP73831 controlează încărcarea Li-Po, cu curent reglat prin rezistor (ex. 500mA).
- **Conversie tensiune:**
  - **LDO / DC-DC:** Reglează 3.7V de la baterie la 3.3V pentru întreaga platformă.
  - Opțional: step-up DC-DC pentru a obține 5V dacă e necesar.

---

### 7. Control fizic – Butoane SMD
- 3 butoane dedicate: Boot, Reset și Funcțional.
- Legate la GPIO-uri configurate cu pull-up/pull-down.
- Logica de debounce poate fi software sau hardware.

---

### 8. Alte funcționalități
- Protecții ESD aplicate pe liniile critice: USB, SPI, I2C, alimentare.
- **Test Pads**: Acces facil la semnale importante (MISO, MOSI, SCK, RX, TX, GND, 3V3) pentru depanare sau programare externă.

---

## 🛠️ Considerații hardware (PCB Design)

### Rutare & alimentare:
- **Trasee de putere:** Minim 0.3 mm pentru 3V3, 5V, VBAT.
- **Linii de semnal:** Minim 0.15 mm pentru SPI, I2C, UART etc.
- **Grosime PCB:** Ideal < 1 mm pentru carcase compacte.

### Decuplare & stabilitate:
- Condensatoare **100nF** (0402) pentru fiecare pin VCC.
- Condensatoare **4.7 µF - 10 µF** pentru stabilizarea LDO și surse locale.

### Antenă ESP32-C6:
- Zona antenei trebuie eliberată de cupru (decupaj în GND + semnale), conform datasheetului Espressif.

### DRC/ ERC:
- Verificări obligatorii:
  - Unghiuri ≤ 45°.
  - Planuri GND corect cusute cu via stitching.
  - Clearance și distanțe respectate conform producătorului.

### Layout:
- Componente doar pe **Top Layer** (SMD).
- USB-C și butoanele poziționate ergonomic și accesibil.

### Test Pad-uri:
- Expunere pentru debug, programare și validare semnale: 3V3, GND, RX, TX, SPI.

---
