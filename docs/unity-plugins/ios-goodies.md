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

### Requirements

#### Sharing setting for images

!> If you want to share an image (`Texture2D`) from your project assets you have to set **Texture Type** to **Advanced** plus set **Read/Write Enabled** checkbox to true and **Compression Format** to **RGBA 32 bit** otherwise you will get errors.

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/share-image-settings-read-write.png "Image Settings")
![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/share-image-settings.png "Image Settings")

#### Frameworks to link

You must link against `MessageUI.framework` in XCode project (used by sending SMS or E-mail via controller). In the latest versions of Unity its handled automatically so you don't have to do anything, otherwise, check [How to link a framework in XCode project?](xxx)

### Sharing Text/Image/Text+Image

Shares text and image using iOS default share. You must provide callback, text and optionally image.

!> Only for iPad you can also select the popover screen position (`iPadScreenPosition` argument)

**Example:**

```csharp
Texture2D image = GetTexture(); // existing Texture2D to share
string message = "iOS Native Goodies by Dead Mosquito Games http://u3d.as/zMp #AssetStore";
var screenPosition = new Vector2(Screen.width / 2, Screen.height / 2);

IGShare.Share(
    activityType =>
    {
        if (string.IsNullOrEmpty(activityType))
        {
            Debug.Log("Posting was canceled or unknown result");
        }
        else
        {
            Debug.Log("DONE sharing, activity: " + activityType);
        }
    },
    error => Debug.LogError("Error happened when sharing activity: " + error),
    message, image, screenPosition);
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/share.jpg" width="256">
<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/twitter.jpg" width="256">

### Sharing Text+Link

Some programs (like Facebook Messenger) do not allow pre-filled text to be shared but allow sharing links. This method is solving this problem allowing to share only link on those apps that do not allow pre-filled text:

```csharp
var iosGoodiesAreGreat = "iOS Goodies are great!";
var dmgLink = "http://bit.ly/dmg-asset-store";
IGShare.ShareTextWithLink(activityType =>
    {
        if (string.IsNullOrEmpty(activityType))
        {
            Debug.Log("Posting was canceled or unknown result");
        }
        else
        {
            Debug.Log("DONE sharing, activity: " + activityType);
        }
    },
    error => Debug.LogError("Error happened when sharing activity: " + error), iosGoodiesAreGreat, dmgLink);
```

### Sending an Email

Send an email using default Mail application providing array of addresses, subject, body, optionally with cc and bcc recipients.

**Example:**

```csharp
var recipients = new[] {"x@gmail.com", "hello@gmail.com"};
var ccRecipients = new[] {"cc@gmail.com"};
var bccRecipients = new[] {"bcc@gmail.com", "bcc-guys@gmail.com"};
IGShare.SendEmail(recipients, "The Subject", "My email message!\nHello!", ccRecipients, bccRecipients);
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/email.png" width="256">

If you want to send an image attachment in the e-mail or/and receive callbacks about the state of sending use the `IGShare.SendEmailViaController()` method: 

```csharp
IGShare.SendEmailViaController(recipients, ccRecipients, bccRecipients, Subject, Message,
	() => {Debug.Log("Success");}, () => {Debug.Log("Cancelled");}, 
	() => {Debug.Log("Failure");}, () => {Debug.Log("Saved as draft");}, Image);
```

### Sending SMS using default app

Launches the the Messages app to send SMS with provided phone number and message. You can specify message body but its not guaranteed to work as specified in Apple documentation (However it works on latest iOS versions, test if it works for you).

**Example:**

```csharp
IGShare.SendSmsViaDefaultApp("123456789", "My message!");
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/message.jpg" width="256">

### Sending SMS using modal controller

Brings up the modal window to send SMS on top of your application (not launching Messages app).

**Example:**

```csharp
IGShare.SendSmsViaController("123456789", "Hello worksadk wa dwad !!!", () => Debug.Log("Success"),
    () => Debug.Log("Cancel"), () => Debug.Log("Failure"));
```

Result:

<img src="https://github.com/TarasOsiris/iOS-Goodies-Docs/blob/master/images/message.jpg" width="256">


## IGImagePicker.cs

This class allows you to get a `Texture2D` into Unity by [Taking Photo with Camera](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGImagePicker.cs#taking-photo-with-camera) / [Choosing Photo from Photo Library](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGImagePicker.cs#choosing-photo-from-photo-library) / [Choosing Photo from Photos Album](https://github.com/TarasOsiris/iOS-Goodies-Docs/wiki/IGImagePicker.cs#choosing-photo-from-photos-album)

**Please read the Requirements & Setup section below carefully before contacting support.**

## Requirements & Setup

In order to use Camera/Photo Gallery you need to add permission descriptions to your `Info.plist` file because of [iOS 10 Privace Settings](http://useyourloaf.com/blog/privacy-settings-in-ios-10/). 

Currently plugin contains a postprocessing script `IOSGoodies/Editor/Postprocessing/IGProjectPostprocessor.cs` that will add descriptions automatically for you. Modify the file to set descriptions to something meaningful and relevant to your project.

## Important Notes

Each of the methods accepts `compressionQuality` float variable (0 - mins minimum quality, 1 - means maximum quality). Use this to reduce memory pressure on your app, the lower the quality of the image the less memory it takes.

## Taking Photo with Camera

To take photo from camera and get the result as `Texture2D`:

```csharp
// Settings
const bool allowEditing = true;
const float compressionQuality = 0.8f;
const IGImagePicker.CameraType cameraType = IGImagePicker.CameraType.Front;
const IGImagePicker.CameraFlashMode flashMode = IGImagePicker.CameraFlashMode.On;

IGImagePicker.PickImageFromCamera(tex =>
    {
        Debug.Log("Successfully picked image from camera");
        image.sprite = SpriteFromTex2D(tex);
        // IMPORTANT! Call this method to clean memory if you are picking and discarding images
        Resources.UnloadUnusedAssets();
    }, 
    // Cancel callback
    () => Debug.Log("Picking image from camera cancelled"), 
    compressionQuality,
    allowEditing, cameraType, flashMode);
```

## Choosing Photo from Photo Library

To pick photo from photo library and get the result as `Texture2D`:

```csharp
const bool allowEditing = false;
const float compressionQuality = 0.5f;
var screenPosition = new Vector2(Screen.width, Screen.height); // On iPads ONLY you can choose screen position of popover

IGImagePicker.PickImageFromPhotoLibrary(tex =>
    {
        Debug.Log("Successfully picked image from photo library");
        image.sprite = SpriteFromTex2D(tex);
        // IMPORTANT! Call this method to clean memory if you are picking and discarding images
        Resources.UnloadUnusedAssets();
    },
    () => Debug.Log("Picking image from photo library cancelled"),
    compressionQuality,
    allowEditing, screenPosition);
```

## Choosing Photo from Photos Album

To pick photo from photos album and get the result as `Texture2D`:

```csharp
const bool allowEditing = true;
const float compressionQuality = 0.1f;
var screenPosition = new Vector2(Screen.width / 2, Screen.height / 2); // On iPads ONLY you can choose screen position of popover

IGImagePicker.PickImageFromPhotosAlbum(tex =>
    {
        Debug.Log("Successfully picked image from photos album");
        image.sprite = SpriteFromTex2D(tex);
        // IMPORTANT! Call this method to clean memory if you are picking and discarding images
        Resources.UnloadUnusedAssets();
    },
    () => Debug.Log("Picking image from photos album cancelled"),
    compressionQuality,
    allowEditing, screenPosition);
```

## Choosing video from photos album

Use `IGImagePicker.PickVideoFromPhotoLibrary()` method to pick videos from user gallery. You will receive a path to video file on disk as a success callback parameter

Example:

```csharp
public void PickVideoFromPhotosAlbum()
{
    const bool allowEditing = false;
    var screenPosition = new Vector2(Screen.width / 2, Screen.height / 2);

    IGImagePicker.PickVideoFromPhotoLibrary(path =>
        {
            Debug.Log("Successfully picked video from photos album: " + path);
            if (IGVideoEditor.CanEditVideoAtPath(path))
            {
                IGVideoEditor.EditVideo(path);
            }
            else
            {
                Debug.Log("Can't edit video at path: " + path );
            }
        },
        () => Debug.Log("Picking video from photos album cancelled"),
        allowEditing, screenPosition);
}
```

## Saving image to gallery

You can save the specified image to iOS gallery.

**NOTE!**

If you want to save an image (`Texture2D`) from your project assets you have to set **Texture Type** to **Advanced** plus set **Read/Write Enabled** checkbox to true and **Compression Format** to **RGBA 32 bit** otherwise you will get errors.

```csharp
public void SaveImageToPhotoLibrary()
{
    Texture2D testImage = GetSomeImage();
    IGImagePicker.SaveImageToGallery(testImage);
}
```

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

