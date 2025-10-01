# HVACR-refrigeartion-PLC

This project contains Structured Control Language (SCL) blocks and a WinCC
screen definition to simulate a simple vapour-compression refrigeration cycle on
Siemens S7-1500 hardware. The cycle accepts compressor frequency as a primary
input and produces suction pressure, enthalpy state points, COP, and derived
metrics suitable for visualisation on an h-log(p) diagram.

## Contents

| Path | Description |
| --- | --- |
| `src/FB_RefrigerationCycle.scl` | Main simulation FB with safeguarded placeholder thermodynamic model |
| `src/FB_BitzerMapLoader.scl` | Stub FB for future Bitzer polynomial map CSV parsing |
| `wincc/RefrigerationHMI.xml` | WinCC Basic/Unified screen layout with h-log(p) trend and key indicators |
| `docs/ImplementationNotes.md` | Additional integration notes and TODOs |

## Usage

1. Import the SCL files into a TIA Portal project (S7-1500 target) and create an
   instance data block (e.g., `DB_RefrigCycle`).
2. Map the WinCC tags provided in `RefrigerationHMI.xml` to the PLC instance to
   animate the HMI.
3. Adjust `Refrigerant` and temperature inputs as required for your simulation.
   The block exposes integer constants `REFRIG_R134A`, `REFRIG_R404A`, and
   `REFRIG_CO2` for the refrigerant selector.
4. Replace the placeholder calculations with Bitzer polynomial maps by
   implementing the logic inside `FB_BitzerMapLoader` and wiring its outputs into
   `FB_RefrigerationCycle`.

## Safety & Numerical Guards

The simulation clamps inputs to realistic ranges, protects divisions via minimum
denominators, and bounds pressure/logarithmic conversions to avoid runtime
faults when visualising the cycle in real time.
