# Zendure Home Assistant Steuerung – Pro Version (Release 10.2)

Dies ist die vollständig optimierte Steuerung für **Zendure SolarFlow 2400 AC** in Home Assistant.

Enthalten sind:

- KI-Ladeplanung Pro – mit Peak-Vorhersage über 48h
- dynamische Ermittlung des Energiebedarfs
- vorausschauende Ladeentscheidungen
- PI-Regler (PID-Light) zur stabilen Entladung ohne Flackern
- dynamische Null-Einspeisung
- Sommer/Winter-Automatik
- Debug-Pro-Modul
- optionaler Mini-Zeitstrahl für UI
- vollständige YAML-Automationen, Sensoren und helper-Definitionen

**Ziel:** Maximale Wirtschaftlichkeit & Stabilität mit realer Lastregelung und Preisoptimierung.

---

## Features

### 🔋 KI-Ladeplanung (Pro)
- erkennt alle Preisspitzen automatisch (heute + morgen)
- berechnet notwendige Energie für alle Peaks
- berechnet tatsächliche Restenergie über SoC-Limit
- legt optimale Ladezeitpunkte fest
- berücksichtigt Preisverlauf und Schwellwerte automatisch

### ⚡ PID-Entladung (Pro-Modus)
- PI-Regler (proportional + integral)
- dynamische Null-Einspeisung ohne Überschwinger
- Totzone (Deadband) vermeidet „Zittern“
- reagiert in <1s auf schnellen Lastwechsel

### 🌞 Sommer/Winter-Modus
Automatische Umschaltung über PV-7-Tage-Mittel.

### 🛡 SoC-Schutz
- entladen bis SoC_Min
- Notladung bei SoC_Notfall (dynamisch: SoC_Min - 4%, aber min. 5%)

### 📊 Debug Pro
- Peakblöcke
- benötigte kWh
- verfügbare kWh
- maximaler Preis in Peakphase
- Zeit bis Peak
- Future Mode / Normal Mode Status

---

## Installation

Siehe **INSTALLATION.md**

---

## Changelog

Siehe **CHANGELOG.md**
