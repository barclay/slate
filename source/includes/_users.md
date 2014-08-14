# Users

The user object represents an entity that's able to authenticate and interact
with the deliv system. Users have roles, and belong to accounts.

### API Endpoint
`https://api.deliv.co/v1/users`

### Properties

|Attribute           |Type       |Description       |
|--------------------|-----------|------------------|
|id                  |int        |The identifier of the user |
|photo\_url          |string     |A **protocol-relative** URL pointing to a photo of the user  |
|authentication_token|string     |The token that can be used for future API requests by this user |
|first\_name         |string     |The user's first name |
|last\_name          |string     |The user's last name |
|name                |string     |Concatenated first and last name of the user |
|roles               |array      |An array of the current roles of the user |
|email               |string     |The email address of the user |
|last\_sign\_in\_at  |timestamp  |The last time the user authenticated successfully |
|account\_id         |int        |The id of the account the user belongs to |
|store\_ids          |array      |An array of store ids the user is associated with |


## Roles

Roles are an enumerated set of permissions that a user may have. 

### Role Types
|name          |description            |
|--------------|-----------------------|
|**admin**       |Administrator. Access to everything. 
|**dispatcher**  |Similar to an administrator, should have access to manage stores, deliveries, etc.
|**api_user**    |A retailer account that's configured to talk directly to the API. Should be able to manage only the deliveries and objects that belong to their account. 
|**walkin_user** |A user in the store that will access the **instore** system. Should only be able to manage deliveries from their account, that they've created.
|**driver**      |A deliv driver that can deliver or fetch packages from store to customer
|**runner**      |A runner in the mall that can move packages from store to consolidation
|**attendant**   |Manager of runners, and is able to access the Inventory Management System
|**mall_admin**  |Manager of attendants 


## Authentication

```bash
$ curl -X POST https://api.deliv.co/v1/users/login \
       -d email=barclay@deliv.co \
       -d password=pA55w0rd
```

> Response:

```javascript
{
    "id" : 1,
    "photo_url" : "//d3ki2wdz8t61xp.cloudfront.net/driver_photos/fdd0055.jpg",
    "authentication_token" : "5ecr3t7ok3n",
    "authorized" : true,
    "first_name" : "Barclay",
    "last_name" : "Loftus",
    "name" : "Barclay Loftus",
    "roles" : [
        "admin",
        "api_user",
        "driver"
    ],
    "email" : "barclay@deliv.co",
    "last_sign_in_at" : "2014-08-12T18:13:30Z",
    "account_id": 1,
    "store_ids": [
        14
    ]
}
```

The first step for a user is to authenticate with the system. The common flow
for the deliv API, is that you first authenticate yourself with a username and 
password, and on success, you're given back your user account information. 
Contained in the user object is an `authentication_token` that you can then 
use for further API requests.

### Definition 
`POST https://api.deliv.co/v1/users/login`

### Arguments

|Argument            |Type       |Description       |
|--------------------|-----------|------------------|
|string              |email      |(**Required**) The email of the user to be authenticated|
|string              |password   |(**Required**) The password of the user to be authenticated|

<aside class="notice">
Authentication is one of the few API endpoints that uses traditional
form data in the body of the POST, **not** JSON.
</aside>

## Retrieve a User

```bash
$ curl -X GET https://api.deliv.co/v2/users/1 \
    -d api_key=5ecr3t7ok3n
```

> Response:

```javascript
{
    "id": 1,
    "photo_url": "//d3ki2wdz8t61xp.cloudfront.net/driver_photos/fdd0055.jpg",
    "authentication_token": "5ecr3t7ok3n",
    "authorized": true,
    "first_name": "Barclay",
    "last_name": "Loftus",
    "name": "Barclay Loftus",
    "roles": [
        "admin",
        "api_user",
        "driver"
    ],
    "email": "barclay@deliv.co",
    "last_sign_in_at": "2014-08-12T18:13:30Z",
    "account_id": 1,
    "store_ids": [
        14
    ]
}
```

Retrieves the details of a user that has previously been created.
You supply the ID, and we'll return the corresponding user information.

<aside class="warning">
    Only users with [admin](#resources/roles) roles can make requests for 
    user id's other than their own.
</aside>

### Definition
`GET https://api.deliv.co/v2/users/{:id}`

### Arguments

|Argument    |Type       |Description       |
|------------|-----------|------------------|
|id          |string     |(**Required**) The id of the [user](#user)|


## Update a User

```bash
$ curl -X PUT https://api.deliv.co/v2/users/1?api_key=5ecr3t7ok3n \
   --header "Content-Type:application/json" \
   --data '{"first_name": "Test", "last_name": "Foo"}'
```

> Response:

```javascript
{
    "id": 1,
    "photo_url": "//d3ki2wdz8t61xp.cloudfront.net/driver_photos/fdd0055.jpg",
    "authentication_token": "5ecr3t7ok3n",
    "authorized": true,
    "first_name": "Test",
    "last_name": "Foo",
    "name": "Test Foo",
    "roles": [
        "admin",
        "api_user",
        "driver"
    ],
    "email": "barclay@deliv.co",
    "last_sign_in_at": "2014-08-12T18:13:30Z",
    "account_id": 1,
    "store_ids": [
        14
    ]
}
```

Updates the details of a user record.

### Definition
`PUT https://api.deliv.co/v2/users/{:id}`

### Arguments

|Argument              |Type       |Description       |
|----------------------|-----------|------------------|
|id                    |id         |(**Required**) The id of the [user](#users) to be updated |
|location              |array      |(_optional_) Array containing `latitude, longitude` values of the current user's location (drivers)|
|photo                 |string     |(_optional_) A base64 encoded 300x300 png of the user |
|first\_name           |string     |(_optional_) The user's first name |
|last\_name            |string     |(_optional_) The user's last name |
|email                 |string     |(_optional_) The user's email address (must be a valid email) |


## Password Recovery

```bash
$ curl -X POST https://api.deliv.co/v1/users/forgot_password?email=foo@bar.com
```

> The response for this call will be empty:

```javascript
{ }
```

Occasionally a user might need to recover a forgotten password. We've 
conveniently provided an endpoint to trigger that flow. Calling this endpoint, 
with a user's email, will initiate an email to them to recover their password.

### Definition
`POST https://api.deliv.co/v1/users/forgot_password?email={:email}`

|Argument      |Type       |Description       |
|--------------|-----------|------------------|
|email         |string     |(**Required**) The email of the [user](#users) to reset the password for |

