# 🚀 INSTALLATION -- Zendure SolarFlow 2400 AC Pro KI-Steuerung

**Version V9 (KI + PID + Null-Einspeisung + Peak-Vorhersage)**

Diese Anleitung beschreibt die vollständige Einrichtung der
KI-gestützten, dynamischen Steuerung für den Zendure SolarFlow 2400 AC
in Home Assistant.

------------------------------------------------------------------------

## ✅ Funktionen

-   Intelligente Ladeplanung (Heute + Morgen)
-   PI-Entladeregelung (PID-Light)
-   Null-Einspeisung
-   Dynamische Preislogik
-   PV-Priorisierung
-   SoC-Schutz mit Notladestufe
-   Stabile Entladeleistung ohne Flattern
-   10-Sekunden-Regelintervall

------------------------------------------------------------------------

## 1. Voraussetzungen

-   Home Assistant 2024.x oder neuer
-   Zendure SolarFlow 2400 AC Integration
-   Energiedaten:
    -   sensor.gesamtverbrauch
    -   sensor.sb2_5\_1vl_40_401_pv_power
    -   sensor.einspeisung
    -   sensor.bezug
-   Strompreis-Prognose (z.B. Tibber)

------------------------------------------------------------------------

## 2. Template-Integration aktivieren

In `configuration.yaml` hinzufügen:

``` yaml
template: !include_dir_merge_list templates/
```

Home Assistant neu starten.

------------------------------------------------------------------------

## 3. Helfer erstellen

### input_number

-   zendure_soc_reserve_min (5--30)
-   zendure_soc_ziel_max (80--100)
-   zendure_max_ladeleistung (0--2400)
-   zendure_max_entladeleistung (0--800)
-   zendure_notladeleistung (100--2000)
-   zendure_pid_i\_term (0--800)

### input_select

    input_select.zendure_betriebsmodus
    Optionen:
    - Automatik
    - Sommer
    - Winter
    - Manuell

------------------------------------------------------------------------

## 4. Template-Sensoren

Ordner erstellen:

    /config/templates/

Dort ablegen: - akku_empfehlung.yaml - ki_ladeplan.yaml -
restlaufzeit.yaml - entscheidungsstatus.yaml - ladeplanung_debug.yaml

Home Assistant neu starten.

------------------------------------------------------------------------

## 5. Automation installieren

Pfad: Einstellungen → Automatisierungen → Neue → Code-Editor

Einfügen:

``` yaml
# zendure_v9.yaml Vollständiger Code aus Projekt
```

------------------------------------------------------------------------

## 6. Dashboard einrichten

Neues Dashboard: Name: Zendure Pro

Code-Editor:

``` yaml
dashboard/zendure_dashboard.yaml
```

------------------------------------------------------------------------

## 7. Funktionstest

  Szenario           Erwartetes Verhalten
  ------------------ ----------------------
  SoC \< 8%          Notladung aktiv
  Hoher Strompreis   Entladen
  Günstiger Preis    billig_laden
  PV-Überschuss      laden
  Manuell            Alles = 0

------------------------------------------------------------------------

## 8. Feinjustierung

### PID-Parameter

-   kp: 0.5 -- 1.2
-   ki: 0.01 -- 0.04
-   abs_tol: 10--40W

------------------------------------------------------------------------

## 9. Debug-Sensoren prüfen

Diese müssen gültige Werte liefern:

-   sensor.zendure_akku_steuerungsempfehlung
-   sensor.zendure_ki_ladeplan
-   sensor.zendure_restlaufzeit_stunden
-   sensor.zendure_debug_planung

------------------------------------------------------------------------

## ✅ Fertig!

Deine Zendure KI-Steuerung ist nun aktiv und optimiert dein
Energiemanagement.

Viel Erfolg & Autarkie! ⚡🤖
