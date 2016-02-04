Brands
======

The following endpoint describes manipulations of a Brand. A Brand has its own store, links, logos and campaigns.


/v1/Brand/
----------

.. _GetBrands:

GET
~~~

Returns the Brands that this user Owns.

Returns
^^^^^^^

Fields

    * **Brands**: A list of brands. See :ref:`Get Brand Return <GetBrandReturns>`
    * **Error**: Optional error string.

Return Example (200 OK)::

    {
        "Brands": [
            {
                "OwnerId": "<owner uuid>",
                "BrandId": "<brand uuid>",
                "VanityUrl": "<brand vanity url>",
                "Name": "name of brand",
                "Website": "http://...",
                "BookingLink": "http://...",
                "Description": "some 140 character description",
                "Social": {
                    "FacebookUrl": "https://...",
                    "TwitterUrl": "https://...",
                    "GooglePlusUrl": "https://..."
                },
                "Review": {
                    "TripAdvisorUrl": "https://"
                },
                "Banner300": "/image/suffix.jpg",
                "Banner1280": "/image/suffix.jpg",
                "Logo100": "/image/suffix.jpg",
                "Logo150": "/image/suffix.jpg",
                "Logo300": "/image/suffix.jpg",
                "GoogleAnalytics": "UA-000000-01",
                "Pricing": {
                    "Mode": "flat",
                    "Flat": 500,
                    "Currency": "USD",
                    "Levels": [],
                    "FullAlbum": 0,
                },
                "MailChimp": {
                    "Account": "string",
                    "List": "string",
                    "AccountName": "string",
                    "ListName": "string"
                },
                "Stripe": {
                    "Account": "string",
                    "AccountName": "string",
                    "PublishableKey": "string"
                },
                "Campaign": {
                    "Headline": "string",
                    "CreatedAt": 1449791030,
                    "Type": "string"
                },
                "Ads": [],
                "AdsUpdated": 1449791030,
                "GoogleRemarketingCode": "string",
                "FacebookRemarketingCode": "string",
                "RemarketingUpdated": 1449791030
            },
            ...
        ]
    }


POST
~~~~
Creates a brand.

PostData
^^^^^^^^
Only 'Name' and 'VanityUrl' are required. All other Brand fields are optional. A VanityUrl must be unique and is case insensitive.

Fields

    * **VanityUrl**: Required. The url suffix for the brand. Eg. 'picthrivedemo' will result in the storefront url of https://store.picthrive.com/picthrivedemo
    * **Name**: Required. The name of the Brand.

Example PostData::

    {
        "VanityUrl": "string",
        "Name": "string name"
    }

Returns
^^^^^^^

Fields

    * See :ref:`Get Brand Return <GetBrandReturns>`

Example Return (201 Created)::

    {
        "OwnerId": "<owner uuid>",
        "BrandId": "<brand uuid>",
        "VanityUrl": "<brand vanity url>",
        "Name": "name of brand"
    }

/v1/Brand/<brand uuid>
----------------------

GET
~~~
Returns info about the specified Brand.


.. _GetBrandReturns:

Returns
^^^^^^^

Fields

    * **OwnerId**: Owner UUID
    * **BrandId**: Brand UUID
    * **VanityUrl**: Vanity Url Suffix for this brand. Eg. 'picthrivedemo'. Which means the storefront url is 'https://store.picthrive.com/picthrivedemo'
    * **Name**: Name of the Brand.
    * **Website**: Optional. Website of the brand.
    * **BookingLink**: Optional. Booking link of the brand.
    * **Description**: Optional. Description of the brand.
    * **Social**: Optional. Set of social links.

        * **FacebookUrl**: Optional. Url to their Facebook page.
        * **TwitterUrl**: Optional. Url to their Twitter page.
        * **GooglePlusUrl**: Optional. Url to their Google+ page.

    * **Review**: Optional. Set of review based links to TripAdvisor and Google+.

        * **TripAdvisorUrl**: Optional. Link to their TripAdvisor page.

    * **Banner300**: Optional. Their storefront banner url suffix, sized at a width of 300px.
    * **Banner1280**: Optional. Their storefront banner url suffix, sized at a width of 1280px.
    * **Logo100**: Optional. Their logo url suffix, sized at a width of 100px.
    * **Logo150**: Optional. Their logo url suffix, sized at a width of 150px.
    * **Logo300**: Optional. Their logo url suffix, sized at a width of 300px.
    * **GoogleAnalytics**: Optional. Their Google Analytics code to inject onto their store and album pages.
    * **Pricing**: Optional. Pricing Information for their store.

        * **Mode**: Three possible values { off, flat, grad }
        * **Flat**: Only present if **Mode** is **flat**. The flat cost in cents for each photo.
        * **Currency**: The currency to charge in { "CAD", "USD" }
        * **Levels**: Only present if **Mode** is **grad**. The set of graduated levels of pricing.

            * **Level**: The number of photos. <= this value.
            * **Amount**: The cost in cents for this level.

        * **FullAlbum**: Only present if **Mode** is **grad**. The total price to pay if all levels are exceeded.

    * **MailChimp**: Optional. Describes their selected account. Does not manage the MailChimp credentials.

        * **Account**: The selected Account Id.
        * **List**: The selected List Id.
        * **AccountName**: The Name of the selected account.
        * **ListName**: The Name of the selected list.

    * **Stripe**: Optional. Describes their selected account. Does not manage the Stripe credentials.

        * **Account**: The selected Account Id.
        * **AccountName**: The selected Account Name.

    * **Campaign**: Optional. Info about which campaign is currently running.

        * **Headline**: The title of the prompt.
        * **CreatedAt**: When the campaign was started.
        * **Type**: Type of campaign. {"Subscribe", "Review", "Share", "Website", "Book", "Like"}

    * **Ads**: Optional. List of banner ads to run.

        * **Chance**: The chance of running this add. [0.0 - 1.0]
        * **ImageUrl**: The url suffix of the banner image.
        * **Link**: The link to navigate to when clicking the ad.

    * **AdsUpdated**: Optional. Unix time of the last udpate to ads.
    * **GoogleRemarketingCode**: Optional. Google Remarketing code.
    * **FacebookRemarketingCode**: Optional. Facebook Retargeting code.
    * **RemarketingUpdated**: Optional. Unix time of last remarketing update.


Example Returns (200 OK)::

    {
        "OwnerId": "<owner uuid>",
        "BrandId": "<brand uuid>",
        "VanityUrl": "<brand vanity url>",
        "Name": "name of brand",
        "Website": "http://...",
        "BookingLink": "http://...",
        "Description": "some 140 character description",
        "Social": {
            "FacebookUrl": "https://...",
            "TwitterUrl": "https://...",
            "GooglePlusUrl": "https://..."
        },
        "Review": {
            "TripAdvisorUrl": "https://"
        },
        "Banner300": "/image/suffix.jpg",
        "Banner1280": "/image/suffix.jpg",
        "Logo100": "/image/suffix.jpg",
        "Logo150": "/image/suffix.jpg",
        "Logo300": "/image/suffix.jpg",
        "GoogleAnalytics": "UA-000000-01",
        "Pricing": {
            "Mode": "flat",
            "Flat": 500,
            "Currency": "USD",
            "Levels": [
                {
                    "Level": 1,
                    "Amount": 500
                },
                ...
            ],
            "FullAlbum": 0,
        },
        "MailChimp": {
            "Account": "string",
            "List": "string",
            "AccountName": "string",
            "ListName": "string"
        },
        "Stripe": {
            "Account": "string",
            "AccountName": "string"
        },
        "Campaign": {
            "Headline": "string",
            "CreatedAt": 1449791030,
            "Type": "string"
        },
        "Ads": [
            {
                "Chance": 1.00,
                "ImageUrl": "/image/suffix.jpg",
                "Link": "https://..."
            },
            ...
        ],
        "AdsUpdated": 1449791030,
        "GoogleRemarketingCode": "string",
        "FacebookRemarketingCode": "string",
        "RemarketingUpdated": 1449791030
    }

PUT
~~~
Updates a Brand. Missing fields are ignored. Empty fields, for things like description, are interpreted as 'delete'.


PostData
^^^^^^^^

Fields

    * **Name**: Name of the Brand.
    * **Website**: Optional. Website of the brand. Set to "" to delete.
    * **BookingLink**: Optional. Booking link of the brand. Set to "" to delete.
    * **Description**: Optional. Description of the brand. Set to "" to delete.
    * **Social**: Optional. Set of social links.

        * **FacebookUrl**: Optional. Url to their Facebook page. Set to "" to delete.
        * **TwitterUrl**: Optional. Url to their Twitter page. Set to "" to delete.
        * **GooglePlusUrl**: Optional. Url to their Google+ page. Set to "" to delete.

    * **Review**: Optional. Set of review based links to TripAdvisor and Google+.

        * **TripAdvisorUrl**: Optional. Link to their TripAdvisor page. Set to "" to delete.

    * **Banner**: Optional. Base64 encoded image to use as the banner.
    * **Logo**: Optional. Base64 encoded image to use as the logo.
    * **GoogleAnalytics**: Optional. Their Google Analytics code to inject onto their store and album pages. Set to "" to delete.
    * **Pricing**: Optional. Pricing Information for their store.

        * **Mode**: Three possible values { off, flat, grad }. Set to 'off' to remove pricing.
        * **Flat**: Only present if **Mode** is **flat**. The flat cost in cents for each photo.
        * **Currency**: The currency to charge in. { "CAD", "USD" }
        * **Levels**: Only present if **Mode** is **grad**. The set of graduated levels of pricing. Any posted levels will overwrite other levels.

            * **Level**: The number of photos. <= this value.
            * **Amount**: The cost in cents for this level.

        * **FullAlbum**: Only present if **Mode** is **grad**. The total price to pay if all levels are exceeded.

    * **MailChimp**: Optional. Describes their selected account. Does not manage the MailChimp credentials.

        * **Account**: The selected Account Id.
        * **List**: The selected List Id.
        * **AccountName**: The Name of the selected account.
        * **ListName**: The Name of the selected list.

    * **Stripe**: Optional. Describes their selected account. Does not manage the Stripe credentials.

        * **Account**: The selected Account Id.
        * **AccountName**: The selected Account Name.

    * **Campaign**: Optional. Info about which campaign is currently running. Set 'Type' to 'Delete' to remove campaigns.

        * **Headline**: The title of the prompt.
        * **Type**: Type of campaign. {"Subscribe", "Review", "Share", "Website", "Book", "Like"}

    * **Ads**: Optional. List of banner ads to run.

        * **Chance**: The chance of this ad running. [0.0 - 1.0]
        * **ImageBase64**: Base64 encoded image to use as the ad.
        * **Link**: The link to navigate to when clicking the ad.

    * **GoogleRemarketingCode**: Optional. Google Remarketing code.
    * **FacebookRemarketingCode**: Optional. Facebook Retargeting code.

Example Returns (200 OK)::

    {
        "OwnerId": "<owner uuid>",
        "BrandId": "<brand uuid>",
        "VanityUrl": "<brand vanity url>",
        "Name": "name of brand",
        "Website": "http://...",
        "BookingLink": "http://...",
        "Description": "some 140 character description",
        "Social": {
            "FacebookUrl": "https://...",
            "TwitterUrl": "https://...",
            "GooglePlusUrl": "https://..."
        },
        "Review": {
            "TripAdvisorUrl": "https://"
        },
        "Banner300": "/image/suffix.jpg",
        "Banner1280": "/image/suffix.jpg",
        "Logo100": "/image/suffix.jpg",
        "Logo150": "/image/suffix.jpg",
        "Logo300": "/image/suffix.jpg",
        "GoogleAnalytics": "UA-000000-01",
        "Pricing": {
            "Mode": "flat",
            "Flat": 500,
            "Currency": "USD",
            "Levels": [
                {
                    "Level": 1,
                    "Amount": 500
                },
                ...
            ],
            "FullAlbum": 0,
        },
        "MailChimp": {
            "Account": "string",
            "List": "string",
            "AccountName": "string",
            "ListName": "string"
        },
        "Stripe": {
            "Account": "string",
            "AccountName": "string"
        },
        "Campaign": {
            "Headline": "string",
            "CreatedAt": 1449791030,
            "Type": "string"
        },
        "Ads": [
            {
                "Chance": 1.00,
                "ImageUrl": "/image/suffix.jpg",
                "Link": "https://..."
            },
            ...
        ],
        "AdsUpdated": 1449791030,
        "GoogleRemarketingCode": "string",
        "FacebookRemarketingCode": "string",
        "RemarketingUpdated": 1449791030
    }


/v1/Brand/<brand uuid>/Subscribe
--------------------------------

POST
~~~~
Adds an email to the Brand's subscription list.

PostData
^^^^^^^^

Fields

    * **Email**: The email to subscribe.

PostData::

    {
        "Email": "example@example.com"
    }

Returns
^^^^^^^

Example Returns (201 Created)::

    {
        "Error": "optional error string"
    }

/v1/Brand/<brand uuid>/Group/
-----------------------------

GET
~~~
Returns all the groups for the given day for the specified brand.

See :ref:`Get Groups <GetGroupsCall>` for query params and return data.