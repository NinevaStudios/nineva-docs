# **Facebook Goodies**

# **Login**

There are 3 blueprint nodes that you can use to login via Facebook:

* Simple login node. Use this node if you do not require any additional permissions. The user will only be asked to grant access to his public profile data.

![](images/facebook/FbLogin.png)

* Login with read permissions. Use this node if you require specific read permissions. If at any time you need to ask the user for more permissions simply call this node again with the new permissions specified.

![](images/facebook/FbLoginReadPermissions.png)

* Login with publish permissions. Use this node if you require specific publish permissions. If at any time you need to ask the user for more permissions simply call this node again with the new permissions specified.

![](images/facebook/FbLoginPublishPermissions.png)

?> Some permissions require your app to be reviewed by Facebook. Please consult the official Facebook documentation if your app is authorized to ask for specific permissions.

If the user successfully authorized you can get his login information in the form of an **Access Token** and **User ID** by calling an appropriate node. These nodes return empty strings if the user is not logged in.

![](images/facebook/FbLoginInfo.png)

To check if a user is logged in you can use the *Is logged in* node. The *Logout* node will log out the current user and will clear the active access token.

![](images/facebook/FbLoginOps.png)

To customize your app with user data you can run special queries. These queries will return a JSON string with the requested data. A query can also contain additional parameters and execute different Http methods (Get, Post, Delete). Please consult the official Facebook documentation for all available parameters and endpoints. A good starting point is the official [API Explorer](https://developers.facebook.com/tools/explorer?method=GET&path=me&version=v7.0), you can see what data is available to you and what permission you require to access it. Below is an example of a query that retrieves the users birthday:

![](images/facebook/FbRunQuery.png)

!> This plugin does not provide any helper methods to parse JSON responses.

For convenience, there is a special node that runs a query to retrieve the user's name, email (requires email read permission), and profile picture.

![](images/facebook/FbGetProfile.png)

To access certain data the user needs to grant specific permissions. You can check at any time what permissions were granted, declined, or expired by using these nodes:

![](images/facebook/FbPermissionOps.png)

# **Analytics**

# **Sharing**
