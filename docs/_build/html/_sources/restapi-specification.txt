REST API Specification (including PubNub JSON format)
======================================================

This specification describes about the REST APIs to control HK Omni speakers and stream audio to the speakers via Hub Speaker or HKWHub app.

All the APIS are in REST API protocol.

.. Note::
	
	In this documentation, for HKIoTCloud mode, <server_host> should be "hkiotcloud.azurewebsites.net".
	For Local server mode, <server_host> should be the URL (IP address and port number) that Hub speaker or HKWHub app is showing.


.. Note::

	All the REST request should contain ``Authorization`` header that contains the access token, as described above.


Session Management
------------------

Start Session
~~~~~~~~~~~~~~~

This starts a new session. As a response, the client will receive a SessionToken. The SessionToken is required to be sent in any following requests. Note that the REST requests differs depending on the server mode.


- API: GET /api/v1/init_session
- Response
	- Returns a unique session token
	- The session token will be used for upcoming requests.
- Example:
	- Request: 
	
	.. code-block:: json
	
		curl -H "Authorization: Bearer 15c0507f3a550d7a31f7af5dc45e4dd9fd9f4bc8" 
		    http://<server_host>/api/v1/init_session

	- Response: 

	.. code-block:: json

		{"ResponseOf":"init_session","SessionToken":"r:abciKaTbUgdpQFuvYtgMm0FRh"}

			

Close Session
~~~~~~~~~~~~~~~~~

Close the session. The SessionToken information is removed from the session table.

- API: GET /api/v1/close_session?SessionToken=<session token>
- Response
	- Returns true or false indicating success or failure
- Example:
	- Request:
	
	.. code-block:: json	
	
		http://<server_host>/api/v1/close_session?SessionToken=
		    r:abciKaTbUgdpQFuvYtgMm0FRh
		
	- Response: 

	.. code-block:: json

		{"Result" : "true"}


		
----

Device Management
-------------------

Get the device count
~~~~~~~~~~~~~~~~~~~~~~~

Returns the number of speakers available in the network.

- API: GET /api/v1/device_count?SessionToken=<session token>
- Response
	- Returns the number of devices connected to the network
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/device_count?SessionToken=
		      r:abciKaTbUgdpQFuvYtgMm0FRh
		
	- Response: 

	.. code-block:: json

		{"DeviceCount":"2"}

		
----


Get the list of devices and their information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the list of speakers and their information including several status information.

- API: GET /api/v1/device_list?SessionToken=<session token>
- Response
	- Returns the list of devices with all the device information
- Example:
	- Request: 
	
	.. code-block:: json	
	
		http://<server_host>/api/v1/device_list?SessionToken=
		      r:abciKaTbUgdpQFuvYtgMm0FRh
	
	- Response: 

 .. code-block:: json

 	   {"DeviceList":
			[{"GroupName":"Bathroom", 
			"Role":21, 
			"MacAddress":"b0:38:29:1b:36:1f", 
			"WifiSignalStrength":-47, 
			"Port":44055, 
			"Active":true, 
			"DeviceName":"Adapt1", 
			"Version":"0.1.6.2", 
			"ModelName":"Omni Adapt", 
			"IPAddress":"192.168.1.40", 
			"GroupID":"3431724438", 
			"Volume":47, 
			"IsPlaying":false, 
			"DeviceID":"34317244381360"
			},
		{"GroupName":"Temp", 
			"Role":21, 
			"MacAddress":"b0:38:29:1b:9e:75", 
			"WifiSignalStrength":-53, 
			"Port":44055, 
			"Active":true, 
			"DeviceName":"Adapt", 
			"Version":"0.1.6.2", 
			"ModelName":"Omni Adapt", 
			"IPAddress":"192.168.1.39", 
			"GroupID":"1293219209", 
			"Volume":47, 
			"IsPlaying":false, 
			"DeviceID":"129321920968880"
			}]
		}
	

----

Get the Device Information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Gets the device information of a particular device (speaker) identified by DeviceID.

- API: GET /api/v1/device_info?SessionToken=<session token>&DeviceID=<device id>
- Response
	- Returns the information of the device
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host>/api/v1/device_info?SessionToken=
		       r:abciKaTbUgdpQFuvYtgMm0FRh&DeviceID=129321920968880

	- Response: 

	.. code-block:: json

		{"GroupName":"Temp", 
		"Role":21, 
		"MacAddress":"b0:38:29:1b:9e:75", 
		"WifiSignalStrength":-52, 
		"Port":44055, 
		"Active":true, 
		"DeviceName":"Adapt", 
		"Version":"0.1.6.2", 
		"ModelName":"Omni Adapt", 
		"IPAddress":"192.168.1.39", 
		"GroupID":"1293219209", 
		"Volume":47, 
		"IsPlaying":true, 
		"DeviceID":"129321920968880"}

	
----

Add a Device to Session
~~~~~~~~~~~~~~~~~~~~~~~~~

Add a speaker to playback session. Once a speaker is added, then the speaker will play the music. There is no impact of this call to other speakers.

- API: GET /api/v1/add_device_to_session?SessionToken=<session token>&DeviceID=<device id>
- Response
	- Returns true or false
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host>/api/v1/add_device_to_session?SessionToken=
		       r:abciKaTbUgdpQFuvYtgMm0FRh&DeviceID=129321920968880

	- Response: 

	.. code-block:: json

		{"Result":"true"}

	
----

Remove a Device from Session
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Removes a speaker from playback session. Once a speaker is removed, then the speaker will not play the music. There is no impact of this call to other speakers.

- API: GET /api/v1/remove_device_from_session?SessionToken=<session token>&DeviceID=<device id>
- Response
	- Returns true or false
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/remove_device_from_session?SessionToken=
		       r:abciKaTbUgdpQFuvYtgMm0FRh&DeviceID=129321920968880
		
	- Response: 
	
	.. code-block:: json

		{"Result":"true"}
		
----

Set party mode
~~~~~~~~~~~~~~~~~~

Addes all speakers to playback session. Once it is done, all speakers will play music.

- API: GET /api/v1/set_party_mode?SessionToken=<session token>
- Response
	- Returns true or false
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/set_party_mode?SessionToken=r:abciKaTbUgdpQFuvYtgMm0F

	- Response: 
	
	.. code-block:: json

		{"Result":"true"}

		
----

Media Playback Management
----------------------------

Get the list of media item in the Media List of the HKWHub app
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the list of media items added to the Media List of the app. User can add music items to the **Media List** of the app via **Setting** of the app.

.. Note::

	A music item downloaded from Apple Music is not supported. The music file from Apple music is DRM-enabled, and cannot be played with HKWirelessHD. Only music items purchased from iTunes Music or added from user's own library are supported.

	To be added to the Media List, the music item must be located locally on the device. No streaming from iTunes or Apple Music are supported.


- API: GET /api/v1/media_list?SessionToken=<session token>
- Response
	- Returns JSON of the list of store media in the HKWHub app.
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/media_list?SessionToken=r:abciKaTbUgdpQFuvYtgMm0F
		
	- Response: 

	.. code-block:: json

		{"MediaList": [
			{"PersistentID":"7387446959931482519",
			"Title":"I Will Run To You",
			"Artist":"Hillsong",
			"Duration":436,
			"AlbumTitle":"Simply Worship"
		},
			{"PersistentID":"5829171347867182746",
			"Title":"I'm Yours [ORIGINAL DEMO]",
			"Artist":"Jason Mraz",
			"Duration":257,
			"AlbumTitle":"Wordplay [SINGLE EP]"}
			]}

	
----

Play a song in the Media List of the HKWHub app
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plays a song in the Media List of the Hub app. Each music item is identified with MPMediaItem's PersistentID. It is a unique ID to identify a song in the iOS Music library.

.. note::

	``play_hub_media`` does not specify speakers to play. It just uses the current session setting. If there is no speaker in the current session, then the play fails.

- API: GET /api/v1/play_hub_media?SessionToken=<session token>&PersistentID=<persistent id>
- Response
	- Play a song stored in the hub, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host>/api/v1/play_hub_media?SessionToken=
		           r:abciKaTbUgdpQFuvYtgMm0F&PersistentID=7387446959931482519

	- Response: 

	.. code-block:: json

		{"Result":"true"}
		
----

Play a song in the Media list as party mode
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plays a song in the Media List with all speakers available. So, regardless of current session setting, this command play a song to all speakers.

- API: GET /api/v1/play_hub_media_party_mode?SessionToken=<session token>&PersistentID=<persistent id>
- Response
	- Play a song in the hub's media list to all speakers, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json 
		
		http://<server_host>/api/v1/play_hub_media_party_mode?SessionToken=
		           r:abciKaTbUgdpQFuvYtgMm0F&PersistentID=7387446959931482519
		
	- Response: 

	.. code-block:: json

		{"Result":"true"}
		

----

Play a song in the Media list with selected speakers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plays a song in the Media List with selected speakers. The selected speakers are represented in ``DeviceIDList`` parameter as a list of ``DeviceID`` separated by ",".

- API: GET /api/v1/play_hub_media_selected_speakers?SessionToken=<session token>&PersistentID=<persistent id>&DeviceIDList=<xxx,xxx,...>
- Response
	- Play a song in the hub's media list to selected speakers, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host>/api/v1/play_hub_media_selected_speakers?SessionToken=
		            r:abciKaTbUgdpQFuvYtgMm0F&PersistentID=7387446959931482519&
					DeviceIDList=34317244381360,129321920968880

	- Response: 

	.. code-block:: json

		{"Result":"true"}

		
----

Play a Song from Web Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plays a song from Web (http:) or rstp (rstp:) or mms (mms:) server. The URL of the song to play is specified by ``MediaUrl`` parameter.

.. note::

	``play_web_media`` does not specify speakers to play. It just uses the current session setting. If there is no speaker in the current session, then the play fails.
	
.. note::

	``play_web_media`` cannot be resumed. If it is paused by calling ``pause``, then it just stops playing music, and cannot resume.
	
	
- API: GET /api/v1/play_web_media?SessionToken=<session token>&MediaUrl=<URL of the song>
- Response
	- Play a song from HTTP server, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host_name>/api/v1/play_web_media?SessionToken=
		          r:abciKaTbUgdpQFuvYtgMm0F&MediaUrl=http://seonman.github.io/music/hyolyn.mp3
			
	- Response: 

	.. code-block:: json

		{"Result":"true"}

.. Note::
	This API call takes several hundreds millisecond to return the response.

		
----

Play a Song from Web Server as party mode
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plays a song from Web server with all speakers. The URL of the song to play is specified by ``MediaUrl`` parameter.

.. note::

	``play_web_media`` cannot be resumed. If it is paused by calling ``pause``, then it just stops playing music, and cannot resume.
	

- API: GET /api/v1/play_web_media_party_mode?SessionToken=<session token>&MediaUrl=<URL of the song>
- Response
	- Play a song from HTTP server to all speakers, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host>/api/v1/play_web_media_party_mode?SessionToken=
		         r:abciKaTbUgdpQFuvYtgMm0F&MediaUrl=http://seonman.github.io/music/hyolyn.mp3
			
	- Response: 

	.. code-block:: json

		{"Result":"true"}

.. Note::
	This API call takes several hundreds millisecond to return the response.
	
	
----

Play a Song from Web Server with selected speakers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plays a song from Web server with selected speakers. The URL of the song to play is specified by ``MediaUrl`` parameter. The selected speakers are represented in ``DeviceIDList`` parameter as a list of ``DeviceID`` separated by ",".

.. note::

	``play_web_media`` cannot be resumed. If it is paused by calling ``pause``, then it just stops playing music, and cannot resume.

- API: GET /api/v1/play_web_media_selected_speakers?SessionToken=<session Token>&MediaUrl=<URL of the song>&DeviceIDList=<xxx,xxx,...>
- Response
	- Play a song from HTTP server to selected speakers, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host>/api/v1/play_web_media_selected_speakers?SessionToken=
		         r:abciKaTbUgdpQFuvYtgMm0F&MediaUrl=http://seonman.github.io/music/hyolyn.mp3&
				 DeviceIDList=34317244381360,129321920968880

	- Response: 

	.. code-block:: json

		{"Result":"true"}

.. Note::
	This API call takes several hundreds millisecond to return the response.
	
----


Play TTS (Text-to-Speech)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plays a Text-to-Speech audio from VoiceRRS server. The Text to play is specified by ``Text`` parameter.

.. note::

	In order to use APIs for playing TTS (Text-To-Speech), you need to set VoiceRRS Application key on the setting menu of HKWHub App. You can go to the `VoiceRRS`_ web site to get your application key.

.. _`VoiceRRS`: http://www.voicerss.org/

.. note::

	``play_tts`` does not specify speakers to play. It just uses the current session setting. If there is no speaker in the current session, then the play fails.
	
.. note::

	``play_tts`` cannot be resumed. If it is paused by calling ``pause``, then it just stops playing music, and cannot resume.
	
	
- API: GET /api/v1/play_tts?SessionToken=<session token>&Text=<Text>
- Response
	- Play TTS audio, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host_name>/api/v1/play_tts?SessionToken=r:abciKaTbUgdpQFuvYtgMm0F&Text="Hello World. How are you today?"
			
	- Response: 

	.. code-block:: json

		{"Result":"true"}

.. Note::
	This API call takes more than several hundreds millisecond to return the response, depending on the network condition.

		
----

Play TTS (Text-to-Speech) as party mode
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plays a Text-to-Speech audio from VoiceRRS server with all speakers. The Text to play is specified by ``Text`` parameter.	

- API: GET /api/v1/play_tts_party_mode?SessionToken=<session token>&Text=<Text>
- Response
	- Play TTS audio to all speakers, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host>/api/v1/play_tts_party_mode?SessionToken=r:abciKaTbUgdpQFuvYtgMm0F&Text="Hello World. How are you today?"
			
	- Response: 

	.. code-block:: json

		{"Result":"true"}

.. Note::
	This API call takes several hundreds millisecond to return the response.
	
	
----

Play a Song from Web Server with selected speakers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plays a Text-to-Speech audio from VoiceRRS server with selected speakers. The Text to play is specified by ``Text`` parameter. The selected speakers are represented in ``DeviceIDList`` parameter as a list of ``DeviceID`` separated by ",".

- API: GET /api/v1/play_tts_selected_speakers?SessionToken=<Session Token>&Text=<Text>&DeviceIDList=<xxx,xxx,...>
- Response
	- Play TTS from VoiceRSS server to selected speakers, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host>/api/v1/play_tts_selected_speakers?SessionToken=
		      r:abciKaTbUgdpQFuvYtgMm0F&Text="Hello World. How are you today?"&
			  DeviceIDList=34317244381360,129321920968880

	- Response: 

	.. code-block:: json

		{"Result":"true"}

.. Note::
	This API call takes several hundreds millisecond to return the response.
	
	
----


Pause the Current Playback
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Pauses the current playback. The client can resue the playback by ``resume_hub_media``.

- API: GET /api/v1/pause_play?SessionToken=<session token>
- Response
	- Pause the current playback, and then return true or false.
	- It can resume the current playback by calling ``resume_hub_media`` if and only if the playback is playing hub media. ``play_web_media`` cannot be resumed once it is paused or stopped.
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/pause_play?SessionToken=r:abciKaTbUgdpQFuvYtgMm0F

	- Response: 

	.. code-block:: json

		{"Result":"true"}

	
----

Resume the Current Playback with Hub Media
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- API: GET /api/v1/resume_hub_media?SessionToken=<session token>&PersistentID=<persistent id>
- Response
	- Resume the current playback with Hub Media, and then return true or false.
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/resume_hub_media?SessionToken=
		       r:abciKaTbUgdpQFuvYtgMm0F&PersistentID=7387446959931482519
		
	- Response: 

	.. code-block:: json

		{"Result":"true"}

		
----

Resume the Current Playback with Hub Media as Party Mode
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- API: GET /api/v1/resume_hub_media_party_mode?SessionToken=<session token>&PersistentID=<persistent id>
- Response
	- Resume the current playback with Hub Media with all speakers, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json
	
		http://<server_host>/api/v1/resume_hub_media_party_mode?SessionToken=
		          r:abciKaTbUgdpQFuvYtgMm0F&PersistentID=7387446959931482519

	- Response: 

	.. code-block:: json

		{"Result":"true"}

		
----

Resume the Current Playback with Hub Media with selected speakers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- API: GET /api/v1/resume_hub_media_selected_speakers?SessionToken=<session token>&PersistentID=<persistent id>&DeviceIDList=<xxx,xxx,...>
- Response
	- Resume the current playback with Hub Media with selected speakers, and then return true or false.
- Example:
	- Request:
	
	.. code-block:: json

		http://<server_host>/api/v1/resume_hub_media_selected_speakers?SessionToken=
		         r:abciKaTbUgdpQFuvYtgMm0F&PersistentID=7387446959931482519&
				 DeviceIDList=34317244381360,129321920968880

	- Response: 

	.. code-block:: json

		{"Result":"true"}

----

Stop the Current Playback
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- API: GET /api/v1/stop_play?SessionToken=<session token>
- Response
	- Stop the current playback with Hub Media, and then return true or false.
	- If the playback has stopped, then it cannot resume.
- Example:

	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/stop_play?SessionToken=r:abciKaTbUgdpQFuvYtgMm0F
		
	- Response: 

	.. code-block:: json

		{"Result":"true"}

----

Get the Playback Status (Current Playback State and Elapsed Time)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- API: GET /api/v1/playback_status?SessionToken=<session token>
- Response
	- It returns the current state of the playback and also return the elapsed time (in second) of the playback.
	- If it is not playing, then the elapsed time is (-1)
	- The following is the value of each playback state:
		- PlayerStatePlaying : Now playing audio
		- PlayerStatePaused : Playing is paused. It can resume.
		- PlayerStateStopped : Playing is stopped. It cannot resume.

	- Note that if the playback has stopped, then it cannot resume.
	- Developers need to check the playback status during the playback to handle any possible exceptional cases like interruption or errors. We recommedn to call this API every second.
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/playback_status?SessionToken=r:abciKaTbUgdpQFuvYtgMm0F
		
	- Response: 

	.. code-block:: json

		{"PlaybackState":"PlayerStatePlaying",
		 "TimeElapsed":"15"}
		 

----

Check if the Hub is playing audio
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- API: GET /api/v1/is_playing?SessionToken=<session token>
- Response
	- Returns true (playing) or false (not playing)
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/is_playing?SessionToken=r:abciKaTbUgdpQFuvYtgMm0F
		
	- Response: 

	.. code-block:: json

		{"IsPlaying":"true"}

		
		
Volume Control
-----------------

Get Volume for all Devices
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- API: GET /api/v1/get_volume?SessionToken=<session token>
- Response
	- Returns the average volume of all devices.
	- The range of volume is 0 (muted) to 50 (max)
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/get_volume?SessionToken=r:abciKaTbUgdpQFuvYtgMm0F
		
	- Response: 

	.. code-block:: json

		{"Volume":"10"}

		
----

Get Volume for a particular device
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- API: GET /api/v1/get_volume_device?SessionToken=<session token>&DeviceID=<device id>
- Response
	- Returns the  volume of a particular device
	- The range of volume is 0 (muted) to 50 (max)
- Example:
	- Request: 
	
	.. code-block:: json

		http://<server_host>/api/v1/get_volume_device?SessionToken=
		             r:abciKaTbUgdpQFuvYtgMm0F&DeviceID=1234567
		
	- Response: 

	.. code-block:: json

		{"Volume":"10"}


----

Set Volume for all devices
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- API: GET /api/v1/set_volume?SessionToken=<session token>&Volume=<volume>
- Response
	- Returns true or false
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/set_volume?SessionToken=r:abciKaTbUgdpQFuvYtgMm0F&Volume=10
		
	- Response: 

	.. code-block:: json

		{"Result":"true"}

		
----

Set Volume for a particular device
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- API: GET /api/v1/set_volume_device?SessionToken=<session token>&DeviceID=<device id>&Volume=<volume>
- Response
	- Returns true or false
- Example:
	- Request: 
	
	.. code-block:: json
	
		http://<server_host>/api/v1/set_volume_device?SessionToken=
		            r:abciKaTbUgdpQFuvYtgMm0F&DeviceID=1234567&Volume=10
		
	- Response: 

	.. code-block:: json

		{"Result":"true"}


----
		


OAuth2 Authorization API Specification
-------------------------------------------------------

Introduction
~~~~~~~~~~~~~~~~

In order to access the HKIoTCloud REST APIs to control Omni speakers, your HKIoTCloud-enabled product needs to obtain a HKIoTCloud access token that grans access to the APIs on behalf of the product's user.

.. NOTE::

	Please refer `OAuth 2.0 Getting Started in Web-API Security by Matthias Biehl`_ for your more understanding on OAuth2.
	
.. _OAuth 2.0 Getting Started in Web-API Security by Matthias Biehl: http://www.amazon.com/OAuth-2-0-Getting-Security-University/dp/1507800916/ref=tmm_pap_swatch_0?_encoding=UTF8&qid=1454629444&sr=8-1
	
The workflow for obtaining and using an access token is as follows:

1. The user visits your product registration website and enters information about their specific instance of your product.
2. Your website creates a HKIoTCloud consent request using the user-supplied registration information and forwards the user to the HKIoTCloud website.
3. The user logs in to HKIoTCloud.
4. The user authorizes their instance of your product to be used with HKIoTCloud on their behalf.
5. HKIoTCloud returns an access token to your product registration website.
6. Your product registration website securely transfers the access token to the user's specifi instance of your product.
7. The user's speific instance of your product uses the access token to make HKIoTCloud API calls.

Types of Authorization
~~~~~~~~~~~~~~~~~~~~~~~~~

HKIoTCloud supports two types of authorization:


- Authorization Code Grant - Send a client ID and a client secret to get an access token and a refresh token.

- Password Grant - Send username and password along with client ID and client secret to get an access token and refresh token

.. note::

	If you are able to implement server-side scripting, then using authorization code grant is recommended. If you are not able to implement server-side scripting, then using password grant is your choice.
	
.. Note::

	You must generate a new access token every hour, that is, expiration is set to 3,600 seconds. You can use refresh token in conjunction with your client ID and client secret to obtain a new access token without your user having to re-authenticate.
	

Using the Password Grant Type
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To obtain an access token (and a refresh token) with password grant, you should **POST** to ``/oauth/token``. You should include your client ID and client secret in the ``Authorization`` header by combining them with a colon ":" and then encoding in Base64. That is, ``Base64(client_id:client_secret)``. And also, you should include ``grant_type: password``, username and passworkd in the request body.

**Sample Request:**

.. code::

	POST /oauth/token HTTP/1.1
	Host: hkiotcloud.herokuapp.com
	Authorization: Basic RkZjUE9iS2h4OThvNXhtMzpjcENZQ1BrUjA4NXFSR3hFempDMUlGeEoxQWRhZFQ=
	Content-Type: application/x-www-form-urlencoded  

	grant_type=password&username=johndoe&password=A3ddj3w 

Here, ``RkZjUE9iS2h4OThvNXhtMzpjcENZQ1BrUjA4NXFSR3hFempDMUlGeEoxQWRhZFQ=`` is the result of Base64 encoding of clientId:clientSecret.

**Sample Response:**

.. code::

	HTTP/1.1 200 OK
	Content-Type: application/json;charset=UTF-8
	Cache-Control: no-store
	Pragma: no-cache
 
	{
	   "access_token":"62b8c11cfa0840b506230cb8af747230052775e1",
	   "token_type":"bearer",
	   "expires_in":3600,
	   "refresh_token":"7a7687b6b32247573b366d5bf2eeb707ba0a1b4d"
	 }



Creating a Consent Request
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By creating a consent request, your user will be redirected to the HKIoTCloud website where they can enter their HKIoTCloud credentials in order to authorize their devices of your product to be used with the HKIoTCloud service.

The consent request is constructed as follows:

- Redirect the user to HKIoTCloud at https://hkiotcloud.herokuapp.com/oauth/token with the following URL-encoded query parameters:
	- ``client_id`` : The client ID of your application. This information can be found on the HKIoTCloud website.
	- ``response_type``: ``code`` for authorization code grant.
	- ``redirect_uri`` : Specifies the return URI that you added to your app's  profile when signing up.

**Sample Request:**

Send as GET request.

.. code-block:: json

	https://hkiotcloud.herokuapp.com/oauth/authorize?response_type=code&
	          client_id=n7HhiTnKYjJd4zmM&redirect_uri=https://your.app.com/oauthCallbackHKIoTCloud


HKIoTCloud Returns a Response to Your Registration Website
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After the user is authenticated, the user is redirected to the URI that you provided in the ``redirect_uri`` parameter of the request.

The response includes an authorization code.

**Sample Authorizatino Code Grant Response:**

.. code-block:: json

	https://your.app.com/oauthCallbackHKIoTCloud?code=0b368d49809048dd7424d6f7fd869a98f2372859


Next, your service leverages the returned authorization code to ask for an access token:

- Send a **POST** request to https://hkiotcloud.herokuapp.com/oauth/token with the following parameters:

**HTTP Header Parameters:**

- ``Content-Type: application/x-www-form-urlencoded``

**HTTP Body Parameters:**

- ``grant_type: authorization_code``
- ``code`` : The authorization code that was returned in the response.
- ``client_id`` : Your application's client ID. This information can be found on the HKIoTCloud website.
- ``client_secret`` : The application's client secret. This information can be found on the HKIoTCloud website.
- ``redirect_uri`` : The return URI that you added to your app's profile when signing up.

**Sample Request:**

.. code-block:: json

	POST /oauth/token HTTP/1.1
	Host: hkiotcloud.herokuapp.com
	Content-Type: application/x-www-form-urlencoded
	Cache-Control: no-cache
 
	grant_type=authorization_code&code=2b3711911f4f2263e785eeda386046ccc8da6aee&
	    client_id=n7HhiTnKYjJd4zmM&client_secret=ANRfB9z94xtcxFGXrd5XHXEiKg43UY
		&redirect_uri=https://hkvoicecloud.herokuapp.com/oauthCallbackHKIoTCloud


**Sample Response:**

.. code-block:: json

	{
		"access_token": "902da699ed1d5d511bd750366889f3260c2015b4",
		"expires_in": 3600,
		"refresh_token": "5defcb0a9a49ac9b2403b8c78600638238d81011",
		"token_type": "bearer"
	}	


Transfer the access and refresh tokens to the user's product.

.. NOTE::
	
	Currently, a refresh token is valid for one year, while an access token is valid only an hour and an authorization code is valid only a minute.



Using the Access Token to Make HKIoTCloud API Calls
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When you call the HKIoTCloud API calls, pass the value of the access token into the request header. Specifically, create an ``Authorization`` header and give it the value ``Bearer <access token>``.

**Sample Request using curl:**

- curl -X GET -H "Authorization: Bearer 15c0507f3a550d7a31f7af5dc45e4dd9fd9f4bc8" http://hkiotcloud.herokuapp.com/api/v1/init_session


Getting a New Access Token with Refresh Token
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The access token is valid for one hour. When the access token expires or is about to expire, you can exchange the refresh token for new access and refresh tokens.

- Send a ``POST`` request to ``https://hkiotcloud.herokuapp.com/oauth/token`` with the following parameters:

**HTTP Header Parameters:**

- ``Content-Type: application/x-www-form-urlencoded``

**HTTP Body Parameters:**

- ``grant_type: refresh_token``
- ``refresh_token`` : The refresh token returned with the last request for a new access token.
- ``client_id`` : Your application's client ID. This information can be found on the HKIoTCloud website.
- ``client_secret`` : The application's client secret. This information can be found on the HKIoTCloud website.

**Sample Request:**

.. code-block:: json

	POST /oauth/token HTTP/1.1
	Host: hkiotcloud.herukuapp.com
	Content-Type: application/x-www-form-urlencoded
	Cache-Control: no-cache
 
	grant_type=refresh_token&refresh_token=5defcb0a9a49ac9b2403b8c78600638238d81011&
	client_id=n7HhiTnKYjJd4zmM&client_secret=ANRfB9z94xtcxFGXrd5XHXEiKg43UY


**Sample Response:**

.. code-block:: json

	HTTP/1.1 200 OK
	 
	{
		"access_token": "90da03bdceb15cf75d99ff99715ce87b29602651",
		"expires_in": 3600,
		"refresh_token": "6a762dfce9146dbf149f881c5aa15fc6cfdf1fd0",
		"token_type": "bearer"
	}

