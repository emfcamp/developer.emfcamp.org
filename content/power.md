---
title: Power data over MQTT
linkTitle: Power
description: >
  Real-time statistics from the on-site battery energy storage and diesel generators.
---

EMF 2024 has four electrical grids, each run from a large diesel-powered
generator that provides three-phase electricity.  There are also a few
[battery storage systems](https://www.nationalgrid.com/stories/energy-explained/what-is-battery-storage)
for certain backup purposes.

These systems can all put out some interesting data over MQTT, and we're
publishing those to our [MQTT broker](/mqtt/).

The broker is at `mqtt://mqtt.emf.camp` (more info on the [MQTT broker](/mqtt/)
page).

## Generator stats

The prefix for each topic is `emf/power/generator-<X>`, replacing `<X>` with
one of 'p', 'q', 'r', 's'.

* Engine status:
  * `engine_speed`: shaft rotation speed, in RPM
  * `internal_fuel_level`: fuel level (in the engine, not external tanks), units TBC
  * `battery_voltage`: battery voltage, in volts
  * `coolant_temperature`: engine coolant temperature, in degrees Celsius
  * `oil_pressure`: oil pressure, in bar (1 bar = 100 kPa)

* Overall electrical output:
  * `frequency`: output frequency on all phases, in hertz
  * `true_power`: overall real power, in watts
  * `reactive_power`: sum of phases' reactive power, in volt-amps reactive (aka VAR)
  * `apparent_power`: overall apparent power, in volt-amps (aka VA)
  * `power_factor`: overall power factor (dimensionless)

* Per-phase output (replace `N` with 1, 2 or 3):
  * `lN_voltage`: phase voltage, in volts
  * `lN_current`: phase current, in amps
  * `lN_power`: real power, in watts
  * `lN_reactive_power`: reactive power, in volt-amps reactive (aka VAR)
  * `lN_apparent_power`: apparent power, in volt-amps (aka VA)

* `earth_current`: current on the generator's earthing rod, in amps

## Battery storage stats

TBC.
