Tokens
======


/v1/Token/
----------

GET
~~~
Returns a list of generated tokens in this account. Tokens can be redeemed for photos.

Query Params
^^^^^^^^^^^^

Fields

    * **Filter**: Optional. Default 'available'. {'available', 'archived', 'used'}
    * **Count**: Optional. Search for tokens with this value on it. Use -1 for unlimited.
    * **Next**: Optional. Used to retrieve the next page of results.

Returns
^^^^^^^

Fields

    * **Tokens**: A list of tokens.

        * See :ref:`Get Token <GetToken>` for field descriptions.

    * **Next**: Used to retrieve the next page of results.
    * **Error**: Optional error string.

Example Return (200 OK)::

    {
        "Tokens": [
            {
                "OwnerId": "<owner uuid>",
                "Type": "available",
                "TypeAndModified": "available-1449791030",
                "TypeAndNum": "available-55",
                "Token": "abcde",
                "GroupId": "<group uuid>",
                "GroupToken": "pdsfs",
                "LockedToGroup": false,
                "NumPhotos": 55
            }
        ]
    }

.. _PostToken:

POST
~~~~
Creates more tokens for this Owner. Can be done in bunches. Maximum of 256 at a single time.


PostData
^^^^^^^^

Fields

    * **Count**: The number of tokens to generate.
    * **NumPhotos**: The photo value of the token. -1 means unlimited, and forces LockedToGroup to true.
    * **LockedToGroup**: If the token should be locked to be used in a single group.
    * **GroupId**: Optional. If the Token should link to this group.
    * **GroupToken**: Must be specified if and only if GroupId is specified. Must be that Group's Token/Short Id.
    * **Archive**: Optional. True if the tokens should immediately enter 'Archived' type.

Example PostData::

    {
        "Count": 10,
        "NumPhotos": 11,
        "LockedToGroup": false,
        "GroupId": "<group uuid>",
        "GroupToken": "pdsfs",
        "Archive": false
    }

Returns
^^^^^^^

Fields

    * **Tokens**: List of generated Tokens. May not match requested count.

        * See :ref:`Get Token <GetToken>` for field descriptions.

    * **Error**: Optional error string.

Example Returns (201 Created)::

    {
        "Tokens": [
            {
                "OwnerId": "<owner uuid>",
                "Type": "available",
                "TypeAndModified": "available-1449791030",
                "TypeAndNum": "available-11",
                "Token": "abcde",
                "GroupId": "<group uuid>",
                "GroupToken": "pdsfs",
                "LockedToGroup": false,
                "NumPhotos": 11
            },
            ...
        ]
    }



/v1/Token/<token>
-----------------

..  _GetToken:

GET
~~~
Returns info about the specified token.

Returns
^^^^^^^

Fields

    * **OwnerId**: Owner UUId this token belongs to.
    * **Type**: Type of token: {"available", "used", "archived"}
    * **TypeAndModified**: Type + Unix time this token was last updated.
    * **TypeAndNum**: Type + # of photos in this token.
    * **Token**: The actual token string.
    * **GroupId**: Optional. If this token is linked to a Group, this will be specified.
    * **GroupToken**: Optional. If this token is linked to a Group, this will be specified.
    * **LockedToGroup**: Optional. If set to true this token can only be used on the given Group.
    * **NumPhotos**: The number of photos this token is worth. -1 => unlimited.
    * **OrderId**: Optional. Only set if the token has been redeemed. Will specify the Order Id.

Example Return (200 OK)::

    {
        "OwnerId": "<owner uuid>",
        "Type": "available",
        "TypeAndModified": "available-1449791030",
        "TypeAndNum": "available-11",
        "Token": "abcde",
        "GroupId": "<group uuid>",
        "GroupToken": "pdsfs",
        "LockedToGroup": false,
        "NumPhotos": 11
    }


PUT
~~~
Update a token's state. A token can not legally transition out of 'used'.

PostData
^^^^^^^^

Fields

    * **OldState**: Previous state of token.
    * **NewState**: New state of token.

Example PostData::

    {
        "OldState": "available",
        "NewState": "used"
    }

Returns
^^^^^^^

Fields

    * **Error**: Optional error string.