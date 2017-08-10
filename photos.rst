Photos
======

The following endpoint describes manipulations on single photo items.


/v1/Photo/<photo uuid>
----------------------

GET
~~~
Returns a single photo item.

.. _GetPhotoFields:

Returns
^^^^^^^

Fields

    * **OwnerId**: Owner UUID.
    * **GroupId**: Group UUID this photo belongs to.
    * **PhotoId**: Photo UUID.
    * **Created**: Unix time this photo entry was created at.
    * **Medium**: The URL suffix that the 'Medium' sized photo is located at.
    * **Original**: The URL suffix that the 'Original' sized photo is located at.
    * **Small**: The URL suffix that the 'Small' sized photo is located at.
    * **Watermark**: The URL suffix that the 'Medium' Watermarked photo is located at.
    * **WatermarkSmall**: The URL suffix that the 'Small' Watermarked photo is located at.
    * **Deleted**: Optional. True if this photo has been deleted.
    * **SortOrder**: The Sorting order of this photo. Tends to be the Filename of the photo.
    * **Ratio**: The size ratio of this photo (Width / Height).
    * **Region**: The region this photo is stored in. Useful to tell you how to display it. See Making Requests to find how to map regions.
    * **Hue**: The overal 'color' code of the photo.

Example Returns (200 OK)::

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
        "Ratio": 1.5,
        "Region": "picthrive-dyn"
    }

DELETE
~~~~~~
Deletes the given photo.


Returns
^^^^^^^

Fields

    * **Error**: Optional error string

Example Returns (200 OK)::

    {
        "Error": "Error string"
    }
