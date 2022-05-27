# Stage Text
We have an API that provides a feed of the live transcription from each of the 3 stages.

You can fetch a text file of the talk so far at `https://stagetext.emfcamp.app/history/[stage]`,
or you can connect a websocket to `wss://stagetext.emfcamp.app/socket/[stage]` to get a live feed.

Replace [stage] with `a` `b` or `c` depending on the stage you are interested in.

Feeds will also be published to the EMF MQTT broker at mqtt.emf.camp on a topic TBC.

We aim to make the previous talks available soon after they have ended on this endpoint as well,
but more details will be provided.

Where a speaker has opted not to have their talk recorded, no transcription data will be
available via the API.

For questions on this API, please e-mail poc@emfcamp.org
