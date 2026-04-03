# RFID LED Controller met Temperatuurmeting & Blynk Dashboard

> **Auteur(s):** Brecht & Yanis 
> **School:** ThomasMore
> **Datum:** April 2026

---

## Projectbeschrijving

Dit project is een slim IoT-systeem gebouwd op een **ESP32** microcontroller. Via RFID-kaarten kunnen individuele LEDs aan- en uitgeschakeld worden. Tegelijk wordt de omgevingstemperatuur gemeten via een DHT11-sensor. Alle data is live zichtbaar op een **Blynk IoT dashboard** via WiFi.

### Functies
- 3 RFID-kaarten schakelen elk een aparte LED
- Temperatuurmeting elke 5 seconden
- Temperatuur-LED knippert als het warmer is dan 25°C
- Live weergave van LED-statussen en temperatuur in de Blynk app
- LEDs kunnen ook vanuit de Blynk app bediend worden

---

##  Demo Video


---

## Benodigde Hardware

| Component | Aantal | Omschrijving |
|---|---|---|
| ESP32 | 1 | Microcontroller met WiFi |
| MFRC522 RFID lezer | 1 | SPI aansluiting |
| RFID kaarten/tags | 3 | Elke kaart stuurt 1 LED |
| DHT11 sensor | 1 | Temperatuur- en luchtvochtigheidsensor |
| LED (rood/groen/blauw) | 3 | Aangestuurd via RFID |
| LED (geel) | 1 | Temperatuurwaarschuwing |
| Weerstanden 220Ω | 4 | Stroombeperking voor LEDs |
| Breadboard | 1 | Prototype opstelling |
| Jumper wires | - | Verbindingsdraden |

---

## Pinout / Aansluitingen

| Component | ESP32 Pin |
|---|---|
| RFID SS | 10 |
| RFID RST | 5 |
| RFID SCK | 4 |
| RFID MISO | 6 |
| RFID MOSI | 7 |
| LED 1 | 2 |
| LED 2 | 3 |
| LED 3 | 11 |
| LED Temperatuur | 18 |
| DHT11 Data | 23 |

---

## Blynk Dashboard Instelling

### Benodigde Datastreams

| Virtual Pin | Naam | Type | Bereik |
|---|---|---|---|
| V0 | LED 1 | Integer | 0 – 1 |
| V1 | LED 2 | Integer | 0 – 1 |
| V2 | LED 3 | Integer | 0 – 1 |
| V3 | Temperatuur | Double | 0 – 50 |

### Dashboard Widgets
- **3× Button** → V0, V1, V2 (LED aan/uit vanuit app)
- **1× Gauge of Chart** → V3 (temperatuurweergave)
- **Optioneel:** Event notificatie bij temperatuur > 25°C



## Software & Libraries

### Gebruikte Libraries

| Library | Versie | Waarvoor |
|---|---|---|
| Blynk | 1.3.2 | IoT dashboard verbinding |
| MFRC522 | 1.4.11 | RFID lezer aansturen |
| DHT sensor library | 1.4.6 | DHT11 temperatuursensor |

Installeer via Arduino IDE: **Sketch → Include Library → Manage Libraries**

### Programmalogica

```
Start
 ├── Verbind met WiFi & Blynk
 ├── Initialiseer RFID, DHT11, LEDs
 └── Loop:
      ├── Blynk.run() (verbinding onderhouden)
      ├── Elke 5 seconden:
      │    ├── Lees temperatuur
      │    ├── Stuur naar Blynk (V3)
      │    └── Als > 25°C → teWarm = true
      ├── Als teWarm → knippert temperatuur-LED elke 300ms
      └── Als RFID kaart gedetecteerd:
           ├── UID "041C96EA9D1B91" → toggle LED 1 + update V0
           ├── UID "613A2BAA"       → toggle LED 2 + update V1
           ├── UID "E0837A13"       → toggle LED 3 + update V2
           └── Onbekende UID       → print melding
```

---

## Installatie & Gebruik

1. **Clone deze repository**
   ```bash
   git clone https://github.com/gebruikersnaam/project-naam.git
   ```

2. **Maak een Blynk account** op [blynk.cloud](https://blynk.cloud) en maak een nieuw Template aan met de datastreams uit de tabel hierboven.

3. **Vul je gegevens in** bovenaan `code/main.ino`:
   ```cpp
   #define BLYNK_TEMPLATE_ID   "TMPLxxxxxx"
   #define BLYNK_TEMPLATE_NAME "RFID Dashboard"
   #define BLYNK_AUTH_TOKEN    "jouw_auth_token"
   char ssid[] = "jouw_wifi_naam";
   char pass[] = "jouw_wifi_wachtwoord";
   ```

4. **Upload de code** via Arduino IDE naar je ESP32.

5. **Open de Blynk app** en activeer je dashboard.

---


## Mogelijke Problemen

| Probleem | Oplossing |
|---|---|
| DHT11 geeft leesfout | Controleer de bedrading en de pull-up weerstand op de datalijn |
| RFID leest geen kaarten | Controleer SPI pinnen en of de MFRC522 3.3V krijgt en pas kaart numemr aan in code|
| Blynk verbindt niet | Controleer WiFi gegevens en Auth Token |
| UID wordt niet herkend | Lees de UID uit via Serial monitor en pas de code aan |
