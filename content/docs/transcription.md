---
title: Transcription feeds
description: >
  Real-time talk transcription feeds from stage audio.
---

The captions system provides real-time speech-to-text captioning for the three main content stages.

Audio is captured from the stage microphones via Raspberry Pi devices, streamed to a server (in the field) running WhisperLive for transcription, and distributed to display screens and user devices.

We're also aiming to serve a live stream of the presentation content (i.e. what is shown on the project screens) which can be viewed on your own device. This is subject to further testing in the field!

There is an API for the system with documentation here: https://emfcamp.github.io/camptions/

Where a speaker has opted not to have their talk recorded, live transcriptions will be paused and data will be available via the API.

The server is running 3x instances of medium.en Whisper which has performed fairly well when fed talks from 2024. It will struggle with poor audio (people not talking into microphones) and very unusual words.

PRs and general improvements welcome here: https://github.com/emfcamp/camptions (you'll note Claude helped a lot with this project!)
