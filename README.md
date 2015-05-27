# node-time-server
A node.js task for candidates

## Description
In this task you are requested to build a small node.js server which tells the time upon request, sets the time zone upon request and provides a time polling mechanism.

#### To be more specific
Please implement the following:  

1) An HTTP server with 2 endpoints:
* GET /time
* POST /timezone
* For full details, please see the [API](README.md#http-api) section.  

2) A Websocket server which sends periodical time updates to each connected client.
* An update should be sent every 10 seconds.
* The update sent should be of the same structure as the response produced by the [GET /time](README.md#get-time) request.
* A client can also send a 'setTimezone' message to set the time zone used in the updates.
  * see [Websocket API](README.md#websocket-api).
* If the client didn't set the time zone, the time zone to be used is the _global time zone_ set in [POST /timezone](README.md#post-timezone).

## Details 
* Use whatever libraries you need. There's no need to implement an HTTP server, a Websocket server or anything else from scratch.
* The server state (e.g the global time zone) can remain in memory and doesn't need to be persisted to a database.  
* The time zone annotation used in the examples is just a suggestion. Feel free to use whatever annotation or convention you want.
* Time format - in the examples we use a specific data/time format. Feel free to change this to your liking.
* It is ok to assume that requests to the [POST /timezone](README.md#post-timezone) endpoint are not made concurrently. 

## How to submit  
* Clone this repo.
* Do whatever you want with it.
* Push it to **_your_** GitHub account.
* Send us the link.

## HTTP API
#### #GET /time  
Get the current time.  
If no _timezone_ parameter has been given, the time zone to be used is the _global time zone_ set by the [POST /timezone](README.md#post-timezone) request. 

The default _global time zone_ (in case it was not set) is the local time zone.

**_Parameters_**:  
(optional) timezone - the time zone to use.  
  
**_Response_**:  
A json with the following fields:  
time - the current time.  
timezone - the time zone.  

Request example:  
```GET /time?timezone=+02```
  
Response example: 
```
{
  time: 14:23:00-19/3/2007,
  timezone: +02
}
```

#### #POST /timezone
Set the global time zone to be used in the response for the [GET /time](README.md#get-time) request.

**_Parameters_**:  
timezone - the time zone to use.  
The parameter should be sent as json.  
Time zone notation - you can use whatever notation you wish.  
A simple way to go would be to use [UTC offsets](http://en.wikipedia.org/wiki/UTC_offset), e.g +01, +10, -08 etc.
  
Request example: 
```
POST /timezone
body: 
{
  timezone: -11
}
```

## Websocket API
#### setTimezone message
Set the time zone for the updates sent to the client who sent this message.

**_Action_**:  
setTimezone

**_Parameters_**:  
timezone - the time zone to use.  
For description, see _timezone_ param in [POST /timezone](README.md#post-timezone).

example: 
```
{
  action: 'setTimezone',
  timezone: -11
}
```

## Examples

**Request:**  
```GET /time```  

**Response:** 
```
{
  time: 12:20:00-27/5/2015,
  timezone: +01
}
```

**Request:**  
```GET /time?timezone=+03```  

**Response:** 
```
{
  time: 14:20:00-27/5/2015,
  timezone: +03
}
```

**Request:**  
```
POST /timezone
body: { timezone : -02 }
```

**Response:** 
```
200
```

**Request:**  
```GET /time (after previous POST request)```  

**Response:** 
```
{
  time: 09:20:00-27/5/2015,
  timezone: -02
}
```

