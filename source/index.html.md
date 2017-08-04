---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/tripit/slate'>Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Tankfarm API (TF-api). You can use our API to access Tankfarm endpoints.

# Authentication

> To authorize, use this code:

```shell
# pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Token token=stage_123"
```

> Make sure to replace `stage_123` with your API key.

TF-api uses API keys to allow access to the API. You can request an API
key from daniel@tankfarmgroup.com.

TF-api expects the API key to be included in all API requests to the server in
a header that looks like the following:

`Authorization: Token token=stage_123`

<aside class="notice">
You must replace <code>stage_123</code> with your personal API key.
</aside>

# Engage/Brandcraft

## Get quote

> To get a quote, use this code:

```shell
curl "https://some_url.com/api/v1/quotes"
-H "Authorization: Token token=stage_123"
--data '{"zipcode": "11001", "annual_usage": 100, "owns_tank": true}'
--header 'content-type: application/json'
```

> The above command returns JSON structured like this:

```json
{
  "data":{
    "id":null,
    "type":"quotes",
    "links":{
      "self":"/api/v1/quotes/"
    },
    "attributes":{
      "zipcode":"11001",
      "annual-usage":100,
      "owns-tank":true,
      "price":"3.9717",
      "city":"Floral Park",
      "state":"NY",
      "supplier-name":"ABC Propane",
      "supplier-description": "Your wonderful supplier."
    }
  }
}
```

This endpoint retrieves quotes.

### HTTP Request

`GET https://some_url.com/api/v1/quotes`

### Request body parameters

Parameter | Type | Description
--------- | ---- | -----------
zipcode | String | Five digit US postal ZIP code.
annual_usage | Integer | Gallons of propane used per year.
owns_tank | Boolean | Does applicant own their propane tank?

<aside class="notice">
For this particular request body parameters use underscores while the returned JSON
uses dashes.
</aside>

## Create an applicant

```shell
curl "https://some_url.com/api/v1/engage-leads"
-H "Authorization: Token token=stage_123"
--data
  '{
    "data": {
      "type": "engage-leads",
      "attributes": {
        "name": "Mars Rover",
        "email": "msrover9a@test.com",
        "address": "123 Somewhere",
        "city": "Assonet",
        "state": "MA",
        "zipcode": "02702",
        "phone": "(888) 555-1212",
        "metadata": {
          "propane_use": ["primary_heat-medium", "hot_water", "cooking_range"],
          "owns_tank": true,
          "annual_usage": 500,
          "underground_tank": false,
          "tank_count": 2,
          "total_tank_size": 700,
          "previous_supplier": "prev sup",
          "quote": {
            "ppg": 2.18,
            "supplier_name": "ABC Supplier"
          }
        }
      }
    }
  }'
--header 'content-type: application/vnd.api+json'
```

> The above command returns JSON structured like this:

```json
{
  "data":{
    "id":"147953",
    "type":"engage-leads",
    "links":{
      "self":"http://some_url.com/api/v1/engage-leads/147953"
    },
    "attributes":{
      "name":"Mars Rover",
      "email":"msrover9a@test.com",
      "address":"123 Somewhere",
      "city":"Assonet",
      "state":"MA",
      "zipcode":"02702",
      "phone":"(888) 555-1212",
      "metadata":{
        "propane_use":["primary_heat-medium", "hot_water","cooking_range"],
        "owns_tank":true,
        "annual_usage":500,
        "underground_tank":false,
        "tank_count":2,
        "total_tank_size":700,
        "previous_supplier":"prev sup",
        "quote":[{"ppg":2.18,"supplier_name":"ABC Supplier","created_at":"2017-08-02T08:26:19.360-07:00"}]
      }
    }
  }
}
```

This endpoint creates a new user/applicant. Used when applicant signs up on tankfarmgroup.com website.

<aside class="notice">
Content-type must be <code>application/vnd.api+json</code>
</aside>

<aside class="warning">
Dupe emails will cause an error but it will update some information on the user about the dupe submit attempt.
It also records all the form data submitted for each dupe request.
</aside>

### HTTP Request

`POST curl "https://some_url.com/api/v1/engage-leads`

### Request body parameters (top level)

Parameter | Type | Description
--------- | ---- | -----------
data | json | Contains `type` and `attributes` keys

### Request body parameters (data)

Parameter | Type | Description
--------- | ---- | -----------
type | string | `engage-leads`
attributes | json | User data to populate

### Request body parameters (data/attributes)

Parameter | Type | Description
--------- | ---- | -----------
name | Integer | Applicants full name
email | Boolean |
address | String |
city | String |
state | String | Two digit state code
zipcode | String | Five digit ZIP code
phone | String |
metadata | json | User related metadata - mostly propane related.

### Request body parameters (data/attributes/metadata)

Parameter | Type | Description
--------- | ---- | -----------
propane_use | Array | e.g. primary_head-medium, hot_water, etc
owns_tank | Boolean | true or false
annual_usage | Integer | amount of propane used per year
underground_tank | Boolean | true (yes underground) or false (above ground)
tank_count | Integer | number of propane tanks
total_tank_size | Integer | sum of propane tank gallons
previous_supplier | String |
quote | json | quote provided by Tankfarm

### Request body parameters (data/attributes/metadata/quote)
Parameter | Type | Description
--------- | ---- | -----------
ppg | decimal | quoted price per gallon
supplier_name | string | named of matched supplier

# Other Endpoints

Just a list of endpoints we want to keep track of.

## Status

Type | Action/Controller
---- | ----
GET "/" | PagesController#status

## TF-CRM

Type | Action/Controller
---- | ----
POST "/api/v1/users" | Api::V1::UsersController#update
PUT "/api/v1/users/61603?include=signup-by&context_current_user_id=135947" | Api::V1::UsersController#update

## Propane.pro

Type | Action/Controller
---- | ----
POST "/api/v1/pp-leads" | Api::V1::PpLeadsController#create

## Webhooks

Type | Action/Controller
---- | ----
POST "/api/v1/sendinblue_webhook" | Api::V1::WebhooksController#sendinblue
POST "/api/v1/opt_out_webhook" | Api::V1::WebhooksController#opt_out
POST "/api/v1/dial_call_status_webhook/25578" | Api::V1::WebhooksController#dialCallStatus

## Engage/Brandcraft

See Engage/Brandcraft section
