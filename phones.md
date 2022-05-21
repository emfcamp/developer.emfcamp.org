# Programmable Communications

As part of the EMF Phone Operations Centre (POC) this year, we have a set of programmable
communications APIs that you can use to interact with the phone network and build your own
services.

This is based on the open source [jambonz](jambonz.org) platform, which has APIs for incoming
and outgoing calls similar to some of the other programable communications platforms on the
market. The key thing is that this one can call and be called from the EMF site phone network.

You can find the details on the API at [https://www.jambonz.org/docs/](https://www.jambonz.org/docs/)

You can configure an application at [https://jambonz.emf.camp/signup](https://jambonz.emf.camp/signup). Enter the name of your app, the URLs where you receive webhooks (see the docs) and your e-mail address. The system will create an app, allocate you a phone number in the range 555 XXXX, and e-mail you the details.

Jambonz can place calls to other EMF phones but not out to the public phone network.

If you want to test your application prior to camp, you can call it by dialling 0117 200 1500
and then entering your application number when prompted.

If you want to learn more, you can come to our workshop on Programming the EMF Phone network
with Jambonz and Node-RED - see the schedule for details. Or come visit the POC in the info
tent. You can also e-mail us questions on poc@emfcamp.org



# Low-Code Tool

We have a platform you can use to create your phone apps on or non-phone apps that interact
with the badge or any of the other APIs at EMF. 

Its provided by [FlowForge](https://flowforge.com) and allows you to run the open source
[Node-REd](https://nodered.org) tool, which privides low code programming for event driven
applications. We'll be using it in our workshop about programming the phone network.

If you would like an account, please e-mail poc@emfcamp.org as we only have limited capactity.

## Setting up a Jambonz / Flowforge / Node-Red Project
1. Log into app.emfcamp.app
1. Create a team
1. Create a project
1. Click editor at the top right
1. Click burger menu at top right
1. Select Manage Palette
1. Select the Install Tab
1. Search for Jambonz
1. Install @jambonz/node-red-contrib-jambonz
1. Confirm install warning
1. Wait about a minute, and it'll appear on the Nodes tab.

Jambonz nodes will now appear on the menu on the left.




# Stage Audio

Live audio from each of the stages is available on your phones by dialling the following numbers:

* 555 5001 - Stage A
* 555 5002 - Stage B
* 555 5003 - Stage C

Where a speaker has opted not to have their talk recorded, this feed will be unavailable.

For questions on this API, please e-mail poc@emfcamp.org
