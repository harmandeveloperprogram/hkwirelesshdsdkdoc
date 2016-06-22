Connecting Omni speakers to your services or devices
==============================================================================================

Overview of HWKHub
---------------------

Use Cases
~~~~~~~~~~~~

.. figure:: img/hub/hub-use-cases.png

	Use Cases: Speakers as Internet of Things 


Hub Speaker
~~~~~~~~~~~~~

To enable your Omni speakers to be controlled by other services or devices either in the Internet or in local network, one of your speakers should become a Hub Speaker. A Hub speaker is just a regular HK Omni speaker, but is configured as a Hub. A Hub device can control other HK Omni speakers when it receives a REST requests from other clients. Hub speaker needs to be connected all the time to the HKIoTCloud which actually receives HTTP request from client in the Internet. HKIoTCloud forwards the command to the Hub speaker to control other speakers as well as Hub speaker. 

In addition, to handle HTTP requests from devices in local network, not through HKIoTCloud, Hub Speaker runs a web server inside. 


HKWHub App
~~~~~~~~~~~~

HKWHub app is an iOS app that uses HKWirelessHD SDK and acts as a Hub Speaker that handles REST requests to control speakers. Like Hub Speaker, HKWHub App is always connected to HKIoTCloud and also runs a web server inside that handles HTTP requests of REST API.


Features
^^^^^^^^^
- Supports integration with cloud-based services, smart devices or sensors
	- Receives the requests from clouds (web service) outside or sensors in the house
	- Translates the requests into HKWirelessHD commands and controls the speakers based on the requests.
	- Sends response with status of speakers to the cloud if necessary 
- Central Music Playlist manager
	- Maintain user’s media list from iOS local music library or streaming services, like MixRadio, etc.
	- Maintain a collection of sound files used for IoT use cases, like door bell, etc.

Usage
^^^^^^^^
- User may place an iOS device on the cradle and run WebHub app. Then the app acts as Web Hub. (A stationary device in a house lik AppleTV can be a nice iOS device for WebHub.)


.. figure:: img/hub/hub-app.png

Overall Architecture
~~~~~~~~~~~~~~~~~~~~~~~

HKWHub App handles requests from and sends responses to sensors, smart devices or cloud-based services to control audio playback with wireless speakers in the house.

.. figure:: img/hub/architecture.png


The latest version of HKWHub app supports the following three modes:

- Cloud mode (HKIoTCloud)
	- HKWHub app communicates with HKIoTCloud to receive speaker control commands by REST API call from 3rd party services or clients.
	- HKIoTCloud handles the REST API request from any clients in the Internet. The clients can be 3rd party apps or services or devices like smartphone or sensors.
	- In this mode, any 3rd party services or clients in the Internet can reach out to HKWHub app and then control speakers and playback of audio.
	- All the 3rd party apps or services should be authorized with OAuth2 to get access token. An access token is required when 3rd party apps call the REST APIs. The detailed information about OAuth2 is available at `this link`_.
	
.. _this link: http://harmandeveloperdocs.readthedocs.org/en/latest/iOS/hkwhub-spec.html#id2

- Local Server Mode
	- HKWHub app lauches a web server internally, and then handles the REST API requests for speaker control and playback from devices, sensors or applications in the same local network. 
	- HKWHub app opens a HTTP port in the local network, so if devices or services outside of the local network want to reach out to HKWHub (and then speakers) then user needs to configure the route so that a request coming from outside can be routed to HKWHub app accordingly, such as firewall, etc.

- PubNub Cloud mode
	- HKWHub app uses PubNub API/SDK to connect to PubNub server and communicate with it to receive commands from other PubNub clients, and also sends events to other PubNub client, through a common PubNub channel.
	- By setting the same PubNub channel, any client devices or services can communicate with the HKWHub app, and then control speakers and playback of audio.
	
The following figure shows how HKWHub app handles three modes.

.. figure:: img/hub/HubAppV2.png

----
