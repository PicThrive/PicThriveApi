Purchase
========

Purchase provides an endpoint to completing transactions through the PicThrive network.

/v1/Purchase/
-------------

POST
~~~~

Create a purchase through Stripe or via Tokens. See `Stripe.js <https://stripe.com/docs/stripe.js>`_ to on how to collect Payment through Stripe. Use our Stripe Publishable Key::

    pk_live_kqt5cDDw5Dvaqq0RdtThcyl8


..  _makePurchase:

PostData
^^^^^^^^

Fields

    * **Groups**: A map of which GroupId's.

        * **key**: Map of Photo Id's that are selected

            * **value**: Boolean, always set to true.

    * **Total**:

        * **NumPaid**: Number of photos paid for through Tokens.
        * **NumToPurchase**: Number of photos need to be paid via Stripe.
        * **NumPhotos**: Total number of photos being purchased.
        * **Groups**: List of Group details

            * **GroupId**: Id of a Group with photos.
            * **NumPaid**: Number of photos in this Group paid for through Tokens.
            * **NumToPurchase**: Number of photos in this Group that need to be paid via Stripe.
            * **NumPhotos**: Total number of photos being purchased from this Group.

    * **TotalCents**: Total cost in Cents to charge through Stripe.
    * **Email**: The email to send the order to.
    * **Stripe**: Optional. The Stripe Token as returned by `Stripe.js <https://stripe.com/docs/stripe.js>`_ Only used when a Stripe purchase must be made.
    * **Tokens**: List of Token Details that were used in this Purchase. See :ref:`Get Token <GetToken>` for more details about these fields.

        * **Token**: The actual Token code.
        * **OwnerId**: The Owner UUID of this token.
        * **GroupId**: The Group UUID this token is locked to. Leave empty if the Token is not locked to a Group.
        * **LockedToGroup**: Boolean. True if the token is locked to a Group.
        * **NumPhotos**: The number of photos this token can redeem.
        * **Type**: They type of token.

    * **OwnerId**: The Owner UUID this purchase is taking place in.
    * **BrandId**: The Brand UUID this purchase is taking place in.
    * **Retry**: Optional. Array of string codes. Filled in by the Returned 'Retry' key.

Example PostData::

    {
        "Groups": {
            "<group uuid>": {
                "<photo uuid>": true,
                "<photo uuid>": true,
                ...
            },
            ...
        },
        "Total": {
            "NumPaid": 5
            "NumToPurchase": 7
            "NumPhotos": 12
            "Groups": [
                {
                    "GroupId": "<group uuid>",
                    "NumPaid": 5
                    "NumToPurchase": 1
                    "NumPhotos": 6
                },
                ...
            ]
        },
        "TotalCents": 1500,
        "Email": "example@example.com",
        "Stripe": "<stripe token>",
        "Tokens": [
            {
                "Token": "sdfds",
                "OwnerId": "<owner uuid>",
                "GroupId": "<group uuid>",
                "LockedToGroup": true,
                "NumPhotos": 5,
                "Type": "available"
            },
            ...
        ],
        "OwnerId": "<owner uuid>",
        "BrandId": "<brand uuid>",
        "Retry": [
            "adfsf...",
            "gdfgdfg...",
            "gfhgfh..."
        ]
    }

Returns
^^^^^^^

Fields

    * **ChargeId**: Optional. Return on success fail Stripe purchase. This is their Charge / Purchase Id.
    * **Cc**: Optional. Displayed Credit Card to show what card was charged. Eg: "Visa ending in 4242".
    * **ChargedAmount**: The amount, in cents, that was charged to their credit card.
    * **Retry**: Optional. Returned on purchase failure. To be used when retrying a failed purchase.
    * **OrderId**: Optional. Returned only on success full 'payment'. Returns the UUID to their order page. Eg. https://order.picthrive.com/<order uuid>
    * **Error**: Optional. Error string on why the purchase failed.
    * **EmailError**: Optional. Present if the purchase was completed, but we failed to send out an email.
    * **NumPhotos**: The number of photos in the purchase/order.
    * **Email**: The email we sent their receipt to.

Example Return (201 Created)::

    {
        "ChargeId": "sdfsdfsfsdf",
        "Cc": "Visa ending in 4242",
        "ChargedAmount": 1500
        "Retry": [],
        "OrderId": "<order uuid>",
        "Error": "optional error string",
        "EmailError": "",
        "NumPhotos": 12,
        "Email": "example@example.com"
    }


/v1/Purchase/Pending/
---------------------

POST
~~~~

Create a Pending Purchase that allows for payment to be done instore through their own POS. The PicThrive Owner will be charge as if a Token was used.

PostData
^^^^^^^^

Fields

    * **Groups**: See :ref:`Make Purchase <makePurchase>` for this fields description.
    * **TotalCents**: See :ref:`Make Purchase <makePurchase>` for this fields description.
    * **Email**: See :ref:`Make Purchase <makePurchase>` for this fields description.
    * **OwnerId**: See :ref:`Make Purchase <makePurchase>` for this fields description.
    * **BrandId**: See :ref:`Make Purchase <makePurchase>` for this fields description.
    * **Retry**: See :ref:`Make Purchase <makePurchase>` for this fields description.

Example PostData::

    {
        "Groups": {
            "<group uuid>": {
                "<photo uuid>": true,
                "<photo uuid>": true,
                ...
            },
            ...
        },
        "TotalCents": 1500,
        "Email": "example@example.com",
        "OwnerId": "<owner uuid>",
        "BrandId": "<brand uuid>",
        "Retry": []
    }

Returns
^^^^^^^

Fields

    * **Retry**: Optional. Returned on purchase failure. To be used when retrying a failed purchase.
    * **PendingOrderId**: The unique id of this Pending Order.

Example Returns (201 Created)::

    {
        "Retry": [],
        "PendingOrderId": "safsdfs"
    }


GET
~~~

Returns a list of Pending Purchases.

QueryParams
^^^^^^^^^^^

Fields

    * **BrandId**: Optional. BrandId to only return.

Returns
^^^^^^^

Fields

    * **Orders**: List of Pending Purchases.

        * **OwnerId**: Owner UUID.
        * **BrandId**: Brand UUID.
        * **GroupId**: List of Group UUIDs this purchase contains.
        * **PendingOrderId**: The unique id of this Pending Order.
        * **Modified**: Unix time this order was last modified.
        * **NumPhotos**: The number of photos in this order.
        * **Email**: The email this order will be sent to.

    * **Error**: Optional error string.

Example Returns (200 OK)::

    {
        "Orders": [
            {
                "OwnerId": "<owner uuid>",
                "BrandId": "<brand uuid>",
                "GroupId": [
                    "<group uuid>",
                    ...
                ],
                "PendingOrderId": "<pending order id>",
                "Modified": 1449791030,
                "NumPhotos": 15,
                "Email": "example@example.com"
            },
            ...
        ]
    }


/v1/Purchase/Pending/<pending order id>
---------------------------------------

PUT
~~~

Updates an pending order by either completing it or discarding it. If the order is approved a email will immediately be sent to the customer.

PostData
^^^^^^^^

Fields

    * **NewState**: New state of the Pending Order: {"approved", "discarded"}.
    * **EmailChange**: Optional. If not empty will override the Email already in the Pending order.

Example PostData::

    {
        "NewState": "approved",
        "EmailChange": ""
    }

Returns
^^^^^^^

Fields

    * **Error**: Optional error string.

Example Returns (200 OK)::

    {
        "Error": ""
    }
