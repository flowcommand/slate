---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:

includes:

search: true
---

# Introduction

Welcome to the FlowCommand API. You can use our API to access information on your FlowCommand flow sensors and pings (sensor readings) from those sensors.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Token api-auth-token"
```

> Make sure to replace `api-auth-token` with your API key.

FlowCommand uses API keys to allow access to the API. You can request an API key from your FlowCommand representative.

FlowCommand expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Token api-auth-token`

<aside class="notice">
You must replace <code>api-auth-token</code> with your personal API key.
</aside>

# Flow Sensors

## Get All Flow Sensors

```shell
curl "https://app.flowcommand.com/api/v1/flow_sensors/"
  -H "Authorization: Token api-auth-token"
```


> The above command returns JSON structured like this:

```json
{
	"count": 2,
	"next": null,
	"previous": null,
	"results": [{
		"id": 1,
		"name": "Sensor 1",
		"latitude": 29.760435,
		"longitude": -95.3698,
		"pipe_diameter": 14.64,
		"last_ping_flowrate": 5356.92,
		"last_ping_datetime": "2019-02-05T20:41:02+00:00"
	}, {
		"id": 2,
		"name": "Sensor 2",
		"latitude": 29.760435,
		"longitude": -95.3698,
		"pipe_diameter": 19.14,
		"last_ping_flowrate": 0,
		"last_ping_datetime": "2019-02-05T20:15:08+00:00"
	}]
}
```

This endpoint retrieves all flow sensors ordered by id.

### HTTP Request

`GET https://app.flowcommand.com/api/v1/flow_sensors/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
page | 1 | Page (each page contains 1000 items)

<aside class="success">
Remember — add Authorization header to every request!
</aside>

### Response Flow Sensor Parameters

Parameter | Description
--------- | -----------
id | ID of the flow sensor
name | Name of flow sensor
latitude | Latitude GPS coordinate of sensor
longitude | Longitude GPS coordinate of sensor
pipe_diameter | Diameter of pipe in inches
last_ping_flowrate | Flowrate in bbls/day from most recent ping
last_ping_datetime | Datetime of most recent ping in UTC

## Get Pings for a Specific Flow Sensor

```shell
curl "https://app.flowcommand.com/api/v1/flow_sensors/1/pings/"
  -H "Authorization: Token api-auth-token"
```

> The above command returns JSON structured like this:

```json
{
	"count": 2,
	"next": null,
	"previous": null,
	"results": [{
		"id": 1,
		"sensor_id": 1,
		"sensor_name": "Sensor 1",
		"flowrate": 759.99,
		"datetime": "2019-02-05T20:15:08+00:00"
	}, {
		"id": 2,
		"sensor_id": 1,
		"sensor_name": "Sensor 1",
		"flowrate": 0.0,
		"datetime": "2019-02-05T19:44:09+00:00"
	}]
}
```

This endpoint retrieves pings for a given flow sensor, ordered by datetime of the sensor reading (newest to oldest).

### HTTP Request

`GET https://app.flowcommand.com/api/v1/flow_sensors/<FLOW_SENSOR_ID>/pings/`

<aside class="notice">
Replace &lt;FLOW_SENSOR_ID&gt; with an integer ID from the Get All Flow Sensors request.
</aside>

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
page | 1 | Page (each page contains 1000 items)

### Response Ping Parameters

Parameter | Description
--------- | -----------
id | ID of the ping
sensor_id | ID of associated flow sensor
sensor_name | Name of associated flow sensor
flowrate | Flowrate in bbls/day at time of ping
datetime | Datetime of ping in UTC


