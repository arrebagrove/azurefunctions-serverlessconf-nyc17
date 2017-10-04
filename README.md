# azurefunctions-serverlessconf-nyc17


This tutorial shows you how to integrate an Azure Function with your Flic button by posting a tweet to your Twitter account when the Flic is clicked. 

Prerequisites:
- Twitter account
- Flic button
- iPhone or Android smartphone with Flic app installed
- Azure account

# Create a Function App
Functions require a function app to host function execution. This can be done in the Azure portal.

1. Log in to the Azure portal and click the New button in the upper left-hand corner.
2. Click Compute > Function App. Then, configure your app settings: 
     - App Name: Create a globally unique name.
     - Subscription: Add a new or existing subscription.
     - Resource Group: Add a new or existing resource group.
     - Hosting plan: the Consumption Plan is recommended. 
     - Location: Choose a location near you.
     - Storage account: Create a globally unique name for the storage account that will be used by your function app, or use an existing account.
3. Click Create.

# C# 
# Create an HTTP Triggered Function
Now that the function app has been created, a function can be added to it. The template for an HTTP triggered function will execute when sent an HTTP request. This example will use C# to develop the function.

1. At the top of the portal, locate and click the magnifying glass button to search for 
your new function app. Enter the function app's name in the search bar to find and select it.
2. Expand your new function app, then click the + button next to functions.
3. Select HttpTrigger - C# function template.
4. Click Create.

# Implement Function
We will be using a NuGet package called linqtotwitter to call interact with the Twitter api. 

1. Select the new function, then click View files on the right hand side.
2. Click the add button and create a file named `project.json`.
3. Select project.json and add the following code to include linqtotwitter in the function:

```
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "linqtotwitter": "4.1.0"
      }
    }
   }
}
```

Save the file.

4. Navigate to run.csx and remove the existing code from lines 5 to 20, leaving only the initial `Run` method and the `using` statement.

5. Add the following code above `Run`:
```
using System.Net;
using System.Net.Http;
using LinqToTwitter;

private static TwitterContext _twitterCtx = null;
private static IDictionary<string, string> _messageMap;
```

6. Add the following code inside the `Run` method:

```
    if (_twitterCtx == null)
    {
        await SetupTwitterClient();
    }

    if (_messageMap.TryGetValue(messageType, out string message))
    {
        try
        {
            Status status = await _twitterCtx.TweetAsync("Testing a demo " + message);
            return req.CreateResponse(HttpStatusCode.OK);
        }
        catch (TwitterQueryException ex) {
            return req.CreateResponse(HttpStatusCode.InternalServerError);
        }
    }

    return req.CreateResponse(HttpStatusCode.BadRequest, "Invalid message");
```
7. Add the following method below the `Run` method to authenticate an associated Twitter account.:

```
private static async Task SetupTwitterClient()
{
    SingleUserAuthorizer authorizer = new SingleUserAuthorizer
    {
        CredentialStore = new SingleUserInMemoryCredentialStore
        {
            ConsumerKey = Environment.GetEnvironmentVariable("TwitterAppKey", EnvironmentVariableTarget.Process),
            ConsumerSecret = Environment.GetEnvironmentVariable("TwitterAppSecret", EnvironmentVariableTarget.Process),
            AccessToken = Environment.GetEnvironmentVariable("TwitterAccessToken", EnvironmentVariableTarget.Process),
            AccessTokenSecret = Environment.GetEnvironmentVariable("TwitterAccessTokenSecret", EnvironmentVariableTarget.Process)
        }
    };
    await authorizer.AuthorizeAsync();

    _messageMap = new Dictionary<string, string>
    {
        ["arrived"] = "Arrived at #ServerlessConf NYC. Trying out this #AzureFunctions demo cool",
        ["joinme"] = "You should join me at the Microsoft booth at #Serverlessconf NYC",
        ["azurefunctions"] = "Azure Functions is awesome!"
    };

    _twitterCtx = new TwitterContext(authorizer);
}
```

# Create Twitter App
1. Navigate to https://apps.twitter.com/ and create a new Twitter app by clicking the button on the top right of the page and filling out the form. The website is a required field, but not needed for the app so you may add a placeholder site. Click on the button at the end of the form to create the app.

2. In the app's details, navigate to the Keys and Access Tokens tab and click on the button in the Token Actions section to create an access token.

3. You will need the Consumer Key, Consumer Secret, Access Token, and Access Token Secret for the next step, keep this page open to copy these values into to your function configuration.

# Configure Function Keys
1. In the portal, navigate to the function app that hosts the recently created function.
2. In the function app overview tab, click on Application settings.
3. Scroll down to the Application settings section, click on the "+ Add new setting" button and add the keys from the Twitter app page as name and value pairs:
  - TwitterAppKey: Consumer Key
  - TwitterAppKeySecret: Consumer Secret
  - TwitterAccessToken: Access Token
  - TwitterAccessTokenSecret: Access Token Secret

# Configure Flic
1. In the Flic App

# Node



# Create Twitter App
# Configure Flic