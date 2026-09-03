# SCADA Monitoring Dashboard (Ignition Perspective)

A live SCADA human-machine interface (HMI) built in Ignition Perspective that monitors a
protective relay over Modbus TCP. Reads real relay data through an OPC device connection,
displays it on a control-room-style dashboard, and raises alarms on fault conditions —
a complete field-device → Modbus → SCADA HMI system.

---

## What this project demonstrates

- Building a SCADA dashboard in **Ignition Perspective** (a leading industrial SCADA platform)
- Connecting Ignition to a live **Modbus TCP** device and mapping its registers to **OPC tags**
- Binding dashboard components to live tags, with **value scaling** and **expression-based alarm logic**
- A working HMI showing current (gauge), voltage, breaker status (color-coded indicator), and an
  overcurrent alarm — updating live as the underlying data changes

## Why it matters for P&C / SCADA engineering

SCADA systems are how substations are monitored and controlled from a control room. Configuring an
HMI — connecting to field devices, mapping their data into tags, and building operator screens with
alarms — is core SCADA integration work. This project does exactly that on a real platform (Ignition),
reading live data from a protective-relay simulation over Modbus and presenting it the way a control
room would.

---

## Architecture

```
Relay (Modbus TCP server) → Ignition Modbus device connection → OPC tags → Perspective dashboard
```

1. A Modbus TCP server (from the substation protocol gateway project) serves live relay data —
   current, voltage, and breaker status.
2. Ignition connects to it as a **Modbus TCP device**, giving Ignition access to the relay's
   registers and coil.
3. **OPC tags** map each Modbus register/coil to a named data point in Ignition (with linear scaling
   applied so the raw ×10 current value displays in real amps).
4. A **Perspective view** displays the tags: a radial gauge for current, a numeric display for
   voltage, a color-coded indicator for breaker status (green = closed, red = open/tripped), and an
   overcurrent alarm driven by an expression binding.

## What it displays

- **Current** — radial gauge, live, scaled to real amps
- **Voltage** — numeric display
- **Breaker status** — indicator that shows green when closed and red when tripped
- **Overcurrent alarm** — activates when current exceeds the pickup threshold (expression binding)

The dashboard updates live: a simulator client varies the relay's current over time, and the gauge,
indicator, and alarm respond in real time — including flipping to a fault/tripped state when the
current spikes.

---

## Key concepts applied

- **Tags** — memory tags for initial development, then OPC tags for live Modbus data
- **Bindings** — connecting component properties to tags (and using expression bindings for alarm
  logic and indicator colors)
- **Scaling** — Ignition linear scale mode to convert the raw integer register (×10) into displayed amps
- **Modbus/OPC addressing** — mapping holding registers (`HRUS`) and coils (`C`) to tags, including the
  1-based addressing and the requirement that an OPC tag specify both the item path and the OPC server

## Tools

Ignition (Inductive Automation) — Perspective, Modbus TCP driver, OPC tags · Modbus data source in Python

## Contents

- Ignition project export (`.zip`) — the importable, reproducible dashboard project
- Tag export (JSON) — the tag configuration
- Dashboard screenshots (normal and fault/tripped states)
- Modbus-connected / live-tags screenshots
- Demo video/GIF of the dashboard reacting to live data

---

## Ties to the portfolio

This dashboard consumes live data from the **substation protocol gateway** project — Ignition reads
the same Modbus registers that gateway serves, connecting the two projects into one end-to-end system:
a simulated relay serving data over Modbus, monitored on a real SCADA HMI. The overcurrent pickup used
here traces back to the short-circuit and coordination work in the pandapower and feeder-study projects.

*Part of a Protection & Control engineering portfolio.*
