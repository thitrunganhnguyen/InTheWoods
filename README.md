# InTheWoods

Edge-Impulse-basiertes Objekterkennungs-Projekt für das **Seeed XIAO ESP32S3 CAM**.  
Ziel: Baum/Hindernis erkennen und per LEDs/Lüftern eine einfache Ausweichlogik ansteuern.

## Inhalt des Repos

- **InTheWoods.ino** – Arduino-Sketch (Inference + Aktorlogik)
- **ei-inthewoods-arduino-1.0.5.zip** – Edge-Impulse-Modell als Arduino-Bibliothek (ZIP)
- **docs/Technische_Dokumentation.pdf** – vollständige technische Dokumentation (Training, Deployment, Elektronik, Troubleshooting)
- **README.md** – diese Datei

## Hardware

- Seeed **XIAO ESP32S3 CAM** (mit PSRAM, OV2640)
- 2 LEDs (Seitenanzeige)
- 2 Lüfter/Motoren (Steuerung links/rechts)

**Pinbelegung (im Sketch definiert):**

| Funktion     | Pin |
|--------------|-----|
| LEFT_LED     | D0  |
| RIGHT_LED    | D1  |
| LEFT_FAN     | D3  |
| RIGHT_FAN    | D8  |


## Software-Voraussetzungen

- **Arduino IDE** (aktuell)
- ESP32 Core: **„esp32 by Espressif Systems“** (Boards Manager)
- Board: **Seeed XIAO ESP32S3**
- Edge-Impulse-Bibliothek: **ZIP importieren** (siehe Quickstart)

## Quickstart

1. **Repo klonen / ZIP laden** und Projekt öffnen.  
2. **Edge-Impulse-Bibliothek importieren:**  
   Arduino IDE → *Sketch* → *Include Library* → *Add .ZIP Library…* → `ei-inthewoods-arduino-1.0.5.zip` wählen.  
3. **Board wählen:**  
   *Tools* → *Board* → *ESP32 Arduino* → **Seeed XIAO ESP32S3**.  
4. **Sketch öffnen & flashen:** `InTheWoods.ino` kompilieren und hochladen.  
5. **Seriellen Monitor** auf **115200 Baud** stellen.

> Details zu Modell-Parametern, Speicher, Bildformaten und Fehlersuche: siehe **docs/Technische_Dokumentation.pdf**.


## Erkennungs- & Steuerlogik (kurz)

- Modellbreite **96 px** (x-Achse 0–95).  
- **Links**: `x < 32` (und `width > 1`)  
- **Rechts**: `x > 64` (und `width > 1`)  
- **Zentral**: sonst.

**Aktorverhalten:**

- **Kein Objekt** *(Konfidenz ≤ 0.5)* → **geradeaus** (beide Lüfter mittel, LEDs aus)  
- **Objekt links** → **LEFT_LED an**, **RIGHT_FAN Rampe** (links eindrehen)  
- **Objekt rechts** → **RIGHT_LED an**, **LEFT_FAN Rampe** (rechts eindrehen)  
- **Zentral** → **Stop** (alle aus)

## Dokumentation

Die **Technische Dokumentation** mit allen wichtigen Informationen (Modelltraining, Datensätze, Labeling, Arduino-Setup, Pin-Mapping, Deployment-Schritte, Tests, bekannte Issues) liegt unter:  
**`docs/Technische_Dokumentation.pdf`**

