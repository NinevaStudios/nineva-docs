# Android Goodies PRO Documentation

# TODO - MOVE PICTURES FROM GITHUB!!!

**Join our [Discord server](https://discord.gg/SuJP9fY) and ask us anything!**

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/logo.png" width="256" height="256">

**Supported Android versions - API Level 14 and higher (4.0 Ice Cream Sandwich)**

[Android Goodies](https://www.assetstore.unity3d.com/#!/content/66662) is a collection of Android native methods that allow you to do different things like showing native dialogs, opening a certain location on the map sharing text and many more.

[**Download Demo App OnGoogle Play**](https://play.google.com/store/apps/details?id=com.deadmosquitogames.androidgoodiesdemo)

The plugin is in active development and new things will be added. Please [contact us](mailto:support@ninevastudios.com) if you have any questions or requests), but first be sure to **[Read the FAQ Section](#faq)**

## Supported Features:

* App Interaction
  * [AGAlarmClock.cs](#agalarmclockcs) - Methods to show all alarms, set alarm with all properties or set timer
  * [AGApps.cs](#agappscs) - Open app by package name, watch YouTube video in native app
  * [AGCalendar.cs](#agcalendarcs) - Class that allows to create calendar event with all required params or open calendar app on the provided date
  * [AGDialer.cs](TODO) - Dial or directly call phone number, check if user has phone app
  * [AGMaps.cs](TODO) - Open maps location, address, check if user has maps app
  * [AGSettings.cs](TODO) - Open any system settings screen
  * [AGShare.cs](TODO) - Native share text, text+image, tweet, email, send SMS etc, check if user has twitter, sms, email app installed.
  * [AGGallery.cs](TODO) - Pick photo from gallery app, save picture to gallery.
  * [AGContacts.cs](TODO) - Pick a contact from phone address book
  * [AGFilePicker.cs](TODO) - Different file pickers (audio file, video file, general file by mime-type)
* UI
  * [AGAlertDialog.cs](TODO) - Showing native alert dialogs with buttons/radiobuttons/checkboxes
  * [AGDateTimePicker.cs](TODO) - Showing date/time picker
  * [AGProgressDialog.cs](TODO) - Show spinner/horizontal progress bar
  * [AGLocalNotifications.cs](TODO) - Showing local notifications with info provided. Requires **android-support-v4.jar** in Android/Plugins folder.
  * [AGUIMisc.cs](TODO) - Showing toasts and immersive mode methods.
* Retrieving Info
   * [AGDeviceInfo.cs](TODO) - Various methods to get various info about device and apps (`android.os.Build`, `android.os.Build.Version`) and other
   * [AGEnvironment.cs](TODO) - Access to some `android.os.Environment` properties and methods
   * [AGNetwork.cs](TODO) - Internet connectivity and wifi related methods
   * [AGTelephony.cs](TODO) - Telephony related methods
* Hardware
  * [AGBattery.cs](TODO) - Get device battery charge level
  * [AGFlashLight.cs](TODO) - Enable and disable camera flashlight (as torch), check if device has flashlight
  * [AGGPS.cs](TODO) - Listen to GPS location changes, check if GPS enabled, get last known location.
  * [AGVibrator.cs](TODO) - Check if device has vibrator, vibrate or vibrate pattern
  * [AGCamera.cs](TODO) - Take photo (or thumbnail photo) from camera and receive it as `Texture2D`, various methods to check device camera capabilities.
* Storage
  * [AGFileUtils.cs](TODO) - Method to save Unity Texture2D to Android gallery
  * [AGSharedPrefs.cs](TODO) - Work natively with [Android Shared Preferences](https://developer.android.com/reference/android/content/SharedPreferences.html)
* Other
  * [AGPermissions.cs](TODO) - Manage, request and check [Android runtime permissions](https://developer.android.com/training/permissions/requesting.html).
  * [AGMediaRecorder.cs](TODO) - Record audio using device microphone
  * [AGPrintHelper.cs](TODO) - Print images and HTML-pages

## Advantages

* Clean and flexible API
* No overriding of Unity default activity
* Full source code included
* Well-written docs

## Running the demo scene

Check the [FAQ section](https://github.com/TarasOsiris/android-goodies-docs-PRO/wiki/FAQ) if you have any problems first!

To build and run the demo scene just connect your Android device switch target platform to Android in Unity build settings and run the example scene on device or emulator. No extra setup is required.

You will see the menu like this on device where you can start testing:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs-PRO/master/images/demo-home.png">

---

# **App Interaction**

## AGAlarmClock.cs

### Required permissions

In order to invoke the `AGAlarmClock.SetAlarm()` method, your app must have the `SET_ALARM` permission in your `AndroidManifest.xml`:

```xml
<uses-permission android:name="com.android.alarm.permission.SET_ALARM" />
```

### Showing all alarms

You can show the list of alarms invoking `AGAlarmClock.ShowAllAlarms()` method. To check if there is an app installed to handle showing alarms invoke `AGAlarmClock.CanShowListOfAlarms()`

### Setting an alarm

You can set an alarm by invoking `AGAlarmClock.SetAlarm()`
 method, optionally specifying days on which alarm has to be invoked (repeating alarm), if to vibrate, and whether to skip the UI when creating an alarm. To check if there is an app installed to handle setting alarms invoke `AGAlarmClock.CanSetAlarm()`

```csharp
var weekdays = new []
{
    AGAlarmClock.AlarmDays.Monday,
    AGAlarmClock.AlarmDays.Tuesday,
    AGAlarmClock.AlarmDays.Wednesday,
    AGAlarmClock.AlarmDays.Thursday,
    AGAlarmClock.AlarmDays.Friday,
};
const bool vibrate = true; // vibrate when alarm goes off
const bool skipUI = false; // don't skip the UI and show in the app to approve
AGAlarmClock.SetAlarm(1, 1, "My morning alarm", weekdays, vibrate, skipUI);
```

Result:

<img src="https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/set-alarm.png" width="400">

### Setting timer

To set a timer invoke `AGAlarmClock.SetTimer` providing time in seconds, label and whether to skip the UI. To check if there is an app installed to handle setting the timer invoke `AGAlarmClock.CanSetTimer()`

```csharp
const bool skipUI = true; // skip the UI and start the timer immediately
AGAlarmClock.SetTimer(5, "My awesome timer", skipUI);
```

Result:

<img src="https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/timer.png" width="400">

Please check [article about common Android intents](https://developer.android.com/guide/components/intents-common.html#Clock) for more information.

---

## AGApps.cs

Contains various methods to open other applications.

### Opening video in YouTube app

To open the video in YouTube app call this methods providing the video id:

```csharp
var videoId = "mZqjmyyJkQc";
AGApps.WatchYoutubeVideo(videoId);
```

### Opening users Instagram profile

To open the profile of any user on Instagram:

```csharp
const string profileId = "tarasleskivlviv";
AGApps.OpenInstagramProfile(profileId);
```

### Opening users Twitter profile

To open the profile of any user on Twitter:

```csharp
const string profileId = "Taras_Leskiv";
AGApps.OpenTwitterProfile(profileId);
```

### Opening users Facebook profile

To open the profile of any user on Facebook:

```csharp
const string profileId = "4"; // Mark Zuckerberg
AGApps.OpenFacebookProfile(profileId);
```

If YouTube app in not installed it will open the video with the browser.

### Opening any app on device provided package

You can open any app on the device if you know its package and this app has launcher activity:

```csharp
// Open twitter app
var package = "com.twitter.android";
AGApps.OpenOtherAppOnDevice(package, () => AGUIMisc.ShowToast("Could not launch " + package));
```

---

## AGCalendar.cs

### Checking if any calendar app is installed on device

To check if the user has an app that can show calendar date or create a calendar event call `AGCalendar.UserHasCalendarApp()`.

### Opening calendar at specific date

To open a calendar at specific date invoke `AGCalendar.OpenCalendarForDate` and provide a `DateTime` object of the date you want to open.

### Creating calendar event

The following details about the event can be provided:
* The event title.
* The event description.
* The event location.
* Array of email addresses that specify the invitees.
* The start time of the event
* The end time of the event
* Whether this is an all-day event.
* RRule (repeating rule)
* Access level
* Availability

To create an event you must create an `AGCalendar.EventBuilder` instance, set the required event properties and call `BuildAndShow()` method.

Example usage:

```csharp
var beginTime = DateTime.Now;
var endTime = beginTime.AddHours(2);
var eventBuilder = new AGCalendar.EventBuilder("Lunch with someone special", beginTime);
eventBuilder.SetEndTime(endTime);
eventBuilder.SetIsAllDay(false);
eventBuilder.SetLocation("Miami beach");
eventBuilder.SetDescription("Amazing lunch with a beautiful lady");
eventBuilder.SetEmails(new [] { "lol@gmail.com", "test@gmail.com" });
eventBuilder.SetRRule("FREQ=DAILY");
eventBuilder.SetAccessLevel(AGCalendar.EventAccessLevel.Public);
eventBuilder.SetAvailability(AGCalendar.EventAvailability.Free);
eventBuilder.BuildAndShow();
```

Result:

<img src="https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/calendar_event.png">

Please check [article about common Android intents](https://developer.android.com/guide/components/intents-common.html#Calendar) for more information.

---

## AGDialer.cs

### Required permissions

To place a phone call you need the following permission in your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.CALL_PHONE" />
```

You don't need this permission if you only want to show dialer with number, only when you need to app initiate a phone call immediately.

### Check if device can place a call

To check if the device can place phone calls call `AGDialer.UserHasPhoneApp()`

### Opening dialer

To open a dialer with specified phone number call

```csharp
string PhoneNumber = "123456789";
AGDialer.OpenDialer(PhoneNumber);
```

### Dialing phone number

To place a phone call immediately invoke


```csharp
string PhoneNumber = "123456789";
AGDialer.PlacePhoneCall(PhoneNumber);
```

---


## AGMaps.cs

Android Goodies provides simple and easy interface to [open a map application (providing coordinates or address) using intent](https://developer.android.com/guide/components/intents-common.html#Maps). If there is no application to handle the intent it will log an exception.

Functionality:
* [Check if user has maps app](#check-if-user-has-maps-app)
* [Open location with zoom level](#open-location-with-zoom-level)
* [Open location with label](#open-location-with-label)
* [Open location with address](#open-location-with-address)

---

### Check if user has maps app

To check if user has maps app installed invoke `AGMaps.UserHasMapsApp()`

### Open location with zoom level

Show the map at the given longitude and latitude at a certain zoom level. A zoom level of 1 shows the whole Earth, centered at the given lat,lng. The highest (closest) zoom level is 23.

Example:

```csharp
AGMaps.OpenMapLocation(47.6f, -122.3f, 9);
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/map_location_zoom.png" width="320" height="569">

---

### Open location with label

Show the map at the given longitude and latitude with a certain label.

Example:

```csharp
AGMaps.OpenMapLocationWithLabel(47.6f, -122.3f, "My Label");
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/map_address.png" width="320" height="569">

---

### Open location with address

Opens the map location with the provided address.

Example:

```csharp
AGMaps.OpenMapLocation("1st & Pike, Seattle");
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/map_label.png" width="320" height="569">

---

## AGSettings.cs

### Opening System Settings Screens

All settings screen action types are represented as constants (mirrored from [`android.provider.Settings`](https://developer.android.com/reference/android/provider/Settings.html)), e.g.

```csharp
/// <summary>
/// Show settings for accessibility modules.
/// </summary>
public const string ACTION_ACCESSIBILITY_SETTINGS = "android.settings.ACCESSIBILITY_SETTINGS"; // API 5
```

You can open any system settings section using this class like this:

```csharp
public void OnOpenWifiSettings()
{
    AGSettings.OpenSettingsScreen(AGSettings.ACTION_WIFI_SETTINGS);
}
```

The method above will open wi-fi settings. For all possible actions please check `AGSettings.cs` class.

Before opening settings screen you will most likely would check if the settings screen is available on current device or API level. Use `AGSettings.CanOpenSettingsScreen(string action)` to check if the screen is available.

### Opening main settings screen

To open main settings screen call `AGSettings.OpenSettings()`

### Opening any application details settings

You can open application details settings for any application installed on the device by calling `AGSettings.OpenApplicationDetailsSettings(string package)`

### Opening "Modify System Settings" activity

The method `AGSettings.OpenModifySystemSettingsActivity(Action onFailure)` opens system activity where user can allow apps to modify Android system settings.

<img src="https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/modify_system_settings.png" width="320">

### Getting/Setting System Screen Brightness

#### Getting system screen brightness

To get current system screen brightness use `AGSettings.GetSystemScreenBrightness()`. The value is between 0 and 1.

#### Setting system screen brightness

To modify system screen brightness you must have the following permission declared in your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.WRITE_SETTINGS"/>
```

Before setting the screen brightness you must also check if user has explicitly allowed your app to modify system settings by invoking `AGSettings.CanWriteSystemSettings()`, if it returns `false` you can prompt the user to allow your app to modify settings and open the **Modify System Settings Screen** by invoking `AGSettings.OpenModifySystemSettingsActivity()` where user can grant a permission to your app. After it you must check again. Shortly - do not invoke `AGSettings.SetSystemScreenBrightness()`
 until `AGSettings.CanWriteSystemSettings()` returns `true`.

Example usage:

```csharp
if (!AGSettings.CanWriteSystemSettings())
{
    AGUIMisc.ShowToast("Can't modify system settings because user did not allow it!");
    return;
}
AGSettings.SetSystemScreenBrightness(newBrightness);
```

### Troubleshooting

On some device to open bluetooth settings you need to add the following permission in `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
```

Add the permission above if you see the relevant message when trying to open bluetooth settings.

---


## AGShare.cs

### Required permissions

To share images you must add the following permission to your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

or modify Android Build Settings to set Write Access to `External (SDCard)`

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/external_storage.png "Storage")

### Sharing setting for images

If you want to share an image (`Texture2D`) from your project assets you have to set **Texture Type** to **Advanced** plus set **Read/Write Enabled** checkbox to true and **Compression Format** to **RGBA 32 bit** otherwise you will get errors.

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/share-image-settings-read-write.png "Image Settings")
![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/share-image-settings.png "Image Settings")

### Functionality

* [Native share](#native-share)
* [Sending email](#sending-email)
* [Sending SMS text](#sending-sms)
* [Sending MMS](#sending-mms)
* [Tweet](#tweeting)
* [Sharing Instagram photo](#sharing-instagram-photo)
* [Sending message via popular messengers](#sending-message-via-popular-messengers)
* [Sending message via arbitrary apps](#sending-message-via-arbitrary-apps)
* [Sharing video on the file system](#sharing-video-on-the-file-system)

### Checking if app is installed

You can check if user has SMS app, email app or twitter app installed:

```csharp
var hasSmsApp = AGShare.UserHasSmsApp();
var hasEmailApp = AGShare.UserHasEmailApp();
var isTwitterInstalled = AGShare.IsTwitterInstalled();
```

### Native share

[Share text or text with image using Android intent](https://developer.android.com/training/sharing/send.html#send-text-content). Shows chooser by default.

Share text:

```csharp
AGShare.ShareText("My subject", "My text to share");

```

Share text with image:

```csharp
public Texture2D image;

AGShare.ShareTextWithImage("My subject", "My text to share", image);
```

### Sending Email

Send an email using Android intent providing array of addresses, subject, body, optionally with cc and bcc recipients. Shows chooser by default.

```csharp
bool withChooser = true;
var recipients = new[] {"x@gmail.com", "hello@gmail.com"};
var ccRecipients = new[] {"cc@gmail.com"};
var bccRecipients = new[] {"bcc@gmail.com", "bcc-guys@gmail.com"};
AGShare.SendEmail(recipients, "subj", "body", image, withChooser, cc: ccRecipients, bcc: bccRecipients); // image, cc and bcc are optional
```

### Sending SMS

Send an SMS text message to specified phone number

Example usage:

```csharp
bool withChooser = true;
AGShare.SendSms("123123123", "Hello", withChooser);
```

### Sending MMS

Send an MMS message to specified phone number

Example usage:

```csharp
AGShare.SendMms("777-888-999", "Hello my friend", image, false, "MMS!");
```

### Tweeting

Android Goodies allows you to tweet text or text with image directly through official Twitter app. If the app is not installed the call will fallback to the browser one.

Example code:

```csharp
AGShare.Tweet("hello! I am tweeting like a boss!", image); // Image is optional
```
### Sharing Instagram photo

You can share photo in Instagram if it is installed. To check whether Instagram is installed use `AGShare.IsInstagramInstalled()`

```csharp
AGShare.ShareInstagramPhoto(image);
```

### Sending message via popular messengers

You can also send message to the most popular messaging apps. Supported apps: Facebook Messenger, WhatsApp, Telegram, Viber, SnapChat. Also methods to check whether the certain messaging app is installed in the form of `AGShare.Is${MessagingAppName}Installed()` e.g. `AGShare.IsWhatsAppInstalled()`

Example code:

```csharp
AGShare.SendFacebookMessageText(message);
AGShare.SendFacebookMessageImage(image);

AGShare.SendWhatsAppTextMessage(message);
AGShare.SendWhatsAppImageMessage(image);

AGShare.SendTelegramTextMessage(message);
AGShare.SendTelegramImageMessage(image);

AGShare.SendViberTextMessage(message);
AGShare.SendViberImageMessage(image);

AGShare.SendSnapChatTextMessage(message);
AGShare.SendSnapChatImageMessage(image);
```

### Sending message via arbitrary apps

You can also send the message and specify the app package to share it to a specific app, for this check `AGShare.SendTextMessageGeneric()` and `AGShare.SendImageGeneric()`


### Sharing video on the file system

You can also share video providing the path to it on your file system. File must exist to do so.


```csharp
// To test this method you put 'xxx.mov' video into your downloads folder
const string videoFileName = "/xxx.mov";
var filePath = Path.Combine(AGEnvironment.ExternalStorageDirectoryPath, AGEnvironment.DirectoryDownloads) + videoFileName;
Debug.Log("Sharing video: " + filePath + ", file exists?: " + File.Exists(filePath));
AGShare.ShareVideo(filePath, "My Video Title", "Upload Video");
```

---


## AGGallery.cs

Class to interact with Android gallery.

### Picking picture from gallery app

You can pick a picture from device and receive it in Unity as `ImagePickResult` object.

Method parameters are:

- a success callback where you will receive `ImagePickResult` object. Invoke `LoadTexture2D()` or `LoadThumbnailTexture2D()` or `LoadSmallThumbnailTexture2D()` to load `Texture2D` texture. Object also contains such info as display name of the picked image, width, height, size etc. Check the class itself for more information.
- a cancel callback which is invoked when user cancels taking photo or an error happens.
- an optional `ImageResultSize` - defaults to `ImageResultSize.Original` - specify this parameter if the photo takes up to much memory and needs to be downscaled before returned.
- an optional `bool` parameter whether to generate thumnails for the image (use `true` if you want a small thumbnail image like small avatar picture), defaults to `false`, must be set to `true` if you want to use `LoadThumbnailTexture2D()` or `LoadSmallThumbnailTexture2D()` methods.

```csharp
var imageResultSize = ImageResultSize.Max512;
AGGallery.PickImageFromGallery(
    selectedImage =>
    {
        var imageTexture2D = selectedImage.LoadTexture2D();

        string msg = string.Format("{0} was loaded from gallery with size {1}x{2}",
            selectedImage.OriginalPath, imageTexture2D.width, imageTexture2D.height);
        image.sprite = SpriteFromTex2D(imageTexture2D);

        // Clean up
        Resources.UnloadUnusedAssets();
    },
    errorMessage => AGUIMisc.ShowToast("Cancelled picking image from gallery: " + errorMessage),
    imageResultSize, shouldGenerateThumbnails);
```

### Saving image to gallery

To save `Texture2D` image to Android gallery use:

```csharp
var imageTitle = "Screenshot-" + System.DateTime.Now.ToString("yy-MM-dd-hh-mm-ss");
const string folderName = "Goodies";
AGGallery.SaveImageToGallery(texture2d, imageTitle, folderName, ImageFormat.JPEG);
```

### Making image visible in gallery

You can also refresh some other file to become noticed by gallery apps. Imagine you saved a picture somewhere on disk and now want for it to become visible in the gallery.

```csharp
AGGallery.RefreshFile(absoluteFilePath);
```

---


## AGContacts.cs

### Functionality

Class allows to pick a contacts from device's address book and receive the info in Unity.

Information returned:
* Contact display name
* URI of of the photo
* Contact list of phones if contact has any
* Contact list of emails if contact has any

### Requirements

For this feature to work you must add `READ_CONTACTS` permission to your `AndroidManifest.xml`

```xml
<uses-permission android:name="android.permission.READ_CONTACTS" />
```

### Example Usage

```csharp
public void OnPickContactFromAddressBook()
{
    AGContacts.PickContact(
        pickedContact =>
        {
            var msg = string.Format("Picked contact: {0}, photo URI: {1}, emails: {2}, phones: {3}",
                            pickedContact.DisplayName,
                            pickedContact.PhotoUri,
                            string.Join(",", pickedContact.Emails.ToArray()),
                            string.Join(",", pickedContact.Phones.ToArray())
                        );

            Debug.Log(msg);
            AGUIMisc.ShowToast(msg);

            if (!string.IsNullOrEmpty(pickedContact.PhotoUri))
            {
                // Load picture as Texture2D from uri
                var contactPicture = AGFileUtils.ImageUriToTexture2D(pickedContact.PhotoUri);
                image.sprite = SpriteFromTex2D(contactPicture);
            }
        },
        failureReason =>
        {
        });
}
```

---

## AGFilePicker.cs

This class contains different file pickers

### Requirements

* You must add this permission to your `AndroidManifest.xml` or set **Write Access** to **External (SD Card)** in Android build settings:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### Picking Audio Files

> Method does not load audio file into memory, you have to read the file yourself with file path received in the callback

Pick audio file from file system and returns the video information as a `AudioPickResult`.

Example:

```csharp
AGFilePicker.PickAudio(audioFile =>
    {
        var msg = "Audio file was picked: " + audioFile;
        Debug.Log(msg);
        AGUIMisc.ShowToast(msg);
    },
    error => AGUIMisc.ShowToast("Cancelled picking audio file"));
```

### Picking Video Files

> Method does not load video file into memory, you have to read the file yourself with file path received in the callback

Pick video from file system and returns the video information as a `VideoPickResult`.

`VideoPickResult` contains the following properties:
+ `OriginalPath`
+ `DisplayName`
+ `PreviewImagePath`, `PreviewImageThumbnailPath`, `PreviewImageSmallThumbnailPath` - only if requested generating thumbnails
+ `Width`, `Height`
+ `Orientation`
+ `Size`
+ `CreatedAt`

Example:

```csharp
AGFilePicker.PickVideo(videoFile =>
    {
        var msg = "Video file was picked: " + videoFile;
        Debug.Log(msg);
        AGUIMisc.ShowToast(msg);
        image.sprite = SpriteFromTex2D(videoFile.LoadPreviewImage());
    },
    error => AGUIMisc.ShowToast("Cancelled picking video file: " + error), generatePreviewImages);
```

### Picking Any Files

This file picker allows you to specify [mime-type]() of the files to be picked so it will limit the files that can be picked by the user, for example only PDF files. You receive `FilePickerResult` in the callback which has path to the file, its display name, size and creation timestamp.

> Method does not load file into memory, you have to read the file yourself with file path received in the callback

Example:

```csharp
const string mimeType = "application/pdf"; // pick only pdfs

AGFilePicker.PickFile(file =>
{
    var msg = "Picked file: " + file;
    Debug.Log(msg);
    AGUIMisc.ShowToast(msg);
}, error => AGUIMisc.ShowToast("Picking file: " + error), mimeType);
```

---

## AGMediaRecorder.cs

### Required permissions

In order use the `AGMediaRecorder` class, your app must have the `RECORD_AUDIO` permission in your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

### Start recording

To start audio recording - call `AGMediaRecorder.StartRecording()` method, providing full path to the file you are about to create:

```csharp
var downloadsDir = Path.Combine(AGEnvironment.ExternalStorageDirectoryPath, AGEnvironment.DirectoryDownloads);
var fullFilePath = Path.Combine(downloadsDir, "my_voice_recording.3gp");
AGMediaRecorder.StartRecording(fullFilePath);
```

You can optionally specify the resulting file output format and preferred audio encoder.

```csharp
AGMediaRecorder.StartRecording(fullFilePath, OutputFormat.THREE_GPP, AudioEncoder.AMR_NB);
```

### Stop recording

To finish the audio recording call `AGMediaRecorder.StopRecording()` method. 
The file you specified in the `AGMediaRecorder.StartRecording()` method will be created then.
This method returns bool value to indicate whether the call has been successful or not.
You should only call this method if you are sure that `AGMediaRecorder` is currently recording audio.
Otherwise, you will get an `java.lang.illegalStateException` exception.

```csharp
var recordingWasStopped = AGMediaRecorder.StopRecording();
AGUIMisc.ShowToast(recordingWasStopped ? "Stopped recording" : "Failed to stop recording");
```

---

# **UI**

## AGAlertDialog.cs

Class for displaying standard Android dialogs

### FAQ

* What if I don't want a callback? Should I pass `null`?
* No, you should just pass empty lambda `() => {}` and the callback will do nothing.

### Message dialog with positive button

Opens Android [AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html) with only positive button and optional dismiss callback.

Example usage:

```csharp
AGAlertDialog.ShowMessageDialog("Single Button", "This dialog has only positive button", "Ok",
    () => AndroidGoodiesMisc.ShowToast("Positive button Click"),
    () => AndroidGoodiesMisc.ShowToast("Dismissed"));
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/dialog_1_btn.png" width="320" height="569">

### Message dialog with positive and negative buttons

Opens Android [AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html) with positive and negative buttons and optional dismiss callback.

Example usage:

```csharp
AGAlertDialog.ShowMessageDialog("Two Buttons", "This dialog has positive and negative button",
    "Ok", () => AndroidGoodiesMisc.ShowToast("Positive button Click"),
    "No!", () => AndroidGoodiesMisc.ShowToast("Negative button Click"),
    () => AndroidGoodiesMisc.ShowToast("Dismissed"));
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/dialog_2_btn.png" width="320" height="569">

### Message dialog with positive, negative and neutral buttons

Opens Android [AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html) with positive, negative and neutral buttons and optional dismiss callback.

Example usage:

```csharp
AGAlertDialog.ShowMessageDialog("Three Buttons",
    "This dialog has positive, negative and neutral buttons button",
    "Ok", () => AndroidGoodiesMisc.ShowToast("Positive button Click"),
    "No!", () => AndroidGoodiesMisc.ShowToast("Negative button Click"),
    "Maybe!", () => AndroidGoodiesMisc.ShowToast("Neutral button Click"),
    () => AndroidGoodiesMisc.ShowToast("Dismissed"));
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/dialog_3_btn.png" width="320" height="569">

### Dialog with simple items chooser

Opens Android [AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html) with a list of items to choose one of them. The dialog is dismissed automatically after the user selects one of the options.

Example usage:

```csharp
string[] Colors = { "Red", "Green", "Blue" };

AGAlertDialog.ShowChooserDialog("Choose color", Colors,
    colorIndex => AndroidGoodiesMisc.ShowToast(Colors[colorIndex] + " selected"));
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/dialog_chooser.png" width="320" height="569">

### Dialog with radio buttons items chooser

Opens Android [AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html) with a list of items to choose one of them with radio buttons. Also has a positive button with callback.

Example usage:

```csharp
string[] Colors = { "Red", "Green", "Blue" };
int defaultSelectedItemIndex = 1;

AGAlertDialog.ShowSingleItemChoiceDialog("Choose color", Colors, defaultSelectedItemIndex,
    colorIndex => AndroidGoodiesMisc.ShowToast(Colors[colorIndex] + " selected"),
    "OK", () => AndroidGoodiesMisc.ShowToast("OK!"));
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/dialog_single_choice.png" width="320" height="569">

### Dialog with check boxes buttons items chooser

Opens Android [AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html) with a list of items to choose multiple of them with check boxes. Also has a positive button with callback.

Example usage:

```csharp
string[] Colors = { "Red", "Green", "Blue" };
bool[] initiallyCheckedItems = { false, true, false }; // second item is selected when dialog is shown

AGAlertDialog.ShowMultiItemChoiceDialog("Choose color", Colors,
    initiallyCheckedItems,
    (colorIndex, isChecked) => AndroidGoodiesMisc.ShowToast(Colors[colorIndex] + " selected? " + isChecked), "OK",
    () => AndroidGoodiesMisc.ShowToast("OK!"));
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/dialog_multi_choice.png" width="320" height="569">

### Progress dialog (spinner and horizontal progress bar)

You can also create [Android progress dialogs](https://developer.android.com/reference/android/app/ProgressDialog.html) as spinners as horizontal progress bars. When you want to dismiss the progress dialog call `Dismiss()` on dialog object to dismiss it.

Example usage of spinner (showing spinner for a given period of time):

```csharp
public void ShowSpinnerProgressDialog()
{
    StartCoroutine(ShowSpinnerForDuration());
}

private IEnumerator ShowSpinnerForDuration()
{
    // Create spinner dialog
    var spinner = AndroidProgressDialog.CreateSpinnerDialog("Spinner", "Call Dismiss() to cancel", AndroidDialogTheme.Dark);
    spinner.Show();
    // Spin for some time (do important work)
    yield return new WaitForSeconds(3.0f);
    // Dismiss spinner after all the background work is done
    spinner.Dismiss();
}
```

Example usage of horizontal progress bar progress dialog (example is showing incremental progress using coroutine):

```csharp
public void ShowHorizontalProgressDialog()
{
    StartCoroutine(ShowHorizontalForDuration());
}

private IEnumerator ShowHorizontalForDuration()
{
    const int dialogMaxProgress = 200;

    // Create progress bar dialog
    var progressBar = AndroidProgressDialog.CreateHorizontalDialog("Progress bar", "Call Dismiss() to cancel", dialogMaxProgress);
    progressBar.Show();

    const float progressDuration = 3.0f;
    float currentProgress = 0f;

    // Display incremental progress
    while (currentProgress < progressDuration)
    {
        currentProgress += Time.deltaTime;
        int progress = Mathf.CeilToInt((currentProgress / progressDuration) * dialogMaxProgress);
        progressBar.SetProgress(progress);
        yield return null;
    }

    // Dismiss spinner after all the background work is done
    progressBar.Dismiss();
}
```

### Specifying Dialog Theme

You can also specify the theme of the dialog (light or dark) as an optional parameter to all `Show...Dialog()` methods. If this parameter is not specified the default device theme is used.

Example:

```csharp
AGAlertDialog.ShowMessageDialog("Single Button", "This dialog has only positive button", "Ok",
    () => AndroidGoodiesMisc.ShowToast("Positive button Click"), () => AndroidGoodiesMisc.ShowToast("Dismissed"),
    AndroidDialogTheme.Dark);
```

---

## AGDateTimePicker.cs

Android goodies allows you to show default Android [TimePickerDialog](https://developer.android.com/reference/android/app/TimePickerDialog.html) and [DatePickerDialog](https://developer.android.com/reference/android/app/DatePickerDialog.html).

### Time Picker

Show the default Android [TimePickerDialog](https://developer.android.com/reference/android/app/TimePickerDialog.html)

Usage Example:

```csharp
public void OnTimePickClick()
{
    var now = DateTime.Now;
    var theme = AGDialogTheme.Default;
    var is24HourFormat = false;
    AGDateTimePicker.ShowTimePicker(now.Hour, now.Minute, OnTimePicked, OnTimePickCancel, theme, is24HourFormat);
}

private void OnTimePicked(int hourOfDay, int minute)
{
    var picked = new DateTime(2016, 11, 11, hourOfDay, minute, 00);
    timeText.text = picked.ToString("T");
}

private void OnTimePickCancel()
{
    timeText.text = "Cancelled picking time";
}
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/time_picker.png" width="320" height="569">

### Date Picker

Show the default Android [DatePickerDialog](https://developer.android.com/reference/android/app/DatePickerDialog.html)

Usage Example:

```csharp
public void OnPickDateClick()
{
    var now = DateTime.Now;
    AGDateTimePicker.ShowDatePicker(now.Year, now.Month, now.Day, OnDatePicked, OnDatePickCancel);
}

private void OnDatePicked(int year, int month, int day)
{
    var picked = new DateTime(year, month, day);
    dateText.text = picked.ToString("yyyy MMMMM dd");
}

private void OnDatePickCancel()
{
    dateText.text = "Cancelled picking date";
}
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/date_picker.png" width="320" height="569">

---


## AGProgressDialog.cs


## AGLocalNotifications.cs


## AGNotificationManager.cs


## AGWallpaperManager.cs


## AGUIMisc.cs


## AGShortcutManager.cs



# **Retrieving Info**

## AGDeviceInfo.cs


## AGEnvironment.cs


## AGNetwork.cs


## AGTelephony.cs



# **Hardware**

## AGBattery.cs


## AGFlashLight.cs


## AGGPS.cs


## AGVibrator.cs


## AGCamera.cs


## AGFingerprintScanner.cs



# **Storage**

## AGFileUtils.cs


## AGSharedPrefs.cs



# **Other**

## AGPermissions.cs


## AGPrintHelper.cs


 # **FAQ**

 ## Can't Build my project after importing Android Goodies

**Q:** I can't Build my project after importing Android Goodies, how do I fix this?

When your build fails, please check the console, it will have a very long error message why the build failed. Find the part with **stderr[** and read the message.

**A #1:** Most likely you can't build because you already have Android Support Library Version 4 in your project, try removing **android-support-v4.jar** that comes with my package in Android/Plugins folder and rebuilding the project.

**A #2:** The error is *java.lang.UnsupportedClassVersionError: com/android/dx/command/Main : Unsupported major.minor version 52.0* - this means you need to update your Java to the latest JDK8 ([Download here and install](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)). **Don't forget to set the JDK inside Unity -> Preferences -> External Tools -> JDK to newly downloaded version**

---

## I click buttons in the demo but nothing happens

**Q:** I click buttons in the demo but nothing happens, why?

**A:** 1) Make sure you switch to Android Build Target in Build Settings. If this doesn't work please right-click on `AndroidGoodies` folder and click "Reimport" from context menu so it could recompile.

---

## AndroidManifest.xml

**Q:** Should I import `AndroidManifest.xml` that comes with the package?

**A:** Yes, if you intend to build the demo app. Although, when you import the package it will override your `AndroidManifest.xml` in `Plugins/Android` folder. Make sure to backup your existing manifest or uncheck `AndroidManifest.xml` in `Plugins/Android` when importing the package.

---

## Permissions

**Q:** How do I found out what permissions I have to put in my `AndroidManifest.xml` file?

**A:** Every class has permissions listed in xml reference docs, also check the documenation for specific class in the right column of this documentation. Also check the permissions provided in `AndroidManifest.xml` in `Plugins/Android`folder - it contains all the required permissions for all the functions.

---

## Overriding `UnityPlayerActivity`

**Q:** Do I have to override `UnityPlayerActivity`for any of the functions to work properly?

**A:** No, Android Native Goodies does not require to override `UnityPlayerActivity`.