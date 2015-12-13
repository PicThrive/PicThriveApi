Owner
=====
Details about the root account holder, and their preferences and settings.

/Owner/
-------

GET
~~~

Returns details about the owner account.

Returns
^^^^^^^

Fields

    * **OwnerId**: Owner UUID.
    * **Created**: Unix time this account was created at.
    * **StripeSubStatus**: The status of this account: {"trialing", "active", "past_due", "canceled", "unpaid"}
    * **CurrentPeriodEnd**: Unix time this account's period ends at.
    * **CardDetails**: Optional. String description of attached card.
    * **LastName**: Optional.
    * **FirstName**: Optional.
    * **Email**: Optional.
    * **Plan**: Optional.
    * **PrimaryBrand**: Optional. UUID of their 'Primary'/Default brand.
    * **Timezone**: Optional. Timezone they are in. Eg. America/Vancouver

Example Returns (200 OK)::

    {
        "OwnerId": "<owner uuid>",
        "Created": 1449791030,
        "StripeSubStatus": "trialing",
        "CurrentPeriodEnd": "1449891030",
        "CardDetails": "Visa ending in 4848",
        "LastName": "Example",
        "FirstName": "Ample",
        "Email": "example@example.com",
        "Plan": "growth-month",
        "PrimaryBrand": "<brand uuid>",
        "Timezone": "America/Vancouver"
    }

PUT
~~~
Updates an owner.

PostData
^^^^^^^^

Fields

    * **LastName**: Optional.
    * **FirstName**: Optional.
    * **Email**: Optional.
    * **PrimaryBrand**: Optional. UUID of their 'Primary'/Default brand.
    * **Timezone**: Optional. Timezone they are in. Eg. America/Vancouver

Example Post Data::

    {
        "LastName": "Thor",
        "FirstName": "The",
        "Email": "thor@example.com"
    }

Returns
^^^^^^^

Fields

    * **Error**: Optional error string.

Example Returns (200 OK)::

    {
    }


/Owner/Invoice/
---------------

GET
~~~
Returns the list of invoices associated with this account.

Returns
^^^^^^^

Fields

    * **Items**: List of stripe Invoices. See https://stripe.com/docs/api#invoices
    * **Error**: Optional error string.



/Owner/Invoice/<invoiceId>
--------------------------

GET
~~~
Returns details about the given invoice. Use 'upcoming' as the invoice id for retrieving the upcoming invoice


.. _MoreChk:

Returns
^^^^^^^

Fields

    * See https://stripe.com/docs/api#invoices
    * **MoreChk**: Used to get more line items for this invoice.
    * **Error**: Optional Error string.

/Owner/Invoice/<invoiceId>/Lines
--------------------------------

GET
~~~
Returns more line details about the given invoice.

Query Params
^^^^^^^^^^^^

Fields

    * **customer**: Stripe Customer Id from invoice.
    * **check**: Returned as :ref:`MoreChk <MoreChk>`
    * **last**: Last item received in the invoice list.

Returns
^^^^^^^

Fields

    * **Items**: List of invoice items. See https://stripe.com/docs/api#invoice_lines

/Owner/Invoice/<invoiceId>/Pdf
------------------------------

GET
~~~
Returns the link to download the invoice PDF from. Not all invoices will have PDFs.

Query Params
^^^^^^^^^^^^

Fields

    * **customer**: Stripe Customer Id from invoice.
    * **check**: Returned as :ref:`MoreChk <MoreChk>`

Returns
^^^^^^^

Fields

    * **Link**: Link to the invoice.
    * **Error**: Optional error string

/Owner/Sales
------------

GET
~~~
Returns sales data for graphing.

Query Params
^^^^^^^^^^^^

Fields

    * **period**: Optional. Default 'day'. {'day', 'month', 'hour'}
    * **rangeStart**: Optional. Default '-1w'. {'h': hour, 'd': day, 'w': week, 'm': month}
    * **rangeEnd**: Optional. Default 'now'.
    * **brandId**: Optional. Default 'all'.

Returns
^^^^^^^

Fields

    * **Labels**: List of x-axis labels. Will be date strings according to period.
    * **Data**: List of series and value data. Is a 2D array. First array [0, 1] matches the series indexes. Second array [0][0, 1, ...] is the y-values for the series data.
    * **Series**: A list of series names.
    * **Error**: Optional error string

Example Returns (200 OK)::

    {
        "Labels": ["2015-01", "2015-02"],
        "Data": [
            [0, 100],
            [10, 0]
        ],
        "Series": ["AmountCharged", "AmountRefunded"]
    }


/Owner/Balance
--------------

GET
~~~
Returns the all the connected stripe account's balances.

Returns
^^^^^^^

Fields

    * **Accounts**: List of balances

        * **Name**: Name of account.
        * **Pending**: List of pending amounts.

            * **amount**: Amount int cents.
            * **currency**: Currency.

        * **Available**: List of available amounts.

            * **amount**: Amount int cents.
            * **currency**: Currency.

    * **Error**: Optional error string.

Example Returns (200 OK)::

    {
        "Accounts": [
            {
                "Name": "Example",
                "Pending": [
                    {
                        "Amount": 200,
                        "Currency": "USD"
                    }
                ],
                "Available": [
                    {
                        "Amount": 3500,
                        "Currency": "USD"
                    }
                ]
            }
        ]
    }