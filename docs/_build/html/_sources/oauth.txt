
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

