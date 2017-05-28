---
layout: default
title: Example Calls
---

# GetNumberOfCurrentPlayers
Now that we have set up our server, let's use it to make some calls to the API. We'll start by making a call to the <font color="lightblue">GetNumberOfCurrentPlayers</font> method from the <font color="orange">ISteamUserStats</font> interface. This method should return the number of players currently playing a specific game. The game is specified by its appid which is passed in the url query string. You can quickly find a game's appid by searching for it in [steamdb.info/apps/](https://steamdb.info/apps/). For our example, we will use the game [Rocket League](http://store.steampowered.com/app/252950/Rocket_League/) with an appid of 252950. Let's modify our code to do this.

```js
var express = require('express');
var request = require('request');

var app = express();

app.get('/', function(req, res){
  //set the url for the request
  var url = 'https://api.steampowered.com/ISteamUserStats/GetNumberOfCurrentPlayers/v1/?key={YOURSTEAMAPIKEY}&appid=252950';
  //make the GET request
  request.get(url, function(error, steamHttpResponse, steamHttpBody){
    //After receiving a response from the API, sent the body of the response
    //as a response to our client.
    res.setHeader('Content-Type', 'application/json');
    res.send(steamHttpBody);
  });
});

app.set('port', 1234);
app.listen(app.get('port'), function(){
  console.log('Express started on port:' + app.get('port') + '; press Ctrl-C to terminate.');
});
```

Now, let's examine the data returned by the API.

```json
{
  "response": {
    "player_count": 18754,
    "result": 1
  }
}
```

Notice that since we didn't specify which format we wanted the data to be returned in, it defaulted to JSON. Now, the only interesting bit of data in this object lies within the inner **response** object. The **player_count** attribute of this object holds the current number of players playing this game. I'm not quite sure what the **result** attribute holds since there is no documentation describing this method. Cool, now let's try to get some data that's a little more interesting.

# GetGlobalAchievementPercentagesForApp
The <font color="lightblue">GetGlobalAchievementPercentagesForApp</font> method, also from the <font color="orange">ISteamUserStats</font> interface, returns all the achievements related to the specified game along with a percentage of player's who own the game and have successfully completed each achievement. To make this call, we only need to modify the line in our server where we assign the request url string. Note that this time the name of the parameter that we pass in the query string is named **gameid** instead of **appid** even though it is the same value.

```js
var url = 'https://api.steampowered.com/ISteamUserStats/GetGlobalAchievementPercentagesForApp/v2/?key={YOURSTEAMAPIKEY}&gameid=252950';
```

Let's take a look at some of the data we get back.

```json
{
  "achievementpercentages": {
    "achievements": [
      {
        "name": "Tinkerer",
        "percent": 71.356430053710938
      },
      {
        "name": "FirstTimer",
        "percent": 70.837776184082031
      },
      {
        "name": "PickMeUp",
        "percent": 65.873130798339844
      },
      {
        "name": "TripleThreat",
        "percent": 63.88018798828125
      },
      {
        "name": "Turbocharger",
        "percent": 62.543148040771484
      },
      {
        "name": "WallCrawler",
        "percent": 61.361373901367188
      },
      {
        "name": "Winner",
        "percent": 60.891704559326172
      }
    ]
  }
}
```

Note that not all achievements returned in the actual response are shown here. There were too many achievements associated with this game so only enough are shown for the purposes of this example. Again, since we didn't specify the format in our query string, we got the data back in JSON format. This time, the data we're after is contained within the **achievements** array, which itself is inside the **achievementpercentages** object. Each element in the **achievements** array is an object that contains the name of an achievement and the completion percent. Now this type of data can definitely find its use in a variety of different applications.