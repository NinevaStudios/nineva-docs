![iOS Goodies](https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/icon.jpg)

iOS Goodies Documentation

# Features

This section is a brief overview of the functionality provided by API. Click on a class name to visit full class documentation.

* App Interaction
	+ [IGApps.cs](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGApps.cs) - Opening various installed app, currently contains YouTube and FaceTime
	+ [IGMaps.cs](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGMaps.cs) - Opening Apple Maps application on specified address, location, performing search on map or displaying directions
	+ [IGShare.cs](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGShare.cs) - Social sharing text+image, sending SMS or E-mail etc.
	+ [IGShare.cs](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGShare.cs) - Social sharing text+image, sending SMS, E-mail, Facebook & Twitter sharing.
	+ [IGImagePicker.cs](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGImagePicker.cs) - Picking image from Camera/Photo Library/Photos Album and getting `Texture2D` as a result.
	+ [IGCalendar.cs](https://github.com/NinevaStudios/iOS-Goodies-Unity-Plugin-Documentation/wiki/IGCalendar.cs) - Creating, modifying and deleting calendar events and reminders.
	+ [IGVideoEditor.cs](https://github.com/NinevaStudios/iOS-Goodies-Unity-Plugin-Documentation/wiki/IGVideoEditor.cs) - Open default iOS video editor for local video
	+ [IGFilePicker.cs](https://github.com/NinevaStudios/iOS-Goodies-Unity-Plugin-Documentation/wiki/IGFilePicker.cs) - Interact with files in other applications (Files, OneDrive, etc.)

* UI
	+ [IGDialogs.cs](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGDialogs.cs) - Opening standard 1-2-or-3 button dialogs and getting a callback which button has been clicked
	+ [IGActionSheet.cs](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGActionSheet.cs) - Displays UIActionSheet to the user and allows to receive callbacks for clicked buttons.
	+ [IGDateTimePicker.cs](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGDateTimePicker.cs) - Showing native date, time, date-time, countdown timer pickers and getting callbacks

* Hardware
	* [IGFlashlight.cs](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGFlashlight.cs) - Using the flashlight
	* [IGHapticFeedback.cs](https://github.com/NinevaStudios/iOS-Goodies-Unity-Plugin-Documentation/wiki/IGHapticFeedback.cs) - Using the haptic feedback and vibration
	* [IGLocalAuthentication.cs](https://github.com/NinevaStudios/iOS-Goodies-Unity-Plugin-Documentation/wiki/IGLocalAuthentication.cs) - using Face Id/Touch Id

* Other
	* [IGDevice.cs](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGDevice.cs) - The functionality of iOS [UIDevice](https://developer.apple.com/reference/uikit/uidevice) class: 
	+ Battery State and Level
	+ Proximity sensor
	+ Device UUID
	+ Device name, localized name, system name, os version
	+ More...


**[Troubleshooting & FAQ](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/Troubleshooting-&-FAQ)**

# Getting started

After importing the unitypackage add the demo scene `IOSGoodies/Example/ExampleScene.unity` to your Build Settings, switch platform to iOS and export the XCode project.

![Build Settings](https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/setup/build_settings.png)

Open the XCode project afterwards and run it on your device.

# Configure settings

You can use new editor window to disable unused features to avoid unneeded dependencies in the XCode project. Please go to `Window -> iOS Goodies -> Edit Settings`.

<img src="https://github.com/NinevaStudios/iOS-Goodies-Unity-Plugin-Documentation/blob/master/images/editorSettings.jpg" width="400">

Here is the full list of features that can be enabled/disabled:
* Social Sharing / Email /SMS (`MessageUI.framework`)
* App rating dialog (`StoreKit.framework`)
* Image picker from Photos / Library / Camera, saving image to gallery
* Contact picker (`ContactsUI.framework`)
* Fingerprint / Face ID authentication (`LocalAuthentication.framework`)
* Calendar / Reminder access (`EventKit.framework`)

# App Interaction

## IGApps.cs

### Showing a dialer

Sometimes you need to prompt the user to call certain phone number.

Example:

```csharp
IGApps.OpenDialer("123456789");
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/dialer.PNG" width="256">

### Opening YouTube video

You can open YouTube video on your device in YouTube app or fallback to Safari if app is not installed.

**Note: If the YouTube video cannot be viewed on the device, iOS displays an appropriate warning message to the user.**

Example:

```csharp
const string videoId = "rZ2csdtP440";
IGApps.OpenYoutubeVideo(videoId);
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/youtube.png" width="256">

### Starting FaceTime Audio Call

Starts FaceTime audio call with provided email

Example:

```csharp
IGApps.StartFaceTimeAudioCall("user@example.com");
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/facetime.png" width="256">

### Starting FaceTime Video Call

Starts FaceTime video call with provided email

Example:

```csharp
IGApps.StartFaceTimeVideoCall("user@example.com");
```

### Opening Application Settings

You can open your application settings screen using:

```csharp
IGApps.OpenAppSettings();
```

### Opening Application App Store Page

You can open application app store page using:

```csharp
const string facebookAppId = "284882215";
IGApps.OpenAppOnAppStore(facebookAppId);
```

The **app Id** is the unique identifier of an iOS application. It can be found by searching for the app and finding the iTunes store web page for the app.

For the example below, the iTunes ID of Facebook app is 284882215:

https://itunes.apple.com/us/app/facebook/id284882215?mt=8

## IGMaps.cs

This class is used to show geographical locations and to generate driving directions between two points. You can read more about [map links in Apple Documentation](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html#//apple_ref/doc/uid/TP40007899-CH5-SW1)

You can use this class to help the user:

- Perform a search
- Get directions to a location
- View a specific location

### Opening a location (coordinates + optional label)

You can open maps location and optionally provide label to be displayed:

**Example:**

```csharp
const float latitude = 52.3781019f;
const float longitude = 4.8983588f;
var amsterdamCentralStation = new IGMaps.Location(latitude, longitude);
IGMaps.OpenMapLocation(amsterdamCentralStation, "Label-X", IGMaps.MapViewType.Satellite);
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/maps/label.png" width="256">

### Opening address (string address)


**Example:**

```csharp
const string address = "1,Infinite Loop,Cupertino,California";
IGMaps.OpenMapAddress(address, "Label-Y", IGMaps.MapViewType.Hybrid);
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/maps/address.png" width="256">

### Preforming simple search

**Example:**

```csharp
const string searchQuery = "Eiffel tower, Paris";
IGMaps.PerformSearch(searchQuery);
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/maps/location.png" width="256">

### Preforming simple search near location

**Example:**

```csharp
const float latitude = 50.894967f;
const float longitude = 4.341626f;
var atomiumLocation = new IGMaps.Location(latitude, longitude);
const int zoom = 5;
IGMaps.PerformSearch("Fish restaurant", atomiumLocation, zoom, IGMaps.MapViewType.Standard);
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/maps/search.png" width="256">

### Getting directions to a location


**Example:**

```csharp
// show route from Baker Street in London to Manchester
const string sourceAddress = "221B Baker Street, London, Great Britain";
const string destAddress = "Manchester, Great Britain";
IGMaps.GetDirections(destAddress, sourceAddress, IGMaps.TransportType.ByCar, IGMaps.MapViewType.Hybrid);
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/maps/directions.png" width="256">

## IGShare.cs


## IGImagePicker.cs


## IGAppStore.cs


## IGContactPicker.cs


## IGCalendar.cs


## IGVideoEditor.cs


## IGFilePicker.cs


**UI**

## IGDialogs.cs
## IGDateTimePicker.cs
## IGActionSheet.cs

***

**Hardware**

## IGFlashlight.cs
## IGHapticFeedback.cs
## IGLocalAuthentication.cs

***

**Other**
## IGDevice.cs

