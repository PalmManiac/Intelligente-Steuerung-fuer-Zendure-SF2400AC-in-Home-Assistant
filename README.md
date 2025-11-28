# 🔋 Intelligente Steuerung für Zendure SolarFlow SF2400AC in Home Assistant

Dieses Projekt enthält eine **vollautomatische, praxiserprobte Steuerung** für den
**Zendure SolarFlow 2400 AC** in **Home Assistant**.

Ziele:

- Maximale Nutzung des eigenen PV-Stroms
- Minimierung der Stromkosten bei dynamischen Preisen (z. B. Tibber)
- Schutz des Akkus (SoC-Reserven, Notladung)
- Möglichst keine Netzeinspeisung in teuren Zeiten
- Ruhiges, stabiles Regelverhalten (kein „Flackern“)

---

## 🚀 Funktionsumfang (V9 – KI + PID Pro)

- 🔮 **KI-Ladeplan**
  - Auswertung der 15-Minuten-Strompreisprognose *heute + morgen*
  - Erkennung teurer Zeitfenster (Peaks)
  - Abschätzung, wie viel Energie der Akku für diese Peaks braucht
  - Ergebnis-Sensor: `sensor.zendure_ki_ladeplan`
    - `laden_erforderlich`
    - `ausreichend_geladen`

- 📊 **Steuerungs-Empfehlung**
  - Sensor: `sensor.zendure_akku_steuerungsempfehlung`
  - Mögliche Zustände:
    - `laden`
    - `billig_laden`
    - `entladen`
    - `standby`
  - Berücksichtigt:
    - SoC-Reserve (einstellbar, z. B. 12 %)
    - dynamische Notfallgrenze (Reserve − 4 %, aber nie unter 5 %)
    - PV-Überschuss
    - Betriebsmodus (`Automatik`, `Sommer`, `Winter`, `Manuell`)
    - aktuelle Preise & Schwellen (billig/teuer)
    - Ergebnis des KI-Ladeplans

- 🧠 **PI-/PID-Light Entladeregelung**
  - Lastabhängige Entladung (orientiert sich an der realen Hauslast abzüglich PV)
  - P-Anteil (`kp`) → schnelle Reaktion
  - I-Anteil (`ki`) → langfristige Korrektur, gespeichert in `input_number.zendure_pid_i_term`
  - Totzone (`abs_tol`), um kleine Schwankungen zu ignorieren
  - Ziel: möglichst **Null-Einspeisung** in teuren Zeiten

- 🛟 **SoC-Schutz & Notladung**
  - SoC-Reserve-Minimum frei einstellbar: `input_number.zendure_soc_reserve_min`
    - z. B. 12 %
  - Notfall-Grenze: `Reserve − 4 %`, aber nie unter 5 %
  - Unterhalb dieser Notfallgrenze → erzwungenes Laden mit `input_number.zendure_notladeleistung`

- 🧩 **Betriebsmodi**
  - `Automatik` – du kannst z. B. per separatem Sensor automatisch Sommer/Winter setzen
  - `Sommer` – Fokus auf Autarkie, keine aggressive Netzladung
  - `Winter` – gezieltes Nutzen günstiger Preisfenster
  - `Manuell` – Automation greift nicht ein, Limits steuerst du direkt

- 🪪 **Debug-Sensoren & Dashboard**
  - Peak-Analyse, KI-Plan, Steuerungsempfehlung, SoC, Preis, Limits
  - Debug-View zur Analyse des Verhaltens

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

---

## 🔧 Installation (Kurzfassung)

1. Dieses Projekt in einen Ordner (z. B. in dein GitHub-Repo) kopieren.
2. In Home Assistant:
   - Helfer (`input_number`, `input_select`) anlegen.
   - In `configuration.yaml` sicherstellen:
     ```yaml
     template: !include_dir_merge_list templates/
     ```
   - Die Template-Dateien aus dem Ordner `templates/` dort verfügbar machen.
   - Automation aus `automation/automation_v9.yaml` importieren (GUI → „Aus YAML importieren“).
   - Dashboard-RAW-Konfiguration aus `dashboard/debug_dashboard.yaml` übernehmen.
3. Home Assistant neu starten.
4. Verhalten beobachten und Fein-Tuning vornehmen:

   - `input_number.zendure_max_entladeleistung`
   - `input_number.zendure_max_ladeleistung`
   - `input_number.zendure_soc_reserve_min`
   - die Werte `kp`, `ki`, `abs_tol` in der Automation (für das Regelverhalten)

---

## 📜 Changelog

Siehe [CHANGELOG.md](CHANGELOG.md).

---

## 🤝 Mitmachen

Pull Requests, Issues und Vorschläge für Verbesserungen sind willkommen 🙂
