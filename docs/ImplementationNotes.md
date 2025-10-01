# Refrigeration Cycle Simulation Notes

This repository provides a Siemens TIA Portal friendly SCL implementation of a
simple vapour-compression refrigeration cycle for the S7-1500 PLC family. The
model is intentionally lightweight and numerically safeguarded so it can run in
simulation without external libraries.

## PLC Program Structure

- `FB_RefrigerationCycle.scl` implements the primary cycle model. The block
  accepts compressor speed and ambient/evaporator temperatures, then outputs the
  estimated suction/discharge pressures, enthalpy state points, mass flow,
  cooling capacity, and COP.
- `FB_BitzerMapLoader.scl` is a placeholder to support future Bitzer polynomial
  map integration. Once coefficients are available, replace the stub state
  machine with real CSV parsing and interpolation.

## HMI Configuration

The `wincc/RefrigerationHMI.xml` file contains a minimal WinCC (Basic Runtime)
layout featuring:

- A compressor frequency gauge
- Real-time h-log(p) scatter/polyline diagram using the computed state points
- Value displays for key thermodynamic outputs
- An alarm view that decodes the diagnostic bits exposed via the function
  block's `Status` word

Import the XML into a WinCC Unified/Basic project and bind the listed tags to a
cycle instance data block (e.g., `DB_RefrigCycle`).

## Model Limitations & TODOs

- Replace placeholder thermodynamic relationships with verified property tables
  or Bitzer maps when available.
- Extend the refrigerant selector constants with property-specific coefficients.
- Implement persistence/loading of CSV polynomial maps in
  `FB_BitzerMapLoader` using TIA Portal Openness APIs or a custom parser.
- Validate the h-log(p) trend scaling against real equipment to fine-tune
  clipping thresholds and improve user feedback.
