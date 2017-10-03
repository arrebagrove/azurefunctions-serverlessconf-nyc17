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
```

# Create Twitter App
# Configure Flic