Playback Session Management
=============================

Playback Session Management
-----------------------------

Since the HKWHub app should be able to handle REST HTTP requests from more than one clients at the same time, the HKWHub app manages the requests with session information associated with the priority when a new playback is initiated.

The following is the policy of the session management:

Playback Session Creation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- When a client wants to start a playback, it sets the priority of the session (using ``Priority=<priority value>`` parameter).
- If Priority parameter is not specified, HKWHub app assumes it as default value, that is, 100.

Priority of Session
~~~~~~~~~~~~~~~~~~~~~
- Each session is associated with a priority value which will be used to determine which request can override the current on-going playback session.
- The priority value is specified as parameter (``Priority``) when the client calls ``play_xxx``.
	- If the command does not specify the Priority parameter, 100 is set as default value.
- If the priority of a new playback request, such as ``play_hub_media`` or ``play_web_media``, and so on, is greater than or equal to the priority of the current playback session, then it interrupts the current playback session, that is, stops the current playback session and start a new playback for itself.
	- The playback status of the interrupted session becomes ``PlayerStateStopped``. (see the related API in the next section)
	
The following diagrams show how HKWHub app handles incoming playback request based on the session priorities.

.. figure:: img/hub/session-management.png
	:alt: Session management flow diagram

Session Timeout
~~~~~~~~~~~~~~~~~
- A session becomes expired and invalid when about 60 minutes is passed since the last command was received.
- Session timer is extended (renewed) once a playback is executed successfully.
- All requests with expired session will be denied and "SessionNotFound" error returns.



----
