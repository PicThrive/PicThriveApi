Emails
======

The following endpoint allows for emails to be sent to customers, and for the status to be checked.

/v1/Email/
----------

.. _PostEmail:

POST
~~~~
Sends an email.

PostData
^^^^^^^^

Fields

    * **Messages**: A list of emails to send.

        * **To**: Email address to send to.
        * **Type**: Type of email being sent. {"tokenAndGroup", "group", "token"}
        * **BrandId**: The Brand this email is sent from.
        * **Token**: Used in "tokenAndGroup" or "token". This specifies a purchase token.
        * **GroupId**: Used in "group" or "tokenAndGroup". Specifies the GroupId you want to direct the customer to.
        * **NumPhotos**: Used in "tokenAndGroup" or "token". Specifies the # of photos a customer receives. -1 => unlimited.

Example PostData::

    {
        "Messages": [
            {
                "To": "example@example.com",
                "Type": "token",
                "BrandId": "<brand uuid>",
                "Token": "abcde",
                "NumPhotos": 10
            },
            ...
        ]
    }

Returns
^^^^^^^

Fields

    * **Results**: A list of the send results.

        * **Error**: Optional error string on why the email can't be sent.
        * **MessageId**: The id of the email sent.
        * **Status**: http status code (Eg. 200) on email send result.

    * **Error**: Optional error string

Example Returns (200 OK)::

    {
        "Results": [
            {
                "Status": 200,
                "MessageId": "<message uuid>"
            }
        ]
    }


GET
~~~
Returns the email history.

Query Params
^^^^^^^^^^^^

Fields

    * **BrandId**: Optional. Filter based on the specified BrandId.
    * **Next**: Optional. Return the next set of results, take from the result of a previous call.
    * **After**: Optional. Unix time that emails cannot be updated after. Default: Now.
    * **State**: Optional. Which state the email should be in {"sent", "delivered", "bounced", "complaint"}
    * **Email**: Optional. Which email to search for. Other Query Params will be ignored. Must be a direct match.

Returns
^^^^^^^

Fields

    * **Emails**: List of email status and errors.

        * **Email**: The email address that this was sent to.
        * **Type**: The type of email that was sent: {"tokenAndGroup", "group", "token", "purchase"}
        * **MessageId**: The UUID of this email message.
        * **SentAt**: The unix time this email was sent at.
        * **LastUpdated**: The unix time this email was last updated at. Eg. Delivered notification.
        * **State**: The state of this email: {"sent", "delivered", "bounced", "complaint"}
        * **BrandId**: The Brand UUID this email was sent from.
        * **OwnerId**: The Owner UUID this email was sent from.
        * **MsgDetails**: Details about how the message was sent, and its content.

            * **To**: Email to send to.
            * **Type**: Type of email being sent. {"tokenAndGroup", "group", "token", "purchase"}
            * **BrandId**: The Brand this email is being sent from.
            * **Token**: Used in "tokenAndGroup" or "token". This specifies a purchase token.
            * **GroupId**: Used in "group" or "tokenAndGroup". Specifies the GroupId we want to direct the customer to.
            * **NumPhotos**: Used in "tokenAndGroup" or "token". Specifies the # of photos a customer received. -1 => unlimited.
            * **OrderId**: Used in "purchase". UUID of the customer's order.

        * **ComplaintType**: Type of complaint.
        * **BounceType**: Type of bounce.
        * **BounceSubType**: Subtype of bounce.
        * **BounceDetails**: Rawer Details about the bounce.

            * **bounceSubType**: See http://docs.aws.amazon.com/ses/latest/DeveloperGuide/notification-contents.html#bounce-types
            * **bounceType**: See http://docs.aws.amazon.com/ses/latest/DeveloperGuide/notification-contents.html#bounce-types
            * **reportingMTA**: See http://docs.aws.amazon.com/ses/latest/DeveloperGuide/notification-contents.html#bounce-object
            * **bouncedRecipients**: See http://docs.aws.amazon.com/ses/latest/DeveloperGuide/notification-contents.html#bounce-object.
            * **timestamp**: ISO8601 format timestamp.
            * **feedbackId**: unique id for this bounce.

        * **DeliveredDate**: Unix time this message was delivered at.

    * **Next**: Used to get the next set of data. Pass in as query param 'Next'.
    * **Error**: Optional error string

Example Returns (200 OK)::

    {
        "Emails": [
            {
                "Email": "example@example.com",
                "Type": "tokenAndGroup",
                "MessageId": "<message uuid>",
                "SentAt": 1449791030,
                "LastUpdated": 1449792030,
                "State": "delivered",
                "BrandId": "<brand uuid>",
                "OwnerId": "<owner uuid>",
                "MsgDetails": {
                    "To": "example@example.com",
                    "Type": "tokenAndGroup",
                    "BrandId": "<brand uuid>",
                    "Token": "abcde",
                    "NumPhotos": 10,
                    "GroupId": "<group uuid>"
                },
                "ComplaintType": "",
                "BounceType": "",
                "BounceSubType": "",
                "DeliveredDate": 1449792030
            },
            ...
        ]
    }