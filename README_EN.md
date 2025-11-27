# Zendure SolarFlow SF2400AC – Smart Control (V9 AI + PID) for Home Assistant

This project provides an **advanced, real-world tested control logic**
for **Zendure SolarFlow 2400 AC** inside **Home Assistant**.

Goals:

- Maximize self-consumption of PV power
- Minimize electricity cost with dynamic tariffs (e.g. Tibber)
- Protect the battery (SoC reserve, emergency charging)
- Avoid feeding into the grid during high price periods (as far as possible)
- Stable behaviour without oscillation

---

## 🚀 Features (V9 – AI + PID Pro)

- 🔮 **AI-based charge planning**
  - Uses 15-minute price forecast (today + tomorrow)
  - Detects expensive time windows (peaks)
  - Estimates how much energy the battery should hold for these peaks
  - Result sensor: `sensor.zendure_ki_ladeplan`
    - `laden_erforderlich` (charging required)
    - `ausreichend_geladen` (sufficiently charged)

- 📊 **Control recommendation**
  - Sensor: `sensor.zendure_akku_steuerungsempfehlung`
  - States:
    - `laden` (charge)
    - `billig_laden` (cheap charge)
    - `entladen` (discharge)
    - `standby`
  - Takes into account:
    - SoC reserve / emergency limit
    - PV surplus
    - operating mode (Automatic / Summer / Winter / Manual)
    - current price & cheap/expensive thresholds
    - the AI charge planner

- 🧠 **PI-/PID-light discharge controller**
  - Tries to follow the **real house load** (minus PV)
  - P-term: fast reaction
  - I-term: long-term correction, stored in `input_number.zendure_pid_i_term`
  - Deadband (`abs_tol`) to avoid nervous behaviour
  - Target: **near-zero grid feed-in** during expensive periods

- 🛟 **SoC protection & emergency charging**
  - Reserve minimum configurable: `input_number.zendure_soc_reserve_min` (e.g. 12 %)
  - Emergency limit: reserve − 4 %, but never below 5 %
  - Below emergency limit → forced charging at `input_number.zendure_notladeleistung`

---

## 🧱 Requirements

See `README.md` for the full entity list and German explanation.

---

## 📦 Files

- Automation logic: `automation/automation_v9.yaml`
- Templates:
  - `templates/sensor_ki_ladeplan.yaml`
  - `templates/sensor_steuerungsempfehlung.yaml`
  - `templates/debug_sensors.yaml`
  - `templates/sensor_restlaufzeit.yaml`
- Debug dashboard: `dashboard/debug_dashboard.yaml`

For installation details, see `docs/INSTALLATION.md` (if present) or follow `README.md`.
