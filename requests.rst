.. _MakingRequests:

Making Requests
===============

Auth
----

When making requests to PicThrive's servers you will need to set the **Authorization** header to the **access_token** received in the OAuth flow::

    Header:
        Authorization: Bearer <access_token>

See :ref:`OAuth` for info about obtaining an access_token


Endpoint
--------

PicThrive's Api can be accessed from::

    https://api.picthrive.com/


Displaying Images
-----------------

All image URLs returned by the API do not include the CDN hostname or protocol. The following prefix can be added to obtain access to all our hosted images and logos.::

    https://d1rj07wouwybr9.cloudfront.net/


Rate Limiting
-------------

Dev account access may be rate limited at our discretion.