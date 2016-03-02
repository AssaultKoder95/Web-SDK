# Web-SDK

Javascript SDK enables quickly embeding the teamchat web version to your web application/website. You can seamlessly enable chat feature for all your users without having them explicitly signup for teamchat.



##Pre-requisites (for Javascript SDK)



###Create an App 

The Javascript SDK requires the end user's id and token. You need to register an app with teamchat to generate id and token for your end users.



From API Explorer of the developer console run "Create App" API to register an app. The API accepts one parameter "name" and returns response in following format.



```JSON

{

  "created": "2015-12-04T11:37:51.062Z",

  "domain": "56617b0fe4b0b9734bc9be16.tc.im",

  "id": "56617b0fe4b0b9734bc9be16",

  "lastUpdated": "2015-12-04T11:37:51.078Z",

  "name": "My Website App",

  "type": "APP",

  "verified": true

}

```



The "id" in above response will be required to register users of your newly created app.



### Add App Users

Once you create an app, you can add users and obtain their user id and token.



From API Explorer of the developer console run "Add App User" API to add a user. The API accept three parameters



* id - Your app id (see Create an App section above)

* email - Email address of the end user

* name - Name of the end user



API will return response in the following format



```JSON

{

  "compId": "56617b0fe4b0b9734bc9be16",

  "created": "2015-12-04T12:09:06.514Z",

  "displayEmail": "hitesh.khatri@teamchat.com",

  "email": "56618262e4b0b9734bc9be17@56617b0fe4b0b9734bc9be16.tc.im",

  "id": "56618262e4b0b9734bc9be17",

  "lastUpdated": "2015-12-04T12:09:07.255Z",

  "name": "Hitesh Khatri",

  "token": "XgPAy981745306",

  "userId": "37195",

  "validity": 2079950946992

}

```



Note, the email address supplied is stored as displayEmail and a dummy email address is generated and registered with the Teamchat for the user. 



The userId and token in response will be needed to launch teamchat using Javascript SDK.



You may require to add users on regular basis, so ideally you should integrated call to this API inside your backend services.



If you call the API with same user email address, it will return exising record. Also, You can pull list of all users registered under your app any time using "Get App Users" API



## Embed Javascript SDK and Launch Teamchat



### Embed the SDK

To embed the javascript SDK, you just need to include following markup in your web page. 



```html

<!--This is the  teamchat Javascript SDK-->

<script src="https://s3.amazonaws.com/teamchat-js-sdk/teamchat_sdk_1.0.js"></script>

```



Alternatively, you can download the Javacript file and host it on your server.



### Initialize Teamchat

Once you embed the SDK, you must initilize the teamchat. You will require user's token and userid to do this. (Refer "Add App User" above to know how to obtain  it) 



Use api.init method to initialize teamchat. See example below.



```html

<script type="text/javascript">	

	// This can also be included in document onload handler

	api.init("qvOXH467981093","37529");

</script>

```



Once initialized, all api calls will be executed in the  context of the user whose token was provided.



### Launch Rooms List

Once you initialized the teamchat, you need to launch teamchat room list UI inside your webpage. 



Create a DIV where teamchat will get embeded.



```html

<!--The DIV inside which the Teamchat will be loaded-->		

<div id="chatdiv" style="display:none;position:fixed;bottom:40px; right:32px; width:280px; height:450px; border:1px solid #00A2E8;">

</div>

```

Use api.embedroomlist method to launch room list. The method takes id of the div as parameter.



```html

<script type="text/javascript">	

//Embeds the room list in the chatdiv

api.embedroomlist("chatdiv"); 

</script>

```



## Complete Sample Code



```html

<!doctype html>

<html>

    <head>

        <title>Teamchat</title>

        <meta charset="utf-8">

        <meta name="viewport" content="width=device-width, initial-scale=1">

    </head>

    <body>

		<!--The chat logo on click of which teamchat will be launched-->				

        <div class="fixed">

            <a href="javascript:launchTeamchat();">

            <img id="logodiv" src="https://s3.amazonaws.com/teamchat-js-sdk/logo.png" style="position:fixed;bottom:10px;right:150px;width:24px;height:24px" />

            </a>

        </div>



		<!--The DIV inside which the Teamchat will be loaded-->		

        <div id="chatdiv" style="display:none;position:fixed;bottom:40px; right:32px; width:280px; height:450px; border:1px solid #00A2E8;">

        </div>



		<!--This is the  teamchat Javascript SDK-->

        <script src="https://s3.amazonaws.com/teamchat-js-sdk/teamchat_sdk_1.0.js"></script>

		

        <script type="text/javascript">	

			// This can also be included in document onload

			api.init("qvOXH467981093","37529");

		

			//function called on click of teamchat logo;

			function launchTeamchat(){

				document.getElementById("chatdiv").style.display="inline";

				//Embeds the room list in the chatdiv

				api.embedroomlist("chatdiv");  

			}		

        </script>

    </body>

</html>

```



## SDK Method Reference

Following methods are available in the SDK



### api.embedroomlist

The api accepts div id as parameter and loads the room list inside the div.

```javascript

api.embedroomlist("chatdiv");  

```

### api.embedroom

The api accepts div id and room id as parameter and loads the conversation UI of the room in the div.

```javascript

api.embedroom("chatdiv","566147d9dc43c39406f52ebb");

```

### api.embedmessage

The api accepts div id, room id and message id as parameter and loads a single message in the div

```javascript

api.embedmessage("d1","566147d9dc43c39406f52ebb","566155bfe4b0fe2dce16dfb6");

```



### api.createroom

The api creates a new group. It accepts group name, email address list and welcome message as parameter.

```javascript

api.createroom("New Group","email1@domain.com,email2@domain.on","Hello");

```



###api.addmembers

The api adds member to an existing group. It accepts room id, email address list as parameter. The user must be group admin to call this api.



```javascript

api.addmembers("566147d9dc43c39406f52ebb","sandeep@webaroo.com,one@gmail.com");

```



###api.sendmessage

The api sends a message in an existing group. It accepts room id and message text as paramter. The user must be member of the group to call this api.



``` javascript

api.sendmessage("566147d9dc43c39406f52ebb","Hello")

```



###api.rename

The api renames the group name. It accepts room id and room name as parameters. The user must be group admin to call this api.

``` javascript

api.rename("566147d9dc43c39406f52ebb","New Name")

```




