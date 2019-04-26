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
FlowCommand supports two types of authentication - Token Auth and Basic Auth. All requests must be authenticated using one of the two schemes.

You can request security credentials from your FlowCommand representative.

### Token Auth
> To authorize with Token Auth, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl https://app.flowcommand.com/api-path
  -H "Authorization: Token api-auth-token"
```

> Make sure to replace `api-auth-token` with your API key.

With Token Auth, FlowCommand expects for the API key to be included in all API requests as an HTTP header like:

`Authorization: Token api-auth-token`

<aside class="notice">
You must replace <code>api-auth-token</code> with your personal API key.
</aside>

### Basic Auth

> To authorize with Basic Auth, use this code:

```shell
curl https://username:password@app.flowcommand.com/api-path

OR

curl https://app.flowcommand.com/api-path
  -H "Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ="
```

> Make sure to replace `username` and `password` with your API credentials.

With Basic Auth you can include your username and password in the URL like:
`https://username:password@app.flowcommand.com/api-path`

Alternatively you can pass the Basic Auth credentials as an Authorization header. Make sure to base 64 encode your credentials formatted as "username:password" and include them in an HTTP header:

`Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=`

# Flow Sensors

## Get All Flow Sensors

```shell
curl "https://app.flowcommand.com/api/v1/flow_sensors/"
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
		"last_ping_flowrate": 5356.9,
		"last_ping_datetime": "2019-02-05T20:41:02Z"
	}, {
		"id": 2,
		"name": "Sensor 2",
		"latitude": 29.760435,
		"longitude": -95.3698,
		"pipe_diameter": 19.14,
		"last_ping_flowrate": 0,
		"last_ping_datetime": "2019-02-05T20:15:08Z"
	}]
}
```

This endpoint retrieves all flow sensors ordered by id.

### HTTP Request

`GET https://app.flowcommand.com/api/v1/flow_sensors/?page=1`

### Optional Query String Parameters

Parameter | Default | Description
--------- | ------- | -----------
page | 1 | Page (each page contains 1000 items)

<aside class="success">
Remember — you must authenticate each request using an Authorization token header or Basic Auth
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
activated_at | Datetime of when the sensor was activated

## Get Pings for a Specific Flow Sensor

```shell
curl "https://app.flowcommand.com/api/v1/flow_sensors/1/pings/"
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
		"flowrate": 759.9,
		"datetime": "2019-02-05T20:15:08Z"
	}, {
		"id": 2,
		"sensor_id": 1,
		"sensor_name": "Sensor 1",
		"flowrate": 0.0,
		"datetime": "2019-02-05T19:44:09Z"
	}]
}
```

This endpoint retrieves pings for a given flow sensor, ordered by datetime of the sensor reading (newest to oldest).

### HTTP Request

`GET https://app.flowcommand.com/api/v1/flow_sensors/<FLOW_SENSOR_ID>/pings/?page=1`

<aside class="notice">
Replace &lt;FLOW_SENSOR_ID&gt; with an integer ID from the Get All Flow Sensors request.
</aside>

### Path Parameters

Parameter | Description
--------- | -----------
FLOW_SENSOR_ID | ID of Sensor

### Optional Query String Parameters

Parameter | Default | Description
--------- | ------- | -----------
page | 1 | Page (each page contains 1000 items)
start| datetime of first ping | Include only pings after this date (in ISO 8601 format e.g. 2019-02-05T19:44:09Z)
end | datetime of last ping |  Include only pings before this date

### Response Ping Parameters

Parameter | Description
--------- | -----------
id | ID of the ping
sensor_id | ID of associated flow sensor
sensor_name | Name of associated flow sensor
flowrate | Flowrate in bbls/day at time of ping
datetime | Datetime of ping in UTC


## Get Flow Volume For a Specific Flow Sensor and Time Range

```shell
curl "https://app.flowcommand.com/api/v1/flow_sensors/1/flow_volume/2019-02-27T10:00:00Z/2019-02-28T10:00:00Z/"
```

> The above command returns JSON structured like this:

```json
{
  "sensor_id": 1,
  "sensor_name": "Sensor 1",
  "flow_volume": 759.9,
  "start": "2019-02-27T10:00:00Z",
  "end": "2019-02-28T10:00:00Z"
}
```

This endpoint retrieves the total flow volume in bbls between the start and end dates for a given sensor. 

### HTTP Request

`GET https://app.flowcommand.com/api/v1/flow_sensors/<FLOW_SENSOR_ID>/flow_volume/<START>/<END>/`

<aside class="notice">
Replace &lt;FLOW_SENSOR_ID&gt; with an integer ID from the Get All Flow Sensors request.
</aside>

### Path Parameters

Parameter | Description
--------- | -----------
FLOW_SENSOR_ID | ID of Sensor
START | Start of period timestamp (in ISO 8601 format e.g. 2019-02-05T19:44:09Z)
END | End of period timestamp

### Response Ping Parameters

Parameter | Description
--------- | -----------
sensor_id | ID of associated flow sensor
flow_volume | Flow Volume between start and end dates in bbls
start | Start timestamp
end | End timestamp


## Get Hourly Flow Volumes For a Specific Flow Sensor

```shell
curl "https://app.flowcommand.com/api/v1/flow_sensors/1/flow_volume_hours/2019-02-27T10:00:00Z/2019-02-27T12:00:00Z/"
```

> The above command returns JSON structured like this:

```json
{
	"count": 2,
	"next": null,
	"previous": null,
	"results": [{
    "sensor_id": 1,
    "sensor_name": "Sensor 1",
    "flow_volume": 759.9,
    "start": "2019-02-27T11:00:00Z",
    "end": "2019-02-27T12:00:00Z"
	}, {
    "sensor_id": 1,
    "sensor_name": "Sensor 1",
    "flow_volume": 356.1,
    "start": "2019-02-27T10:00:00Z",
    "end": "2019-02-27T11:00:00Z"
	}]
}
```

This endpoint retrieves hourly flow volumes for a given flow sensor, ordered from newest to oldest.

### HTTP Request

`GET https://app.flowcommand.com/api/v1/flow_sensors/<FLOW_SENSOR_ID>/flow_volume_hours/<START>/<END>/`

### Path Parameters

Parameter | Description
--------- | -----------
FLOW_SENSOR_ID | ID of Sensor
START | Start of range timestamp (in ISO 8601 format e.g. 2019-02-05T19:44:09Z)
END | End of range timestamp

### Response Flow Volume Hour Parameters

Parameter | Description
--------- | -----------
sensor_id | ID of flow sensor
sensor_name | Name of flow sensor
flow_volume | Flow Volume for the hour in bbls
start | Start timestamp of the hour
end | End timestamp of the hour

# Webhook Endpoints
Set up a webhook endpoint in order to receive a POST request each time an incoming transmission is received. Each request will include the following data:

```json
{
  "type": "ping_received",
  "api_version": "1",
	"data": {
		"id": 2,
		"sensor_id": 1,
		"sensor_name": "Sensor 1",
		"flowrate": 0.0,
		"datetime": "2019-02-05T19:44:09Z"
	}
}
```

To add a webhook endpoint, go to the webhook configuration tool at https://app.flowcommand.com/app/developers

Each webhook POST request will include an authentication HTTP header. The header name is "FC-Signature" and the secret value is shown in the webhook configuration tool.

