# 🔋 Intelligente Steuerung für Zendure SolarFlow SF2400AC

Eine hochentwickelte, KI-gestützte und PID-geregelte Home-Assistant-Steuerung für den Zendure SF2400AC Akku mit:

✅ Dynamischer Strompreis-Optimierung  
✅ KI-basierter Ladeplanung  
✅ Lastabhängiger Entladung (Null-Einspeisung)  
✅ PID-Regelung mit Totzone  
✅ SoC-Schutz & intelligente Notladung  
✅ Peak-Vorhersage  
✅ Debug-Dashboard & Pro-Überwachung  

---

## 🎯 Ziel des Projekts

Maximale Wirtschaftlichkeit und Akkuschonung bei gleichzeitiger Maximierung des Eigenverbrauchs.

Das System analysiert:
- Strompreise heute & morgen
- Hauslast in Echtzeit
- PV-Erzeugung
- Netzeinspeisung
- State of Charge (SoC)
- Historische Peaks

Und entscheidet intelligent:
- Wann geladen wird
- Wann entladen wird
- Wie stark entladen wird
- Wann Reserve gehalten wird

---

## 🚀 Features

| Funktion | Beschreibung |
|----------|--------------|
| KI-Ladeplanung | vorausschauende Optimierung auf Basis zukünftiger Preise |
| PID-Regelung | extrem stabile Entladeleistung ohne Flackern |
| Null-Einspeisung | verhindert Stromverschwendung |
| Totzone | verhindert unnötige Mikro-Regelungen |
| dynamisches SoC-Min | automatischer Notladepunkt |
| Debug-Sensoren | detaillierte Entscheidungsanalyse |

---

## 🔧 Voraussetzungen

- Home Assistant >= 2024.6
- HACS
- Zendure SolarFlow Integration
- Sensoren:
  - `sensor.gesamtverbrauch`
  - `sensor.sb2_5_1vl_40_401_pv_power`
  - `sensor.einspeisung`
  - `sensor.bezug`
  - `sensor.electricity_price_*`

---

## 📌 Installation (Kurzfassung)

1. Helpers anlegen:
   - input_number.zendure_soc_reserve_min
   - input_number.zendure_soc_ziel_max
   - input_number.zendure_pid_i_term
   - input_number.zendure_max_ladeleistung
   - input_number.zendure_max_entladeleistung
   - input_number.zendure_notladeleistung

2. Template-Sensoren importieren (siehe templates/)

3. Automation importieren (automation_v9.yaml)

4. Optional: Debug-Dashboard importieren

---

## 🧠 Entscheidungslogik (vereinfacht)

1. Notladebereich erreicht → erzwungenes Laden  
2. Hohe Preise erkannt → lastabhängige Entladung  
3. Zukünftige Peaks → vorausschauendes Laden  
4. Niedriger Preis → günstiges Laden  
5. Normale Zeit → Standby

---

## 📈 Versionierung

Siehe CHANGELOG.md

---

## 🤝 Mitwirken

Dieses Projekt ist bewusst Open-Source konzipiert. Pull Requests willkommen!
