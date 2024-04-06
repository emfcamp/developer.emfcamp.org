---
title: Weather over MQTT
linkTitle: Weather
description: >
  Live data from EMF's own weather station.
---

There is a weather station attached to a mast at HQ.
Various sensor readings are pushed to MQTT topics listed below every ten seconds.

The broker is at `mqtt://mqtt.emf.camp`.
The prefix for each topic is `emf/weather/`.

* `runtime`: Weather station runtime in seconds.
* `tempin`: Temperature in the HQ cabin in degrees Celsius.
* `humidityin`: Humidity in the HQ cabin in %RH.
* `baromabs`: Pressure reading in millibars.
* `temp`: Outdoor temperature in degrees Celsius.
* `humidity`: Outdoor humidity in %RH (must be above 40).
* `winddir`: Wind direction in degrees.
* `windspeed`: Wind average speed in miles per hour.
* `windgust`: Wind gust speed in miles per hour.
* `maxdailygust`: Maximum daily wind gust speed in miles per hour.
* `solarradiation`: Solar radiation measured in lux.
* `uv`: Ultraviolet radiation index.
* `rainrate`: Rainfall in mm of last ten minutes x6 (hourly forecast).
* `eventrain`: Total rainfall since last zero reading in mm.
* `hourlyrain`: Total rainfall over the previous hour in mm.
* `dailyrain`: Total rainfall over the previous day in mm.
* `weeklyrain`: Total rainfall over the previous week in mm.
* `dewpoint`: Dewpoint in degrees Celsius.
* `feelslike`: [Feels like](https://blog.metoffice.gov.uk/2012/02/15/what-is-feels-like-temperature/) temperature in degrees Celsius (`temp` must be above 26.7).
* `heatindex`: [Heat index](https://www.weather.gov/ama/heatindex) in degrees Celsius (`temp` must be above 26.7).
* `solarradiation_lux`: Solar radiation in lux.
* `windchill`: [Wind chill](https://www.metoffice.gov.uk/weather/learn-about/weather/types-of-weather/wind/wind-chill-factor) (`temp` must be below 12 and `windspeed` above 3).
