.. _OAuth:

Connecting to an Account
========================

PicThrive uses OAuth2 Web Server flow to allow 3rd party applications to connect to user accounts.

EndPoints
---------

* Authorize URI: https://api.picthrive.com/v1/auth/oauth2/authorize
* Access Token URI: https://api.picthrive.com/v1/auth/oauth2/token


Web Server Flow
---------------

Your Data
^^^^^^^^^
You will need to already have the following:

    * client_id

        * Your Client Id setup through PicThrive's dev portal.

    * client_secret

        * Your Client Secret as setup through PicThrive's dev portal.

    * redirect_uri

        * Your Redirect URI as setup through PicThrive's dev portal.
        * Eg. https://mysite.com/oauth/complete


Step 1: Client Login and Authorization
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Make a GET request in a new browser to the Authorize URI filling in your client data.::

    https://api.picthrive.com/v1/auth/oauth2/authorize?response_type=code&client_id=<your client id>&redirect_uri=<your redirect uri>

This will present the user with a chance to login and authorize your application. After authorizing we will perform a redirect.

Step 2: Redirect
^^^^^^^^^^^^^^^^
The browser window will be redirected to your redirect_uri::

    <your redirect uri>?code=<generated auth code>

Your server will receive this code and then perform the next step in the OAuth flow

Step 3: Server Token Exchange
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Your server will make an out-of-band request to https://api.picthrive.com/v1/auth/oauth2/token.::

    https://api.picthrive.com/v1/auth/oauth2/token?grant_type=authorization_code&client_id=<your client id>&client_secret=<your client secret>&code=<generated auth code>&redirect_uri=<your redirect uri>

We will return you a 'Bearer' Access token::

    {
        "access_token": "adsfsdfsadfs",
        "refresh_token": "asdfsadfsadf",
        "scope": "",
        "token": "adsfsdfsadfs",
        "token_type": "Bearer"
    }


