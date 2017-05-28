---
layout: default
---

# Register for an API key
As is the case with almost every web API, a unique key is required to make calls to methods within the API. This key serves as a way to identify who is making the API calls. To obtain a key, simply [fill out this form](http://steamcommunity.com/dev/apikey). Note that you must first have a Steam account to link with your key. You can sign in with an existing account or create a new one. Also note that you are limited to 100,000 calls per day with your key, as described in the [API Terms of Use](https://steamcommunity.com/dev/apiterms). For this reason, you will want to avoid sharing your key.

# API Call Format
API methods are called through a GET or POST (call specific) request made to http://api.steampowered.com/. The URL request string is formatted as follows:

<p style="font-size:12px">http://api.steampowered.com/<font color="orange">&lt;interface name&gt;</font>/<font color="lightblue">&lt;method name&gt;</font>/v<font color="lightgreen">&lt;version&gt;</font>/?key=<font color="yellow">&lt;api key&gt;</font>&format=<font color="pink">&lt;format&gt;</font></p>

## <font color="orange">Interface Name
Steam categorizes their API into "Interfaces" which groups together related API calls. This is where you'll provide the name of the interface that the specific method you want to use belongs to.

## <font color="lightblue">Method Name
The name of the method you want to call.

## <font color="lightgreen">Version
The version of the method you want to call.

## <font color="yellow">Api Key
The unique API key that serves as an identifier for the caller.

## <font color="pink">Format
The format that you want to receive the requested data. The availabe formats are:
- JSON
- XML
- VDF

You can find more information about the data formats returned in the [Steam Web API Wiki](https://developer.valvesoftware.com/wiki/Steam_Web_API#Formats).
If you do not specify a format, the API will default to JSON.

## Additional Parameters
Any additional parameters, required or optional, expected by the method you want to call should be appended to the end of the url delimited by the '&' character.