# InTheWoods

Edge-Impulse-basiertes Objekterkennungs-Projekt für das **Seeed XIAO ESP32S3 CAM**.  
Ziel: Baum/Hindernis erkennen und per LEDs/Lüftern eine einfache Ausweichlogik ansteuern.

## Inhalt des Repos

- **InTheWoods.ino** – Arduino-Sketch (Inference + Aktorlogik)
- **ei-inthewoods-arduino-1.0.5.zip** – Edge-Impulse-Modell als Arduino-Bibliothek (ZIP)
- **docs/Technische_Dokumentation.pdf** – vollständige technische Dokumentation (Training, Deployment, Elektronik, Troubleshooting)
- **README.md**

## Hardware

Mikrocontroller: Seeed Studio XIAO ESP32S3 Sense
– integrierte Kamera (OV2640)
– mit PSRAM für Edge-Impulse-Inferenz

Antrieb: 2 Gleichstrommotoren mit Propellern (Steuerung links/rechts über MOSFETs)

Signalausgabe: 2 LEDs (optische Richtungsanzeige: links = blau, rechts = rot)

Energieversorgung: Lithium-Polymer-Batterie (tragbar, leicht)

Trägerstruktur: Leichtbau-Aufbau, getragen durch mehrere Heliumballons

**Pinbelegung (im Sketch definiert):**

| Funktion     | Pin |
|--------------|-----|
| LEFT_LED     | D0  |
| RIGHT_LED    | D1  |
| LEFT_FAN     | D3  |
| RIGHT_FAN    | D8  |


## Software-Voraussetzungen

- Arduino IDE (aktuell)
- ESP32 Core: „esp32 by Espressif Systems“ (Boards Manager)
- Board: Seeed XIAO ESP32S3
- Edge-Impulse-Bibliothek: ZIP importieren (siehe Quickstart)

## Quickstart

1. Repo klonen / ZIP laden und Projekt öffnen.  
2. Edge-Impulse-Bibliothek importieren:
   Arduino IDE → *Sketch* → *Include Library* → *Add .ZIP Library…* → `ei-inthewoods-arduino-1.0.5.zip` wählen.  
3. Board wählen: 
   *Tools* → *Board* → *ESP32 Arduino* → *Seeed XIAO ESP32S3*.  
4. Sketch öffnen & flashen: `InTheWoods.ino` kompilieren und hochladen.  

> Details zu Modell-Parametern, Speicher, Bildformaten: siehe **docs/Technische_Dokumentation.pdf**.


## Erkennungs- & Steuerlogik (kurz)

- Modellbreite 96 px (x-Achse 0–95).  
- **Links**: `x < 32` (und `width > 1`)  
- **Rechts**: `x > 64` (und `width > 1`)  
- **Zentral**: sonst.

**Aktorverhalten:**

- **Kein Objekt** *(Konfidenz ≤ 0.5)* → geradeaus (beide Lüfter mittel, LEDs aus)  
- **Objekt links** → LEFT_LED an, RIGHT_FAN Rampe (links eindrehen)  
- **Objekt rechts** → RIGHT_LED an, LEFT_FAN Rampe (rechts eindrehen)  
- **Zentral** → Stop (alle aus)

## Dokumentation

Die **Technische Dokumentation** mit allen wichtigen Informationen (Modelltraining, Datensätze, Labeling, Arduino-Setup, Pin-Mapping, Deployment-Schritte, Tests, bekannte Issues) liegt unter:  
**`docs/Technische_Dokumentation.pdf`**

