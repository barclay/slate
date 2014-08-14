# Accounts

An account the parent object that represents the company that does business 
with Deliv. Accounts can have many users, and all deliveries and stores are 
tied to an account. 

### API Endpoint
`https://api.deliv.co/v2/accounts`

### Properties

|Attribute           |Type       |Description       |
|--------------------|-----------|------------------|
|id                       |int        |The identifier of the user |
|name                     |string     |The name of the account    |
|website\_uri             |string     |The account's website      |
|webhook\_url             |string     |The general URL for delivery webooks |
|delivery\_window\_length |int        |The length of the account's delivery windows (in minutes) |
|fetch\_window\_length    |int        |The length of the account's fetch windows (in minutes) |
|api\_v1\_enabled         |boolean    |Flag for if the account has access to APIv1 |
|api\_v2\_enabled         |boolean    |Flag for if the account has access to APIv1 |
|signature_required_value_threshold|int|The threshold (in dollars) at which a delivery must be signed for |
|logo                     |object     |Image information about the company's logo |

## Retrieve a list of Accounts

```bash
$ curl -X GET https://api.deliv.co/v2/accounts/ \
    -d api_key=5ecr3t7ok3n
```

> Response:

```javascript
[
    {
        "id": 36,
        "name": "Acme Corporation",
        "website_uri": "http://www.acme.co",
        "webhook_url": null,
        "delivery_window_length": 60,
        "fetch_window_length": 120,
        "package_prep_length": 60,
        "display_publicly": true,
        "api_v1_enabled": false,
        "api_v2_enabled": false,
        "signature_required_value_threshold": null,
        "logo": {
            "id": 492,
            "created_at": "2014-02-20T19:18:22Z",
            "image_width": 145,
            "image_height": 95,
            "file_name": "434869cf8ffb7563d4f2022e09a28cf114417bb8",
            "photo_type": "account_logos",
            "updated_at": "2014-02-20T19:18:22Z",
            "format_id": 3
        }
    },
    // additional accounts...
]
```

Retrieves the details of an account that has previously been created.

<aside class="warning">
    Only users with [admin](#resources/roles) roles can make requests for 
    all account details.
</aside>

### Definition
`GET https://api.deliv.co/v2/accounts`


## Retrieve a Single Account

```bash
$ curl -X GET https://api.deliv.co/v2/accounts/1 \
    -d api_key=5ecr3t7ok3n
```

> Response:

```javascript
{
    "id": 36,
    "name": "Acme Corporation",
    "website_uri": "http://www.acme.co",
    "webhook_url": null,
    "delivery_window_length": 60,
    "fetch_window_length": 120,
    "package_prep_length": 60,
    "display_publicly": true,
    "api_v1_enabled": false,
    "api_v2_enabled": false,
    "signature_required_value_threshold": null,
    "logo": {
        "id": 492,
        "created_at": "2014-02-20T19:18:22Z",
        "image_width": 145,
        "image_height": 95,
        "file_name": "434869cf8ffb7563d4f2022e09a28cf114417bb8",
        "photo_type": "account_logos",
        "updated_at": "2014-02-20T19:18:22Z",
        "format_id": 3
    }
}
```

Retrieves the details of an account that has previously been created.

<aside class="warning">
    Only users with [admin](#resources/roles) roles can make requests for 
    account id's other than their own.
</aside>

### Definition
`GET https://api.deliv.co/v2/accounts/{:id}`

### Arguments

|Argument    |Type       |Description       |
|------------|-----------|------------------|
|id          |string     |(**Required**) The id of the [account](#account)|

## List the Stores of an Account

```bash
$ curl -X GET https://api.deliv.co/v2/accounts/1/stores \
    -d api_key=5ecr3t7ok3n
```

> Response:

#### Example
```javascript
[
    {
        "id": "4b8dd1d6-35d8-4540-909d-d646d30c1d7f",
        "id_alias": "49d09"
        "name": "XYZ Mart",
        "phone": "650 555 1234",
        "suite_number": "123",
        "address_line_1": "1014 El Camino Real",
        "address_line_2": "Suite 2",
        "address_city": "Redwood City",
        "address_state": "CA",
        "address_zipcode": "94061"
    }
]
```

Retrieves a list of stores that belong to a given account.

<aside class="warning">
    Only users with [admin](#resources/roles) roles can make requests for 
    account id's other than their own.
</aside>

### Definition
`GET https://api.deliv.co/v2/accounts/{:id}/stores`

### Arguments

|Argument    |Type       |Description       |
|------------|-----------|------------------|
|id          |string     |(**Required**) The id of the [account](#account)|
