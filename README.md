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

## 🧱 Voraussetzungen

- Home Assistant (2024.x oder neuer empfohlen)
- In der Zendure-App die Grenze für Laden und Entladen auf maximum (2400Watt) stellen
- Die Tibber-Preise für Heute und Morgen mit Hilfe der folgenden Anleitung einrichten: 
  https://www.secretisland.de/home-assistant-tibber-preise-15-minuten-intervall/
- Integration(en) für:
  - Zendure SolarFlow 2400 AC (Custom-Integration / Add-on)
  - dynamische Strompreise (z. B. Tibber)
- Entitäten (bitte ggf. Namen anpassen):
  - `sensor.solarflow_2400_ac_electric_level` (SoC in %)
  - `sensor.solarflow_2400_ac_available_kwh` (nutzbare Energie im Akku)
  - `number.solarflow_2400_ac_input_limit` (Ladeleistung-Begrenzung)
  - `number.solarflow_2400_ac_output_limit` (Entladeleistung-Begrenzung)
  - `select.solarflow_2400_ac_ac_mode` (`input` / `output`)
  - `sensor.gesamtverbrauch` (Hausgesamtleistung in Watt)
  - `sensor.sb2_5_1vl_40_401_pv_power` (PV-Leistung in Watt)
  - `sensor.einspeisung` (Netzeinspeisung in Watt, positiv bei Einspeisung)
  - `sensor.bezug` (Netzbezug in Watt)
  - `sensor.electricity_price_paul_schneider_strasse_39` (aktueller Preis)
  - `sensor.strompreis_prognose_15min_paul_schneider_strasse_39`
    - Attribut `today`: Liste von Objekten mit `total`
    - Attribut `tomorrow`: dito
- Helfer:
  - `input_number.zendure_soc_reserve_min`
  - `input_number.zendure_soc_ziel_max`
  - `input_number.zendure_max_ladeleistung`
  - `input_number.zendure_max_entladeleistung`
  - `input_number.zendure_notladeleistung`
  - `input_number.zendure_pid_i_term`
  - `input_select.zendure_betriebsmodus`  
    mit Optionen: `Automatik`, `Sommer`, `Winter`, `Manuell`

## Installation

Siehe **INSTALLATION.md**

---

## Changelog

Siehe **CHANGELOG.md**
