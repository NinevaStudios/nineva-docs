[filename](common/common_ue_header.md ":include")

[filename](common/copy_plugin.md ":include")

# **Facebook Goodies**

Welcome to Facebook Goodies Documentation for Unreal Engine

---

# Setup

?> When dealing with setup we highly encourage you to also read/explore Facebook docs because it's the original source

## Create an app on the Facebook Developer portal

Log in on the [Facebook Developer portal](https://developers.facebook.com/apps/) and create an app.

![](images/facebook/FacebookCreateApp.jpg ":size=900")
![](images/facebook/FacebookCreateApp_2.jpg ":size=900")

## Copy your App ID and Client Token to UE Project settings

After you create your app you will see a screen of your app. Your App ID will be displayed at the top.

![](images/facebook/FacebookCreatedApp.jpg ":size=900")

Copy the Client Token (from `Settings -> Advanced -> Security` on the dashboard)

![](images/facebook/FacebookClientToken.jpg ":size=900")

Now go to your UE project `Edit -> Project Settings` and find the Facebook Goodies section in settings and paste your App ID and Facebook Client Token there.

![](images/facebook/FacebookPasteAppIdIntoSettings.jpg ":size=900")

When you add the App ID or Facebook Client Token to your settings `FacebookGoodies.xml` file will be created in the root folder of your project. This file is required for Android builds to work so do **not** delete it.

?> In case you don't have the default folder structure for your project there might be an issue when copying this file to the Android APK. If you experience crashes when your Android app is launching please verify that this file is present in the built APK located at \*Project\Intermediate\Android\APK\res\values\*. If there is absent, integrate the copy process into your build pipeline.

Enable Automatic Event Logging and Advertiser ID Collection if needed. These flags are also required for the event logging to work properly on iOS 14 and later.

## Fill in settings and add Android and iOS platforms

Go to `Settings -> Basic` and fill in all the information you need there. Now scroll down to the bottom of the page and add Android and iOS platforms to your app. You will see new sections for Android and iOS after you do this.

![](images/facebook/FacebookAppSettingsBasic.jpg ":size=900")
![](images/facebook/FacebookSettingsAddAndroidAndIos.jpg ":size=900")

## iOS Setup

Fill in the Bundle id that you are using for your app

![](images/facebook/FacebookSetupIos.jpg ":size=900")

## Android setup

![](images/facebook/FacebookAndroidSetup.png ":size=900")

In the Android section of your app you need to setup a few things

1. Put your application package name in this field
2. As a class name put `com.epicgames.unreal.GameActivity` (if using UE4 `com.epicgames.ue4.GameActivity`) which is the default name of the main activity
3. You need to find your keystore hashes which you are signing your app with. Please follow the [instructions on Facebook developers](https://developers.facebook.com/docs/android/getting-started/#release-key-hash).

## Products setup

Add all the products you need and follow the setup instructions for each product on the portal. Please ignore the Plist file modifications, the plugin will do them for you automatically.

![](images/facebook/FacebookAddProducts.jpg ":size=900")

If properly configured your app should have green checkboxes for Login and App Events in the products section.

![](images/facebook/FacebookConfiguredApp.png)

# **Login**

There are 3 blueprint nodes that you can use to login via Facebook:

- Simple login node. Use this node if you do not require any additional permissions. The user will only be asked to grant access to his public profile data.

![](images/facebook/FacebookLogin.png)

- Login with read permissions. Use this node if you require specific read permissions. If at any time you need to ask the user for more permissions simply call this node again with the new permissions specified.

![](images/facebook/FacebookLoginReadPermissions.png)

- Login with publish permissions. Use this node if you require specific publish permissions. If at any time you need to ask the user for more permissions simply call this node again with the new permissions specified.

![](images/facebook/FacebookLoginPublishPermissions.png)

?> Some permissions require your app to be reviewed by Facebook. Please consult the official Facebook documentation if your app is authorized to ask for specific permissions.

If the user successfully authorized you can get his login information in the form of an **Access Token** and **User ID** by calling an appropriate node. These nodes return empty strings if the user is not logged in.

![](images/facebook/FacebookLoginInfo.png)

To check if a user is logged in you can use the _Is logged in_ node. The _Logout_ node will log out the current user and will clear the active access token.

![](images/facebook/FacebookLoginOps.png)

To customize your app with user data you can run special queries. These queries will return a JSON string with the requested data. A query can also contain additional parameters and execute different Http methods (Get, Post, Delete). Please consult the official Facebook documentation for all available parameters and endpoints. A good starting point is the official [API Explorer](https://developers.facebook.com/tools/explorer?method=GET&path=me&version=v7.0), you can see what data is available to you and what permission you require to access it. Below is an example of a query that retrieves the users birthday:

![](images/facebook/FacebookRunQuery.png)

!> This plugin does not provide any helper methods to parse JSON responses.

For convenience, there is a special node that runs a query to retrieve the user's name, email (requires email read permission), and profile picture.

![](images/facebook/FacebookGetProfile.png)

To access certain data the user needs to grant specific permissions. You can check at any time what permissions were granted, declined, or expired by using these nodes:

![](images/facebook/FacebookPermissionOps.png)

# **App Events**

To test the events please use the [Event Manager](https://www.facebook.com/events_manager)

![](images/facebook/FacebookEventManager.jpg)

## Permissions

These are the methods to get permissions

![](images/facebook/FacebookPermissionOps.png)

## Log events

There are three convenience methods for event logging:

![](images/facebook/FacebookAnalytics1.png)

You can either just log event with a name, with name and parameters or with name, parameters and a sum value, that will be added for the events with this name.

## Set event flush behavior

By default, the logged events are flushed automatically by the SDK, but if you want, you can change it to manual and flush them whenever it's convenient for you.

![](images/facebook/FacebookAnalytics3.png)

## Use user data

You can also set, get and clear the personal user information:

![](images/facebook/FacebookAnalytics2.png)

## Application identifiers

You can get the Facebook Application ID and the unique identifier for the application installation on the current device for your needs:

![](images/facebook/FacebookAnalytics4.png)

## Advertiser tracking (iOS Only)

It's a privacy feature that allows users to control if advertisers receive data about activity generated by their devices.

Starting with iOS 14, you will need to set `AdvertiserTrackingEnabled` and log each time you give a device permission to share data with Facebook.

![](images/facebook/FacebookAdTracking.png)

## Setting app events on iOS 14

Apparently, a lot of people are having difficulties making app events work and testing them in [Event Manager](https://www.facebook.com/events_manager2). You can check this thread for more info: https://github.com/facebook/facebook-ios-sdk/issues/1667

To make the automatic app events and Testing events tab work, I did these things, and I am not sure which one actually had the impact:

- Make sure your project bundle ID matches the bundle ID in the iOS app setting on the dashboard

- In the plugin settings, enable all the iOS checkboxes as showed here and make sure you have added your ID Token

![](images/facebook/PluginSettingsForAppEvents.png ":size=900")

- Link your advertising account in the app Advanced settings

![](images/facebook/LinkAdManager.png)

- Try enabling 'Allow Tracking' option in the Facebook app on your device and having it open in the background while testing

![](images/facebook/FacebookAppTracking.png)

# **Share**

Official documentation on sharing can be found [here](https://developers.facebook.com/docs/sharing/).

The plugin allows users to share different types of content (photos, videos, links, stories).

?> Your app should not pre-fill any content to be shared. This is inconsistent with Facebook Platform Policy, see [Facebook Platform Policy, 2.3](https://developers.facebook.com/policy/#control).

To share content simply call the following function, providing the content to share, share dialog type, and the respective callbacks:

![](images/facebook/FacebookShare1.png)

## Share modes

!> Automatic, Native, Web and FeedWeb sharing modes are supported on both Android and iOS, while ShareSheet, Browser and FeedBrowser modes are supported only for iOS.

In most cases it is best to use the automatic mode and let the SDK select the best mode for you.

## Create share content assets

Most common cases for sharing are the photo and video sharing.

### Share Photo

You can create a Share Photo or Share Video objects using the following nodes:

![](images/facebook/FacebookShare2.png)

You can provide either a UTexture2D object or a full path to the image on the device for the Share Photo.

### Share Video

![](images/facebook/FacebookShare3.png)

The Share Video object is initialized with a full path to a video on the user's device.

?> On iOS it has to be in specific format: "assets-library://asset/asset.[FILE_EXTENSION]?id=[FILE_ID]&ext=[FILE_EXTENSION]" where FILE_EXTENSION is mov, or mp4, etc., and FILE_ID is the asset ID from PhotoLibrary. Example: assets-library://asset/asset.mp4?id=225640F8-597C-4E71-A1E9-490C7737B0E4&ext=mp4Create share content. Can be obtained as a result of the native image picker - under the deprecated UIImagePickerControllerReferenceURL key.

There are different types of contents available for sharing: photos, video, media, link, and story.
There are common functions for all of the types of content:

![](images/facebook/FacebookShare4.png)

### Share Link Content

You can share a link with an optional quote using the Share Link Content:

![](images/facebook/FacebookShare5.png)

### Share Media Content

!> Currently, only works on iOS due to Android SDK issues!

You can share multiple photos and videos using the Share Media Content:

![](images/facebook/FacebookShare6.png)

?> The maximum allowed amount of images and videos shared at the same time can be found in the official Facebook documentation for [Android](https://developers.facebook.com/docs/sharing/android#multimedia) and [iOS](https://developers.facebook.com/docs/sharing/ios#multimedia).

### Share Photo Content

You can share single or multiple photos using the Share Photo Content:

![](images/facebook/FacebookShare7.png)

### Share Story Content

You can share content to story using the Share Story Content and providing a sticker asset and either a background asset or background colors:

![](images/facebook/FacebookShare8.png)

?> On iOS sharing to stories works different under the hood, therefore, the callbacks are not received during it.

### Share Video Content

You can share video using the Share Video Content, providing a previously created Share Video object:

![](images/facebook/FacebookShare9.png)

# **Friends**

?> Only users with granted `user_friends` permission are displayed in fetched friends list.

![](images/facebook/FacebookUser_FriendsPermoission.png)

## User's Friends

To retrieve user's friends data use `GetUserFriendsData` method. Callback returns `FFBFetchedFriendsData` object that contains fetched data.

?> Note that facebook limits page to **25** friends in a single response.

![](images/facebook/FacebookFetchFriends1.png)
![](images/facebook/FacebookFetchedFriendsData.png)

?> If there are more friends to display `FFBFetchedFriendsData` response will contain string data in `Next` and/or `Previous` fields in `Paging` subobject. Otherwise those fields are empty strings.

### Paged Friends

In order to get another page of friends use `GetFriendsPagingData` method. It returns `FFBFetchedFriendsData` object in callback with updated friends list and new data for further paging requests.

?> `GetFriendsPagingData` method requires `UserId` parameter only for iOS!

# **Profile Picture**

To get user's profile picture call `GetProfilePicture` method. Callback returns image's url.

![](images/facebook/FacebookGetProfilePic.png)

# **Audience Network setup and changes for iOS 14.5**

First of all you should learn what changes with iOS 14.5 and how this impacts your app: https://www.facebook.com/business/help/2750680505215705?id=428636648170202

You can enable enable the Audience Network SDK inclusion in the Project Settings -> Facebook Goodies.

![](images/facebook/FacebookAudience.png)

This will:

- Add and link `FBAudienceNetwork.framework` to your project - see https://developers.facebook.com/docs/audience-network/get-started/ios
- Automatically add Facebook's SKAdNetwork IDs to your Xcode project’s `Info.plist`, see https://developers.facebook.com/docs/SKAdNetwork

?> Enabling Audience Network SDK will automatically add the `-lxml2` flag to the linker flags for iOS (both development and shipping builds).

Then you can call the `SetAudienceNetworkAdvertiserTrackingEnabled` function to enable or disable the Advertiser Tracking.

---

# Changelog


v 3.4.2

- ADDED Limited login functionality on iOS
- ADDED Exposed some more of user profile info (new GetProfile node)

---

v 2.2.2

- FIXED UPL issue on Android with provider

---

v 2.2.2

- FIXED Sharing for Android 11+
- UPDATE Android lib SDK version to 15.2.0
- FIXED Some warnings

---

v 2.2.0

- Update Android SDK to 14.1.1
- Fix some Android sharing issues
- Fix some string Android memory leaks and bugs

---

v 2.1.1

- FIXED 'exported' tag on acitivities for Android 12

v 1.5.4

- FIXED Sharing crashes on release Android builds due to proguad obfuscation

v 1.5.3

- ADD Client ID Token for Android

v 1.5.0

- UPDATE iOS and Android native SDKs
- ADD More settings and customizations

v 1.4.0

- UPDATE iOS SDK to version 9.0.1
- IMPROVED Addition of FBAudienceNetwork framework and other improvements for iOS 14.5. See https://docs.ninevastudios.com/#/ue-plugins/facebook-goodies?id=audience-network-setup-and-changes-for-ios-145

v 1.3.5

- UPDATE iOS SDK to version 8.2.2
- ADD Advertiser Tracking for iOS

v 1.2.2

- ADD Friends permission check for fetching friends demo blueprint
- FIXED FBShareLibrary packaging issue
- FIXED Fetch Friends use of removed reference

v 1.2.1

- ADD Fetch Friends functionality

v.1.2.0

- UPDATE iOS SDK to version 7.1.1
- FIXED iOS plist generation duplicates

v.1.1.0

- UPDATE Android SDK to version 7.1.0

?> This release uses AndroidX libraries and may cause conflicts with other Android plugins

v.1.0.0

- Initial release

---
