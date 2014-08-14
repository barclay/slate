---
title: Deliv Private API Reference

toc_footers:
  - <a href='https://api.deliv.co/docs/v2'>Deliv Public API Documentation</a>
  - <a href='https://deliv.co/dev/dashboard'>Developer API Dashboard</a>

includes:
  - users
  - accounts
  - stores
  - deliveries
  - sweeps

search: true
---

# Introduction

Deliv provides same day delivery from top retailers to their customers
using a crowd-sourced workforce. The Deliv API provides a simple interface
to creating and managing deliveries with the Deliv service.

We've designed the Deliv API in a RESTful way such that the consumption for
your team should be as easy as possible.

This version of the documentation augments the [public v2 API docs](http://api.deliv.co/docs), 
and is meant to be a companion of sorts. 

<aside class="warning">
As this is the **private** version of the API, it should **only** be used internally. 
</aside>

For any questions on this documentation (or the API), please 
contact us at [eng@deliv.co](mailto:eng@deliv.co).


## Status Codes
The Deliv API uses conventional HTTP response codes to indicate success or 
failure on all requests.

|Status Code         |Result                         |
|--------------------|-------------------------------|
|**200 OK**          |Request completed as expected. |
|**201 Created**     |Used for requests that create new objects (i.e. DeliveryEstimate, Delivery). |
|**204 No Content**  |The server has completed the request but does not need to return a body (i.e. DELETE requests). |
|**400 Bad Request** |The request contains invalid/missing information or is out of context. The status text will contain more specific message(s). |
|**401 Unauthorized**|Authentication credentials were missing or incorrect. |
|**403 Forbidden**   |The request is understood, but it has been refused or access is not allowed. |
|**404 Not Found**   |The requested resource could not be found. |
|**50X Errors**      |Occur when something goes wrong in the Deliv API. |


## Dates and Timezones

All dates and times in the Deliv API are expressed in [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601),
with a UTC offset.

#### Example: 

`2013-08-23T15:46:20Z`

<aside class="notice">
You can identify ISO 8691 with a UTC offset by the trailing `Z`
</aside>

# Resources

## Stores

A store is an object that represents one of you physical brick and mortar
places of business, where our drivers pickup packages from you for your customers.

|Attribute                  |Type        |Description       |
|---------------------------|------------|------------------|
|id                         |string      |                  |
|id_alias                   |string      |Optional unique id matching your internal representation
|name                       |string      |                  |
|phone                      |string      |                  |
|suite\_number              |string      |                  |
|address\_line\_1           |string      |                  |
|address\_line\_2           |string      |                  |
|address\_city              |string      |                  |
|address\_state             |string      |Two letter abbreviation (i.e. 'CA')|
|address\_zipcode           |string      |                  |


