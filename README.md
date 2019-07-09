# NWS-Alerts-Custom-Package
## This package contains the code required to provide severe weather alerts from the US National Weather Service

This will allow several functions including a persistent pop-up notification on your HA front end, a notification to whichever notify service you use (I’m using Pushbullet in this example), and, if you use another custom component (found here: https://community.home-assistant.io/t/echo-devices-alexa-as-media-player-testers-needed/58639), it will also trigger an announcement to all of your Echo devices.

This integration uses a custom sensor.

you can find the code and the instructions for use at the following location:

https://github.com/eracknaphobia/nws_custom_component

I made some minor modifications to the code there:

I modified the update interval to 1 minute. (EDIT: I just noticed the latest version already has this set to a one minute update interval so disregard this if you have the latest version)

I also wanted to be able to split out the title, display_desc, and spoken_desc for the times that there are multiple alerts so that I will only get updates on the changes between them, not the entire message every time there is an update.

Here are the lines in the nws_alerts.py file to accomplish that (changes in bold):

Line 21:
MIN_TIME_BETWEEN_UPDATES = timedelta(minutes=1)

Line 149:
display_desc += ‘\n\n-\n\n’

Line 160:
spoken_desc += ‘\n\n-\n\n’

Line 162:
spoken_desc += ‘\n\n-\n\n’

And I only want to get a notification if an alert is added (count increases) but not if one is removed.

You next need to find either your NWS Zone ID or County ID. It is better to use both ID's here but if you onbly use one then the Zone ID is best since it will provide an alert that is more targeted to you exact location

You can find your Zone or County ID by going to https://alerts.weather.gov/, scroll down to your state and click on the “zone list” and/or "county list" then look for the entry for your county.

Once you have your desired ID you create a sensor, replacing my id (INZ009, INC033) with yours. The ID is case sensitive!

Then when you have your ID's, enter them into the configuration.

If you use the optional "name:" configuration entry then you will need to adjust your sensor entity_id in the automations and scripts to reflect the new sensor information.

See the nws_alerts_custom_package.yaml file for all the sections required in the configuration.yaml or, alternatively, if you use packages in Home Assistant you can copy the entire file into your packages location and use it as-is.

## Notes:

I created two announcements that target different announcing systems so that a minor event won't wake you up in the middle of the night.

You’ll notice I repeat the announcement twice in case the first announcement doesn’t wake you up.

The script above generates a persistent notification pop-up.

3/15/2019 - updated to use the updated code for the alexa media player TTS custom component.
