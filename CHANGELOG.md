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
