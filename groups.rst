.. _GroupsPage:

Groups
======

Groups are the Owner visible arrangement of 'Albums'. A Group contains a set of photos that can be sold. Each Group has 1 parent Brand.


/v1/Group/
----------

.. _GetGroupsCall:

GET
~~~
Returns the list of Groups for an owner on the given day.

Query Params
^^^^^^^^^^^^

Fields:

    * **when**: Optional. Date string (Eg. 2015-12-31) for which day to return. Default: Today.


Returns
^^^^^^^

Fields

    * **Groups**: A list of groups for the given day.

        * See :ref:`Get Group Returns <GetGroupFields>` below for field definitions.

    * **Error**: Optional error string.

Example Returns (200 OK)::

    {
        "Groups": [
            {
                "OwnerId": "<owner uuid>",
                "GroupId": "<group uuid>",
                "Name": "Name of the group",
                "Token": "df3Gas",
                "Count": 8,
                "Thumb": "/dsfsdfas/sfsdfsf.jpg",
                "BrandId": "<brand uuid>",
                "WhenUnix": 1449791030,
                "Deleted": false,
                "LastUpdatedUnix": 1449791030
            }
        ],
        "Error": "optional error string"
    }


POST
~~~~
Creates a new Group for the owner.

PostData
^^^^^^^^

Fields

    * **Name**: Optional. Name of the group.
    * **BrandId**: Optional. Brand to associate this group with. Default: Owner's Primary Brand.
    * **WhenUnix**: Optional. What date this group should fill in. Default: Now.

PostData Example::

    {
        "Name": "name of the group",
        "BrandId": "<brand uuid>",
        "WhenUnix": 1449791030
    }

Returns
^^^^^^^

Fields

    * See :ref:`Get Group Returns <GetGroupFields>` below for field definitions.

Example Return (201 Created)::

    {
        "OwnerId": "<owner uuid>",
        "GroupId": "<group uuid>",
        "Name": "Name of the group",
        "Token": "df3Gas",
        "Count": 8,
        "Thumb": "/dsfsdfas/sfsdfsf.jpg",
        "BrandId": "<brand uuid>",
        "WhenUnix": 1449791030,
        "Deleted": false,
        "LastUpdatedUnix": 1449791030
    }


/v1/Group/Search
----------------

GET
~~~
Performs a basic search for the given group Token.

Query Params
^^^^^^^^^^^^

Fields:

    * **q**: The Token to search for.

Returns
^^^^^^^

Fields

    * **Groups**: A list of matching groups. See :ref:`Get Group Returns <GetGroupFields>` below for field definitions.
    * **Error**: Optional error string.

Example Return (200 OK)::

    {
        "Groups": [
            {
                "OwnerId": "<owner uuid>",
                "GroupId": "<group uuid>",
                "Name": "Name of the group",
                "Token": "df3Gas",
                "Count": 8,
                "Thumb": "/dsfsdfas/sfsdfsf.jpg",
                "BrandId": "<brand uuid>",
                "WhenUnix": 1449791030,
                "Deleted": false,
                "LastUpdatedUnix": 1449791030
            },
            ...
        ],
        "Error": "error string"
    }


/v1/Group/<group uuid / token>
------------------------------
Used to manipulate an individual group. Some methods require the full Group UUID, while others work with the Token or the UUID.

GET
~~~
Returns the given Group.


.. _GetGroupFields:

Returns
^^^^^^^

Fields

    * **OwnerId**: Owner UUID of this group.
    * **GroupId**: Group UUID.
    * **Name**: Name of this group.
    * **Token**: Token / Short ID of this group.
    * **Count**: Number of photos in the group.
    * **Thumb**: The URL suffix of the thumbnail of this group.
    * **BrandId**: Brand UUID that this group belongs to.
    * **WhenUnix**: Date this group was created for.
    * **Deleted**: Optional. Boolean if this group was deleted.
    * **LastUpdatedUnix**: The date this group was last updated on.

Example Return (200 OK)::

    {
        "OwnerId": "<owner uuid>",
        "GroupId": "<group uuid>",
        "Name": "Name of the group",
        "Token": "df3Gas",
        "Count": 8,
        "Thumb": "/dsfsdfas/sfsdfsf.jpg",
        "BrandId": "<brand uuid>",
        "WhenUnix": 1449791030,
        "Deleted": false,
        "LastUpdatedUnix": 1449791030
    }

PUT
~~~
Updates the given group. **Requires** that the Group UUID be provided.


PostData
^^^^^^^^

Fields

    * **Thumb**: Optional. The image thumbnail to set. If empty not changed.
    * **Name**: Optional. Name to set on the group. If empty not changed.
    * **BrandId**: Optional. The Brand to associate with this group. If empty not changed.

PostData::

    {
        "Thumb": "/dasdf/asdfdasf.jpg",
        "Name": "New name of group",
        "BrandId": "<brand uuid>"
    }

Returns
^^^^^^^

Fields

    * **Error**: Optional error string.


Example Return (200 OK)::

    {
        "Error": "Error string"
    }


DELETE
~~~~~~
Deletes the given group. **Requires** that the Group UUID be provided.

Returns
^^^^^^^

Fields

    * **Error**: Optional error string.

Example Return (200 OK)::

    {
        "Error": "Error string"
    }

/v1/Group/<group uuid>/Zip
--------------------------

POST
~~~~

Downloads the given photos from the group into a zip file. Returns a zip process. Check CloudConvert in order to determine how to check the status of the zip progress.

PostData
^^^^^^^^

Fields

    * **Photos**: List of photos to include in the zip file.

        * **File**: The URL suffix of the file to zip.
        * **FileName**: What to name this file.

Example PostData::

    {
        "Photos": [
            {
                "File": "/dsfsf/dsfafds.jpg",
                "FileName": "IMG_01.jpg"
            },
            {
                "File": "/dsfsf/hjkhkhjkh.jpg",
                "FileName": "IMG_02.jpg"
            },
            ...
        ]
    }

Returns
^^^^^^^

Fields

    * **Status**: URL to check zip progress at. Note: this is a cloudconvert url. See https://cloudconvert.com/apidoc#status
    * **Error**: Optional error string.

Example Return (201 Created)::

    {
        "Status": "<cloud convert process url>",
        "Error": "Error string"
    }


/v1/Group/<group uuid>/Photo
------------------------------------

GET
~~~
Returns all the photos inside the given group. If not authorized, then "Photos" are returned as "Public" and some fields are hidden.


Returns
^^^^^^^

Fields

    * **Photos**: A list of Photos. Returned when authorized. See :ref:`Get Photo Returns <GetPhotoFields>` for inner field definitions.
    * **Public**: A list of Photos with some data filtered out. Returned when unauthorized.
    * **Error**: Optional error string.

Example Return (200 OK)::

    {
        "Photos": [
            {
                "OwnerId": "<owner uuid>",
                "GroupId": "<group uuid>",
                "PhotoId": "<photo uuid>",
                "Created": 1449791030,
                "Medium": "/sdfadsf/asfsdaf.jpg",
                "Original": "/adfdaf/sadfsfs.jpg",
                "Small": "/sdfsaf/sdafafs.jpg",
                "Watermark": "/dfsdfsadf/sdfsaf.jpg",
                "WatermarkSmall": "/asdfsaf/sdfasf.jpg",
                "Deleted": false,
                "SortOrder": "IMG_01",
                "Ratio": 1.5
            },
            ...
        ],
        "Error": "Error string"
    }


POST
~~~~
Request to add photos to the given group. This is best done in batches of 25. The returned list provides a url for each photo to be uploaded to. **Note**: The number of requested photos may not match the number of returned items.


PostData
^^^^^^^^

Fields

    **AddPhotos**: number of photos you wish to upload. Range: 1-25 inclusive.

Example::

    {
        "AddPhotos": 12
    }



Returns
^^^^^^^

Fields

    * **AddedPhotos**: A list of upload metadata. May not match the # of photos requested.

        * **PhotoId**: The UUID of the photo.
        * **UploadUrl**: The URL to PUT the multipart upload. See :ref:`Upload Format<UploadFormat>` for how to correctly upload content.
        * **Expires**: The time at which the upload URL expires and cannot be used anymore.

    * **Error**: Optional error string.

Example Return (200 OK)::

    {
        "AddedPhotos": [
            {
                "PhotoId": "<photo uuid>",
                "UploadUrl": "https://..",
                "Expires": 1449791030
            },
            ...
        ],
        "Error": "Error string"
    }


.. _UploadFormat:

Upload Format
^^^^^^^^^^^^^

Upload data is expected in multipart format. There are 3 parts that are expected.

In the example below we are uploading for the Group "3acb836b-d45a-43e2-ba79-82f3cbf8b8d5", with the
returned photo id of "694a7a02-3c8f-427f-9aee-15a8ca1073ef".

Eg::

    ------WebKitFormBoundaryTAfxe9HopeoFTY9R
    Content-Disposition: form-data; name="GroupId"

    3acb836b-d45a-43e2-ba79-82f3cbf8b8d5
    ------WebKitFormBoundaryTAfxe9HopeoFTY9R
    Content-Disposition: form-data; name="PhotoId"

    694a7a02-3c8f-427f-9aee-15a8ca1073ef
    ------WebKitFormBoundaryTAfxe9HopeoFTY9R
    Content-Disposition: form-data; name="file"; filename="IMG_0124.JPG"
    Content-Type: application/octet-stream

    <file content goes here>