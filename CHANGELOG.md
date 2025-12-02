# Changelog – Intelligente Steuerung Zendure SF2400AC

## v1.0.0 – V9 KI + PID Pro

- KI-basierte Ladeplanung:
  - Auswertung der 15-Minuten-Preisprognose für heute und morgen
  - Erkennung teurer Zeitfenster (Preis ≥ „teuer“-Schwelle)
  - Berechnung des geschätzten Energiebedarfs für Peak-Zeiten
  - Ergebnis-Sensor `sensor.zendure_ki_ladeplan` (`laden_erforderlich` / `ausreichend_geladen`)
- Steuerungsempfehlungs-Sensor:
  - `sensor.zendure_akku_steuerungsempfehlung`
  - Zustände: `laden`, `billig_laden`, `entladen`, `standby`
  - Integration von:
    - SoC-Reserve und dynamischer Notfallgrenze
    - PV-Überschuss
    - aktuellem Preis vs. billig/teuer
    - KI-Ladeplan
    - Betriebsmodus (Sommer/Winter/Automatik)
- Automation V9:
  - PI-/PID-light Entladeregelung:
    - P-Term (Fehler * `kp`)
    - I-Term (akkumuliert, begrenzt, gespeichert in `input_number.zendure_pid_i_term`)
    - Totzone (`abs_tol`) gegen Flackern
  - Lastabhängige Entladung für Null-Einspeisungsziel
  - Notladung unterhalb dynamischer SoC-Grenze
- Debug-Sensoren:
  - `sensor.zendure_debug_planung` – Peak-Dauer, Bedarf und verfügbarer Akkuinhalt
  - `sensor.zendure_entscheidungsstatus` – kompakte Zusammenfassung
- Debug-Dashboard:
  - Anzeige von Empfehlung, KI-Plan, SoC, Preis, Limits und Live-Daten

## v1.1.0 - V10.2 – Dezember 2025

### Neu
- KI-Ladeplan Pro (Future-Mode Peak Prediction)
- dynamische Peakblock-Erkennung
- separate Analyse Heute / Morgen
- dynamischer Energiebedarf basierend auf `input_number.zendure_max_entladeleistung`
- Ermittlung der benötigten Restenergie über SoC-Min
- Debug Pro Sensor mit Zeitprojektion

### Verbesserungen
- PID-Regler stabilisiert (I-Term Bremsung, Totzone)
- Null-Einspeisung stärker priorisiert
- Ladeempfehlung reagiert korrekt in KI-Phasen
- Verbesserung der Standby-Logik bei SoC-Min
- Performance-Optimierungen

### Bugfixes
- KI-Ladeplan falsche Peaks → korrigiert
- Standby falsch ausgelöst → behoben
- Frühzeitiges Laden durch fehlerhafte Bedingungen → korrigiert
