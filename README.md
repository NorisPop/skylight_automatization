# Skylight Automation Using Siemens LOGO! PLC

Design, programming, wiring, and hardware validation of an automated control system for three electric skylights using a **Siemens LOGO!** programmable logic controller (PLC).

## Goal

Automate the opening and closing of three skylights to simplify operation and improve safety. Because the installation has no limit switches, skylight travel is managed in open-loop mode using calibrated timers.

## Key Features

- **Individual control:** Independent operation of each skylight from the local control panel.
- **Global open / close:** Simultaneous opening (`I7`) or closing (`I8`) of all three skylights.
- **Scheduled auto-close:** Automatic daily closing at **14:00** using an internal Time Switch block, with no operator intervention.
- **Weather safety override:** High-priority forced closing of all skylights when the wind/rain sensor (`I5`) is triggered, overriding any active open command for as long as the signal remains active.
- **Timer-based positioning:** Actuation duration is controlled via timers calibrated from repeated travel-time measurements plus a 1–2 s safety margin.

## PLC I/O Configuration

| Address | Tag | Function |
| --- | --- | --- |
| `I1` | `BTN_DESCHID_LED_ROSU` | Open skylight 1 |
| `I2` | `BTN_TOGGLE_LED_VERDE` | Toggle (open/close) skylight 2 |
| `I3` | `BTN_TOGGLE_LED_ALBASTRU` | Toggle (open/close) skylight 3 |
| `I4` | `BTN_INCHIS_LED_ROSU` | Close skylight 1 |
| `I5` | `SENZOR_VANT` | Forced close on wind/rain (highest priority) |
| `I7` | `BTN_DESCHIS_TOATE` | Global open (all skylights) |
| `I8` | `BTN_INCHIS_TOATE` | Global close (all skylights) |
| `Q1` | `LED_ROSU` | Skylight 1 actuator output (simulated via LED) |
| `Q2` | `LED_VERDE` | Skylight 2 actuator output (simulated via LED) |
| `Q3` | `LED_ALBASTRU` | Skylight 3 actuator output (simulated via LED) |

## Control Logic

The PLC program was developed in **Siemens LOGO! Soft Comfort** using **Function Block Diagram (FBD)** with a modular structure built from:

- **Digital inputs & outputs** for operator commands, sensor input, and actuator signaling.
- **Logic gates (`OR`, `AND`)** to combine individual, global, scheduled, and safety override paths.
- **Latching relays (`RS`)** to hold command states independently of push-button press duration.
- **Timers** to limit actuator runtime to the calibrated travel duration.
- **Time Switch (`INTERVAL_ON/OFF`)** configured for the daily 14:00 auto-close sequence.

## Hardware Test Bench

- **PLC:** Siemens LOGO!
- **Power Supply:** Allen-Bradley 1606-XLE240E (24 V DC, 10 A)
- **Control & Signaling Panel:** Industrial push-buttons and illuminated indicators for individual and global controls
- **Weather Sensor Simulation:** Photoelectric sensor wired to `I5` to test the forced-close protection logic
- **Wiring:** DIN-rail modular terminal blocks with color-coded conductors (`White` = 0 V, `Orange` = 24 V, `Blue` = control signals)

## Verification & Results

The application was validated first in the **Siemens LOGO! Soft Comfort** simulator and then deployed and tested on the physical test bench. All test cases passed without conflicts between manual and automated commands:

- Individual control for skylights L1, L2, and L3
- Global open and global close commands
- Automatic closing at 14:00
- Forced closing and lock-out on `I5` sensor trigger
- Timed actuation duration

## Possible Future Improvements

- Add limit switches for closed-loop position feedback.
- Integrate a temperature sensor for automatic ventilation.
- Add an HMI panel for remote monitoring, control, and fault alarms.

## Tools & Technologies

Siemens LOGO! PLC, Siemens LOGO! Soft Comfort (FBD), 24 V DC Industrial Control Wiring, Photoelectric Sensors.


