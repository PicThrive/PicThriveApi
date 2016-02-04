.. _PhotoSale:

Making a Photo Sale
===================

One common integration request is to make photo sales before or after a booking. This short little guide aims to jump start that process so that you can get started faster.


Authentication
--------------

We use OAuth2 to share access to a client's account. See :ref:`Connecting to an Account<OAuth>` to get started.

See :ref:`Making Requests <MakingRequests>` to determine how to Authenticate your requests.


Making a Pre/Post Sale
----------------------

To create a photo sale you will need to create a Token. Make a POST call to :ref:`/v1/Tokens/ <PostToken>` to create a Token. This will return a unique code, eg: **DBqLVv**. This code can be used by the customer to redeem their photos.

At this point you can either email the token through our system to the client, or you can create the store link yourself.

Send Email Through Us
^^^^^^^^^^^^^^^^^^^^^

Taking the Token from above, make a POST call to :ref:`/v1/Email/ <PostEmail>`. Set 'Type' to 'token' and fill in the rest of the fields. Be sure to check the response body for the Status code of each email.

Send Email Yourself
^^^^^^^^^^^^^^^^^^^

If you wish to send the email yourself, you will need to construct the Store URL.

1. Determine the **VanityUrl** for the given Brand (See :ref:`/v1/Brands/ <GetBrands>`). Eg. 'bobs_rafting'
2. Get the Token from the previous step. Eg. 'DBqLVv'
3. Construct the basic URL::

    https://store.picthrive.com/bobs_rafting/?token=DBqLVv

This link will take them to the store front page, and the customer will be prompted to notify them that they have got X photos to redeem.


Going Further
-------------

Tokens attached to a Group
^^^^^^^^^^^^^^^^^^^^^^^^^^

Tokens can be created with knowledge of which Group they are apart of. This allows us to directly link the customer to the correct gallery, instead of to the generic storefront. To do so, you will need to retrieve the GroupId and GroupToken and use these when making the POST /v1/Token/ call.

To retrieve info on what Groups occured on which date see :ref:`/v1/Group/ <GroupsPage>`.

Now that the Token has been attached to a Group, you can either make the POST /v1/Email/ call or construct a simpler URL. If you make the POST /v1/Email/ call you will need to set the Type to 'tokenAndGroup' and include the required Group information.

If you wish to send the email yourself, you can can use either our short url link, or construct the full link. The short url looks like::

    http://ptur.co/DBqLVv

If the GroupToken is 'EAT4P' then the long url will look like::

    https://store.picthrive.com/bobs_rafting/group/EAT4P=?token=DBqLVv


Changing to Store's default Date
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you want to link the customer to the date of their trip, instead of the general storefront page, you can create the store URL like this::

    https://store.picthrive.com/bobs_rafting/date/2016-03-04?token=DBqLVv


