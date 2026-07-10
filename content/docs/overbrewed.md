---
title: Overbrewed! MQTT
linkTitle: Overbrewed!
description: >
  Live data from the Overbrewed! game in NullSector.
---

Hot coffee*, served directly from Tony's cafe.

*By "coffee" we mean game scores!

The broker is at `mqtt://mqtt.emf.camp` (more info on the [MQTT broker](/mqtt/) page).
The prefix for each topic is `overbrewed/`.

* `coffees`: Published each time a cup is served, with what was in that cup and the total number of coffees served to date.
* `scoreboard/all`: Full scoreboard.
* `scoreboard/latest`: Published each time a team completes the game, with their score.
