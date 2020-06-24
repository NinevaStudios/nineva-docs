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
  + [AGAlarmClock.cs](#agalarmclockcs) - Methods to show all alarms, set alarm with all properties or set timer
  + [AGApps.cs](#agappscs) - Open app by package name, watch YouTube video in native app
  + [AGCalendar.cs](#agcalendarcs) - Class that allows to create calendar event with all required params or open calendar app on the provided date
  + [AGDialer.cs](TODO) - Dial or directly call phone number, check if user has phone app
  + [AGMaps.cs](TODO) - Open maps location, address, check if user has maps app
  + [AGSettings.cs](TODO) - Open any system settings screen
  + [AGShare.cs](TODO) - Native share text, text+image, tweet, email, send SMS etc, check if user has twitter, sms, email app installed.
  + [AGGallery.cs](TODO) - Pick photo from gallery app, save picture to gallery.
  + [AGContacts.cs](TODO) - Pick a contact from phone address book
  + [AGFilePicker.cs](TODO) - Different file pickers (audio file, video file, general file by mime-type)
* UI
  + [AGAlertDialog.cs](TODO) - Showing native alert dialogs with buttons/radiobuttons/checkboxes
  + [AGDateTimePicker.cs](TODO) - Showing date/time picker
  + [AGProgressDialog.cs](TODO) - Show spinner/horizontal progress bar
  + [AGLocalNotifications.cs](TODO) - Showing local notifications with info provided. Requires **android-support-v4.jar** in Android/Plugins folder.
  + [AGUIMisc.cs](TODO) - Showing toasts and immersive mode methods.
* Retrieving Info

   * [AGDeviceInfo.cs](TODO) - Various methods to get various info about device and apps ( `android.os.Build` , `android.os.Build.Version` ) and other
   * [AGEnvironment.cs](TODO) - Access to some `android.os.Environment` properties and methods
   * [AGNetwork.cs](TODO) - Internet connectivity and wifi related methods
   * [AGTelephony.cs](TODO) - Telephony related methods

* Hardware
  + [AGBattery.cs](TODO) - Get device battery charge level
  + [AGFlashLight.cs](TODO) - Enable and disable camera flashlight (as torch), check if device has flashlight
  + [AGGPS.cs](TODO) - Listen to GPS location changes, check if GPS enabled, get last known location.
  + [AGVibrator.cs](TODO) - Check if device has vibrator, vibrate or vibrate pattern
  + [AGCamera.cs](TODO) - Take photo (or thumbnail photo) from camera and receive it as `Texture2D` , various methods to check device camera capabilities.
* Storage
  + [AGFileUtils.cs](TODO) - Method to save Unity Texture2D to Android gallery
  + [AGSharedPrefs.cs](TODO) - Work natively with [Android Shared Preferences](https://developer.android.com/reference/android/content/SharedPreferences.html)
* Other
  + [AGPermissions.cs](TODO) - Manage, request and check [Android runtime permissions](https://developer.android.com/training/permissions/requesting.html).
  + [AGMediaRecorder.cs](TODO) - Record audio using device microphone
  + [AGPrintHelper.cs](TODO) - Print images and HTML-pages

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

In order to invoke the `AGAlarmClock.SetAlarm()` method, your app must have the `SET_ALARM` permission in your `AndroidManifest.xml` :

``` xml
<uses-permission android:name="com.android.alarm.permission.SET_ALARM" />
```

### Showing all alarms

You can show the list of alarms invoking `AGAlarmClock.ShowAllAlarms()` method. To check if there is an app installed to handle showing alarms invoke `AGAlarmClock.CanShowListOfAlarms()` 

### Setting an alarm

You can set an alarm by invoking `AGAlarmClock.SetAlarm()` 
 method, optionally specifying days on which alarm has to be invoked (repeating alarm), if to vibrate, and whether to skip the UI when creating an alarm. To check if there is an app installed to handle setting alarms invoke `AGAlarmClock.CanSetAlarm()` 

``` csharp
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

``` csharp
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

``` csharp
var videoId = "mZqjmyyJkQc";
AGApps.WatchYoutubeVideo(videoId);
```

### Opening users Instagram profile

To open the profile of any user on Instagram:

``` csharp
const string profileId = "tarasleskivlviv";
AGApps.OpenInstagramProfile(profileId);
```

### Opening users Twitter profile

To open the profile of any user on Twitter:

``` csharp
const string profileId = "Taras_Leskiv";
AGApps.OpenTwitterProfile(profileId);
```

### Opening users Facebook profile

To open the profile of any user on Facebook:

``` csharp
const string profileId = "4"; // Mark Zuckerberg
AGApps.OpenFacebookProfile(profileId);
```

If YouTube app in not installed it will open the video with the browser.

### Opening any app on device provided package

You can open any app on the device if you know its package and this app has launcher activity:

``` csharp
// Open twitter app
var package = "com.twitter.android";
AGApps.OpenOtherAppOnDevice(package, () => AGUIMisc.ShowToast("Could not launch " + package));
```

---

## AGCalendar.cs

### Checking if any calendar app is installed on device

To check if the user has an app that can show calendar date or create a calendar event call `AGCalendar.UserHasCalendarApp()` .

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

``` csharp
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

To place a phone call you need the following permission in your `AndroidManifest.xml` :

``` xml
<uses-permission android:name="android.permission.CALL_PHONE" />
```

You don't need this permission if you only want to show dialer with number, only when you need to app initiate a phone call immediately.

### Check if device can place a call

To check if the device can place phone calls call `AGDialer.UserHasPhoneApp()` 

### Opening dialer

To open a dialer with specified phone number call

``` csharp
string PhoneNumber = "123456789";
AGDialer.OpenDialer(PhoneNumber);
```

### Dialing phone number

To place a phone call immediately invoke

``` csharp
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

Show the map at the given longitude and latitude at a certain zoom level. A zoom level of 1 shows the whole Earth, centered at the given lat, lng. The highest (closest) zoom level is 23.

Example:

``` csharp
AGMaps.OpenMapLocation(47.6f, -122.3f, 9);
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/map_location_zoom.png" width="320" height="569">

---

### Open location with label

Show the map at the given longitude and latitude with a certain label.

Example:

``` csharp
AGMaps.OpenMapLocationWithLabel(47.6f, -122.3f, "My Label");
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/map_address.png" width="320" height="569">

---

### Open location with address

Opens the map location with the provided address.

Example:

``` csharp
AGMaps.OpenMapLocation("1st & Pike, Seattle");
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/map_label.png" width="320" height="569">

---

## AGSettings.cs

### Opening System Settings Screens

All settings screen action types are represented as constants (mirrored from [ `android.provider.Settings` ](https://developer.android.com/reference/android/provider/Settings.html)), e.g.

``` csharp
/// <summary>
/// Show settings for accessibility modules.
/// </summary>
public const string ACTION_ACCESSIBILITY_SETTINGS = "android.settings.ACCESSIBILITY_SETTINGS"; // API 5
```

You can open any system settings section using this class like this:

``` csharp
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

To get current system screen brightness use `AGSettings.GetSystemScreenBrightness()` . The value is between 0 and 1.

#### Setting system screen brightness

To modify system screen brightness you must have the following permission declared in your `AndroidManifest.xml` :

``` xml
<uses-permission android:name="android.permission.WRITE_SETTINGS"/>
```

Before setting the screen brightness you must also check if user has explicitly allowed your app to modify system settings by invoking `AGSettings.CanWriteSystemSettings()` , if it returns `false` you can prompt the user to allow your app to modify settings and open the **Modify System Settings Screen** by invoking `AGSettings.OpenModifySystemSettingsActivity()` where user can grant a permission to your app. After it you must check again. Shortly - do not invoke `AGSettings.SetSystemScreenBrightness()` 
 until `AGSettings.CanWriteSystemSettings()` returns `true` .

Example usage:

``` csharp
if (!AGSettings.CanWriteSystemSettings())
{
    AGUIMisc.ShowToast("Can't modify system settings because user did not allow it!");
    return;
}
AGSettings.SetSystemScreenBrightness(newBrightness);
```

### Troubleshooting

On some device to open bluetooth settings you need to add the following permission in `AndroidManifest.xml` :

``` xml
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
```

Add the permission above if you see the relevant message when trying to open bluetooth settings.

---

## AGShare.cs

### Required permissions

To share images you must add the following permission to your `AndroidManifest.xml` :

``` xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

or modify Android Build Settings to set Write Access to `External (SDCard)` 

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/external_storage.png "Storage")

### Sharing setting for images

If you want to share an image ( `Texture2D` ) from your project assets you have to set **Texture Type** to **Advanced** plus set **Read/Write Enabled** checkbox to true and **Compression Format** to **RGBA 32 bit** otherwise you will get errors.

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

``` csharp
var hasSmsApp = AGShare.UserHasSmsApp();
var hasEmailApp = AGShare.UserHasEmailApp();
var isTwitterInstalled = AGShare.IsTwitterInstalled();
```

### Native share

[Share text or text with image using Android intent](https://developer.android.com/training/sharing/send.html#send-text-content). Shows chooser by default.

Share text:

``` csharp
AGShare.ShareText("My subject", "My text to share");

```

Share text with image:

``` csharp
public Texture2D image;

AGShare.ShareTextWithImage("My subject", "My text to share", image);
```

### Sending Email

Send an email using Android intent providing array of addresses, subject, body, optionally with cc and bcc recipients. Shows chooser by default.

``` csharp
bool withChooser = true;
var recipients = new[] {"x@gmail.com", "hello@gmail.com"};
var ccRecipients = new[] {"cc@gmail.com"};
var bccRecipients = new[] {"bcc@gmail.com", "bcc-guys@gmail.com"};
AGShare.SendEmail(recipients, "subj", "body", image, withChooser, cc: ccRecipients, bcc: bccRecipients); // image, cc and bcc are optional
```

### Sending SMS

Send an SMS text message to specified phone number

Example usage:

``` csharp
bool withChooser = true;
AGShare.SendSms("123123123", "Hello", withChooser);
```

### Sending MMS

Send an MMS message to specified phone number

Example usage:

``` csharp
AGShare.SendMms("777-888-999", "Hello my friend", image, false, "MMS!");
```

### Tweeting

Android Goodies allows you to tweet text or text with image directly through official Twitter app. If the app is not installed the call will fallback to the browser one.

Example code:

``` csharp
AGShare.Tweet("hello! I am tweeting like a boss!", image); // Image is optional
```

### Sharing Instagram photo

You can share photo in Instagram if it is installed. To check whether Instagram is installed use `AGShare.IsInstagramInstalled()` 

``` csharp
AGShare.ShareInstagramPhoto(image);
```

### Sending message via popular messengers

You can also send message to the most popular messaging apps. Supported apps: Facebook Messenger, WhatsApp, Telegram, Viber, SnapChat. Also methods to check whether the certain messaging app is installed in the form of `AGShare.Is${MessagingAppName}Installed()` e.g. `AGShare.IsWhatsAppInstalled()` 

Example code:

``` csharp
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

``` csharp
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

* a success callback where you will receive `ImagePickResult` object. Invoke `LoadTexture2D()` or `LoadThumbnailTexture2D()` or `LoadSmallThumbnailTexture2D()` to load `Texture2D` texture. Object also contains such info as display name of the picked image, width, height, size etc. Check the class itself for more information.
* a cancel callback which is invoked when user cancels taking photo or an error happens.
* an optional `ImageResultSize` - defaults to `ImageResultSize.Original` - specify this parameter if the photo takes up to much memory and needs to be downscaled before returned.
* an optional `bool` parameter whether to generate thumnails for the image (use `true` if you want a small thumbnail image like small avatar picture), defaults to `false` , must be set to `true` if you want to use `LoadThumbnailTexture2D()` or `LoadSmallThumbnailTexture2D()` methods.

``` csharp
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

``` csharp
var imageTitle = "Screenshot-" + System.DateTime.Now.ToString("yy-MM-dd-hh-mm-ss");
const string folderName = "Goodies";
AGGallery.SaveImageToGallery(texture2d, imageTitle, folderName, ImageFormat.JPEG);
```

### Making image visible in gallery

You can also refresh some other file to become noticed by gallery apps. Imagine you saved a picture somewhere on disk and now want for it to become visible in the gallery.

``` csharp
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

``` xml
<uses-permission android:name="android.permission.READ_CONTACTS" />
```

### Example Usage

``` csharp
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

``` xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### Picking Audio Files

!> Method does not load audio file into memory, you have to read the file yourself with file path received in the callback

Pick audio file from file system and returns the video information as a `AudioPickResult` .

Example:

``` csharp
AGFilePicker.PickAudio(audioFile =>
    {
        var msg = "Audio file was picked: " + audioFile;
        Debug.Log(msg);
        AGUIMisc.ShowToast(msg);
    },
    error => AGUIMisc.ShowToast("Cancelled picking audio file"));
```

### Picking Video Files

!> Method does not load video file into memory, you have to read the file yourself with file path received in the callback

Pick video from file system and returns the video information as a `VideoPickResult` .

`VideoPickResult` contains the following properties:

* `OriginalPath` 
* `DisplayName` 
* `PreviewImagePath` , `PreviewImageThumbnailPath` , `PreviewImageSmallThumbnailPath` - only if requested generating thumbnails
* `Width` , `Height` 
* `Orientation` 
* `Size` 
* `CreatedAt` 

Example:

``` csharp
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

!> Method does not load file into memory, you have to read the file yourself with file path received in the callback

Example:

``` csharp
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

In order use the `AGMediaRecorder` class, your app must have the `RECORD_AUDIO` permission in your `AndroidManifest.xml` :

``` xml
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

### Start recording

To start audio recording - call `AGMediaRecorder.StartRecording()` method, providing full path to the file you are about to create:

``` csharp
var downloadsDir = Path.Combine(AGEnvironment.ExternalStorageDirectoryPath, AGEnvironment.DirectoryDownloads);
var fullFilePath = Path.Combine(downloadsDir, "my_voice_recording.3gp");
AGMediaRecorder.StartRecording(fullFilePath);
```

You can optionally specify the resulting file output format and preferred audio encoder.

``` csharp
AGMediaRecorder.StartRecording(fullFilePath, OutputFormat.THREE_GPP, AudioEncoder.AMR_NB);
```

### Stop recording

To finish the audio recording call `AGMediaRecorder.StopRecording()` method. 
The file you specified in the `AGMediaRecorder.StartRecording()` method will be created then.
This method returns bool value to indicate whether the call has been successful or not.
You should only call this method if you are sure that `AGMediaRecorder` is currently recording audio.
Otherwise, you will get an `java.lang.illegalStateException` exception.

``` csharp
var recordingWasStopped = AGMediaRecorder.StopRecording();
AGUIMisc.ShowToast(recordingWasStopped ? "Stopped recording" : "Failed to stop recording");
```

---

# **UI**

## AGAlertDialog.cs

Class for displaying standard Android dialogs

### FAQ

* What if I don't want a callback? Should I pass `null` ?
* No, you should just pass empty lambda `() => {}` and the callback will do nothing.

### Message dialog with positive button

Opens Android [AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html) with only positive button and optional dismiss callback.

Example usage:

``` csharp
AGAlertDialog.ShowMessageDialog("Single Button", "This dialog has only positive button", "Ok",
    () => AndroidGoodiesMisc.ShowToast("Positive button Click"),
    () => AndroidGoodiesMisc.ShowToast("Dismissed"));
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/dialog_1_btn.png" width="320" height="569">

### Message dialog with positive and negative buttons

Opens Android [AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html) with positive and negative buttons and optional dismiss callback.

Example usage:

``` csharp
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

``` csharp
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

``` csharp
string[] Colors = { "Red", "Green", "Blue" };

AGAlertDialog.ShowChooserDialog("Choose color", Colors,
    colorIndex => AndroidGoodiesMisc.ShowToast(Colors[colorIndex] + " selected"));
```

Result:

<img src="https://raw.githubusercontent.com/TarasOsiris/android-goodies-docs/master/images/dialog_chooser.png" width="320" height="569">

### Dialog with radio buttons items chooser

Opens Android [AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html) with a list of items to choose one of them with radio buttons. Also has a positive button with callback.

Example usage:

``` csharp
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

``` csharp
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

``` csharp
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

``` csharp
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

``` csharp
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

``` csharp
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

``` csharp
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

You can show native Android progress dialog as spinner or horizontal progress bar.

### Spinner

You can also create [Android progress dialogs](https://developer.android.com/reference/android/app/ProgressDialog.html) as spinners as horizontal progress bars. When you want to dismiss the progress dialog call `Dismiss()` on dialog object to dismiss it.

Example usage of spinner (showing spinner for a given period of time):

``` csharp
public void ShowSpinnerProgressDialog()
{
    StartCoroutine(ShowSpinnerForDuration());
}

private IEnumerator ShowSpinnerForDuration()
{
    // Create spinner dialog
    var spinner = AGProgressDialog.CreateSpinnerDialog("Spinner", "Call Dismiss() to cancel", AndroidDialogTheme.Dark);
    spinner.Show();
    // Spin for some time (do important work)
    yield return new WaitForSeconds(3.0f);
    // Dismiss spinner after all the background work is done
    spinner.Dismiss();
}
```

Result:

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/spinner.png "Spinner")

### Horizontal progress bar

Example usage of horizontal progress bar progress dialog (example is showing incremental progress using coroutine):

``` csharp
public void ShowHorizontalProgressDialog()
{
    StartCoroutine(ShowHorizontalForDuration());
}

private IEnumerator ShowHorizontalForDuration()
{
    const int dialogMaxProgress = 200;

    // Create progress bar dialog
    var progressBar = AGProgressDialog.CreateHorizontalDialog("Progress bar", "Call Dismiss() to cancel", dialogMaxProgress);
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

Result:

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/progress-bar.png "Horizontal progress bar")

---

## AGLocalNotifications.cs

***Note!*** This class is now deprecated. Please, use [AGNotificationManager](https://github.com/TarasOsiris/android-goodies-docs-PRO/wiki/AGNotificationManager.cs) instead.

This class is used to show local notifications. When clicked on push notification it will open your game if it is not in foreground even if it is not running. If the app is in foreground nothing will happen when clicked.

### Setup

1. **`android-support-v4.jar`** must be present in `Plugins/Android` folder.
2. **`androidgoodieslib-release.aar`** must be present in `Plugins/Android` folder.

3. Receiver must be added into you `AndroidManifest.xml` under `application tag` (see manifest inside package for reference):

``` xml
<receiver android:name="com.deadmosquitogames.notifications.GoodiesNotificationManager"/>
```

4. Setup small icon

Use the Android Asset Studio notification icons generator (https://romannurik.github.io/AndroidAssetStudio/icons-notification.html) to prepare small icons pack and replace temporary icons named **notify_icon_small.png** with the new ones in *\UnityProject\Assets\Plugins\Android\res\*

5. Setup large icon

Same as small icon, but use launcher icons generator (https://romannurik.github.io/AndroidAssetStudio/icons-launcher.html)
Just put the result into drawable directories *\UnityProject\Assets\Plugins\Android\res\* with name *notify_icon_big.png*. And in the SendNotification method set bigIcon to **"notify_icon_big"**.

If you want to use app icon just set bigIcon = **"app_icon"**.

<img src="https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/notifications.png" width="256">

### Scheduling the notification

To schedule local notification:

``` csharp
const int NotificationId = 42;
DateTime when = DateTime.Now.AddSeconds(3);
var title = "Title";
var message = "Awesome message";
var sound = true;
var vibrate = true;
var lights = true;
var bigIcon = "app_icon";
AGLocalNotifications.ShowNotification(NotificationId, when, title, message, Color.red,
    sound, vibrate, lights, bigIcon);
```

Result:

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/notification.png "Local notifications")

You can also provide a string icon name but you have to make sure the image is available in `res/drawable` folder of your Android app. By default the application launcher icon will be shown.

### Scheduling repeating notification

!> As of API 19, all repeating alarms are inexact.  If your application needs precise delivery times then it must use one-time exact alarms, rescheduling each time. Legacy applications whose SDK version is earlier than API 19 will continue to have all of their alarms, including repeating alarms, treated as exact.

To schedule the repeating notification:

``` csharp
const int NotificationId = 42;
DateTime when = DateTime.Now.AddSeconds(3);
var intervalMillis = 10 * 1000L;
var title = "Title";
var message = "Awesome message";
var sound = true;
var vibrate = true;
var lights = true;
var bigIcon = "app_icon";
AGLocalNotifications.ShowNotificationRepeating(NotificationId, when, intervalMillis, title, message, Color.red,
    sound, vibrate, lights, bigIcon);
```

### Cancelling scheduled notifications

To cancel scheduled notification you have to provide its id:

``` csharp
const int NotificationId = 42;
AGLocalNotifications.CancelNotification(NotificationId);
```

I suggest persisting the scheduled notification id in the app so you could cancel and reschedule them any time.

---

## AGNotificationManager.cs

!> Emojis may not work on older Android versions so test carefully

This class is used to manipulate local notifications

### Checking if app was launched from notification

``` csharp
var result = AGNotificationManager.IsAppOpenedViaNotification;
```

### Checking if notifications are enabled

``` csharp
var result = AGNotificationManager.AreNotificationsEnabled;
```

### Managing notification channels and notification groups

Starting in Android 8.0 (API level 26), all notifications must be assigned to a channel. For each channel, you can set the visual and auditory behavior that is applied to all notifications in that channel. Then, users can change these settings and decide which notification channels from your app should be intrusive or visible at all.

### Checking if device supports notification channels API

``` csharp
var result = AGNotificationManager.AreNotificationChannelsSupported;
```

### Creating notification channel

After you create a notification channel, you cannot change the notification behaviors — the user has complete control at that point. Though you can still change a channel's name and description.
You should create a channel for each distinct type of notification you need to send. You can also create notification channels to reflect choices made by users of your app. For example, you can set up separate notification channels for each conversation group created by a user in a messaging app.

To create a notification channel, follow these steps:

1. Construct a `NotificationChannel` object with a unique channel ID, a user-visible name, and an importance level.

``` csharp
var channel = new NotificationChannel(channelId, channelName, AGNotificationManager.Importance.Low);
```

2. Optionally, specify the other parameters of the channel.

``` csharp
//What group this channel belongs to. This is used only for visually grouping channels in the UI.
channel.Group = group;
//The user visible description of this channel.
channel.Description = "Android-Goodies test notification channel";
//The vibration pattern for notifications posted to this channel. Will be ignored if vibration is not enabled.
channel.VibrationPattern = new long[] {0, 400, 1000, 600, 1000};
//Whether or not notifications posted to this channel can bypass the Do Not Disturb mode.
channel.BypassDnd = true;
//Whether notifications posted to this channel can appear as badges in a Launcher application.
channel.ShowBadge = true;
//The notification light color for notifications posted to this channel.
channel.LightColor = Color.red;
//Whether notifications posted to this channel should display notification lights, on devices that support that feature. 
channel.EnableLights = true;
//Whether notification posted to this channel should vibrate.
channel.EnableVibration = true;
//Whether or not notifications posted to this channel are shown on the lockscreen in full or redacted form.
channel.LockscreenVisibility = Notification.Visibility.Public;
//The notification sound for this channel.
channel.SetSound(soundFilePath, audioAttributes);
```

3. Register the notification channel by passing it to `CreateNotificationChannel()` .

``` csharp
AGNotificationManager.CreateNotificationChannel(channel);
```

Creating an existing notification channel with its original values performs no operation, so it's safe to call this code when starting an app.
You can also create multiple notification channels in a single operation by calling `CreateNotificationChannels()` .

``` csharp
var channels = new List<NotificationChannel>();
...
AGNotificationManager.CreateNotificationChannels(channels);
```

### Deleting notification channel

``` csharp
AGNotificationManager.DeleteNotificationChannel(channelId);
```

### Getting notification channels

``` csharp
foreach (var channel in AGNotificationManager.NotificationChannels)
{
    //Your logic
}
```

### Opening notification channel settings

``` csharp
AGNotificationManager.OpenNotificationChannelSettings(channelId);
```

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/NotificationChannelSettings.png "Notification Channel Settings")

### Creating notification channel group

If you'd like to further organize the appearance of your channels in the settings UI, you can create channel groups. This is a good idea when your app supports multiple user accounts (such as for work profiles), so you can create a notification channel group for each account. This way, users can easily identify and control multiple notification channels that have identical names.
Organizing the notification channels into groups for each account ensures that users can easily distinguish between them.
Each notification channel group requires an ID that must be unique within your package, as well as a user-visible name.

``` csharp
var group = new NotificationChannelGroup(groupId, groupName)
{
	Description = "Android-Goodies test notification channel group"
};
AGNotificationManager.CreateNotificationChannelGroup(group);
``` 

### Deleting notification channel group

```csharp
AGNotificationManager.DeleteNotificationChannelGroup(groupId);
```

### Getting notification channel group(s)

``` csharp
//Getting one group by its ID
var channelGroup = AGNotificationManager.GetNotificationChannelGroup(group.Id);
//Getting all channel groups
foreach (var group in AGNotificationManager.NotificationChannelGroups)
{
        //Your logic
}
```

### Creating notifications

To get started, you need to set the notification's content and channel using a `Notification.Builder` object. 

``` csharp
var builder = new Notification.Builder(channelId)
	.SetContentTitle("Simple")
	.SetContentText("Notification")
	.SetSmallIcon("notify_icon_small");
```

Notice that the `NotificationCompat.Builder` constructor requires that you provide a channel ID. This is required for compatibility with Android 8.0 (API level 26) and higher, but is ignored by older versions.

!> You must create the notification channel before posting any notifications on Android 8.0 and higher.

To make the notification appear, call `AGNotificationManager.Notify()` , passing it a unique ID for the notification and the result of Notification. Builder. Build(). For example:

``` csharp
...
AGNotificationManager.Notify(notificationId, builder.Build());
```

#### Inbox style

Apply `Notification.InboxStyle` to a notification if you want to add multiple short summary lines, such as snippets from incoming emails. This allows you to add multiple pieces of content text that are each truncated to one line.
To add a new line, call `AddLine()` up to 6 times. If you add more than 6 lines, only the first 6 are visible.

``` csharp
builder.SetInboxStyle(new Notification.InboxStyle()
	.SetBigContentTitle("5 new messages from Julia:")
	.AddLine("Message 1")
	.AddLine("Message 2")
	.SetSummaryText("and +3 more"));
AGNotificationManager.Notify(notificationId, builder.Build());
```

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/InboxNotification.png "Inbox style notification")

#### Progress bar style

Notifications can include an animated progress indicator that shows users the status of an ongoing operation.

``` csharp
builder.SetContentTitle("Progress Bar Notification")
	.SetContentText("So progressive :)");

AGNotificationManager.Notify(notificationId, builder.Build());

StartCoroutine(UpdateProgressBar(builder));
```

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/ProgressBarNotification.png "Progress bar notification")

If you can estimate how much of the operation is complete at any time, use the "determinate" form of the indicator (as shown in figure) by calling `SetProgress(max, progress, false)` . 
The first parameter is what the "complete" value is (such as 100); the second is how much is currently complete, and the last indicates this is a determinate progress bar.
As your operation proceeds, continuously call `SetProgress(max, progress, false)` with an updated value for progress and re-issue the notification.

``` csharp
...
IEnumerator UpdateProgressBar(Notification.Builder builder)
{
	var currentProgress = 0f;
	const float durationTime = 5f;
	const int maxProgress = 100;

	while (currentProgress < durationTime)
	{
		var progress = Mathf.CeilToInt(currentProgress / 5f * maxProgress);
		currentProgress += Time.deltaTime;
		builder.SetProgress(maxProgress, progress, false);
		AGNotificationManager.Notify(notificationId, builder.Build());
		yield return null;
	}

	AGNotificationManager.Notify(notificationId, builder.Build());
}
```

!> The notification ID must stay the same, or updated notifications will be shown as new ones.

#### Messaging style

Starting in Android 7.0 (API level 24), Android provides a notification style template specifically for messaging content. Using the `Notification.MessagingStyle` class, you can change several of the labels displayed on the notification, including the conversation title, additional messages, and the content view for the notification.
The following code snippet demonstrates how to customize a notification's style using the `MessagingStyle` class.

``` csharp
builder.SetMessagingStyle(new Notification.MessagingStyle("John")
	.SetConversationTitle("Conversation Title")
	.AddMessage(NewMessage("Hello!", null)) // Pass in null for user.
	.AddMessage(NewMessage("Sup?", null))
	.AddMessage(NewMessage("Hi! Not bad, you?", "Ella"))
);
```

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/MessagingNotification.png "Messaging notification")

#### Big text/big picture style

To add an image in your notification, pass an instance of `NotificationCompat.BigPictureStyle` to `Notification.Builder.SetBigPictureStyle()` :

``` csharp
builder.SetBigPictureStyle(new Notification.BigPictureStyle()
		.SetSummaryText("Summary Text")
		.SetBigContentTitle("Big Picture Title")
		.SetBigLargeIcon(iconTexture)
		.SetBigPicture(bigPictureTexture));
```

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/BigPictureNotification.png "Big picture notification")

Apply `Notification.BigTextStyle` to display text in the expanded content area of the notification:

``` csharp
builder.SetBigTextStyle(new Notification.BigTextStyle()
					.BigText("Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."));
```

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/BigTextNotification.png "Big text notification")

#### Notification with action

A notification can offer up to three action buttons that allow the user to respond quickly. But these action buttons should not duplicate the action performed when the user taps the notification.
The following example adds "Open URL" button to the notification. When tapped, it opens the page in a web-browser.

``` csharp
builder.AddAction(Notification.CreateOpenUrlAction("https://github.com/TarasOsiris/android-goodies-docs-PRO/wiki", "notify_icon_small", "Open URL"));
```

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/ActionNotification.png "Action notification")

### Passing custom data with notification

You can add custom string key/value pairs to the notification and if the app later was launched/brought to foreground from the notification you can retrieve this data. For example, you can reward a player if he clicked some notification.

When creating the notification:

``` csharp
var builder = new Notification.Builder(channelId, new Dictionary<string, string>
{
	{"key1", "value1"},
	{"key2", "value2"}
});
```

To retrieve the data use `AGNotificationManager.GetNotificationParameter` , which takes the Key as parameter:

``` csharp
var result = string.Format("App was opened via notification with parameters: {0}, {1}",
	AGNotificationManager.GetNotificationParameter(CustomDataKey1),
	AGNotificationManager.GetNotificationParameter(CustomDataKey2));
```

### Scheduling notifications

You can schedule the notification by passing an optional time parameter to `AGNotificationManager.Notify()` .

``` csharp
var time = DateTime.Now.AddSeconds(20);
AGNotificationManager.Notify(notificationId, builder.Build(), time);
```

### Scheduling repeating notifications

Use `AGNotificationManager.NotifyRepeating()` to send a repeating scheduled notification:

``` csharp
var time = DateTime.Now.AddSeconds(20); //is scheduled at 20 seconds after call
const long intervalMillis = 10 * 1000L; // repeats every 10 seconds

AGNotificationManager.NotifyRepeating(null, notificationId, builder.Build(), intervalMillis, time);
```

!> as of API 19, all repeating alarms are inexact. If your application needs precise delivery times then it must use one-time exact alarms, rescheduling each time.

### Cancelling notifications

To cancel a notification use `AGNotificationManager.Cancel()` . To cancel all notifications use `CancelAll()` :

``` csharp
AGNotificationManager.Cancel(notificationId);
//or
AGNotificationManager.CancelAll();
```

---

## AGWallpaperManager.cs

This class allows you to change device wallpaper image

### Required permissons

* Set wallpaper permission must be present in `AndroidManifest.xml` file:

```xml
<uses-permission android:name="android.permission.SET_WALLPAPER"/>
```

* Write external storage permission must be present in `AndroidManifest.xml` file (to save texture to file before setting it as wallpaper):

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

or modify Android Build Settings to set Write Access to `External (SDCard)`

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/external_storage.png "Storage")

### Checking if setting wallpaper is supported or allowed

Before setting the wallpaper you must make sure it is supported and allowed:

```csharp
var isSupported = AGWallpaperManager.IsWallpaperSupported();
var isAllowed = AGWallpaperManager.IsSetWallpaperAllowed();
AGUIMisc.ShowToast(string.Format("Wallpaper supported? - {0}, set allowed? - {1}", isSupported, isAllowed));
```

### Setting wallpaper from Texture2D

To set wallpaper from existing `Texture2D` call `AGWallpaperManager.SetWallpaper(wallpaperTexture)` where `wallpaperTexture` is `Texture2D` texture or `AGWallpaperManager.ShowCropAndSetWallpaperChooser(wallpaperTexture)` if you want to allow the user to crop it beforehand.

?> If you want to set wallpaper (`Texture2D`) from your project assets you have to set **Texture Type** to **Advanced** plus set **Read/Write Enabled** checkbox to true and **Compression Format** to **RGBA 32 bit** otherwise you will get errors.

![alt text](https://github.com/TarasOsiris/android-goodies-docs-PRO/blob/master/images/share-image-settings.png "Image Settings")

### Setting wallpaper from file path

You can also set wallpaper from the file path.

This is the example that allows user to pick image from the gallery and then use it to set the wallpaper:

```csharp
AGGallery.PickImageFromGallery(
    selectedImage =>
    {
        var imageTexture2D = selectedImage.LoadTexture2D();
        
        string msg = string.Format("{0} was loaded from gallery with size {1}x{2}",
            selectedImage.OriginalPath, imageTexture2D.width, imageTexture2D.height);
        AGUIMisc.ShowToast(msg);
        Debug.Log(msg);
        AGWallpaperManager.SetWallpaper(selectedImage.OriginalPath);
        // or if you also want to allow the user to crop the image beforehand
        // AGWallpaperManager.ShowCropAndSetWallpaperChooser(selectedImage.OriginalPath);

        // Clean up
        Resources.UnloadUnusedAssets();
    },
    errorMessage => AGUIMisc.ShowToast("Cancelled picking image from gallery: " + errorMessage));
```

### Show live wallpaper chooser

You can show the screen where user can change his live wallpaper by calling `AGWallpaperManager.ShowLiveWallpaperChooser()` method.

### Reseting to default wallpaper

To reset the wallpaper to default call `AGWallpaperManager.Clear()` method.

---

## AGUIMisc.cs

Other methods relevant to UI

### Showing toast

You can show toast (short or long) like this:

```csharp
AndroidGoodiesMisc.ShowToast("hello long!", AndroidGoodiesMisc.ToastLength.Long);
```

### Showing and hiding app icon from the launcher

You can use the `AGUIMisc.ChangeApplicationIconState(false);` to hide the app icon from the system launcher and `AGUIMisc.ChangeApplicationIconState(true);` to show it again

### Showing and hiding the status bar

Use the following methods to show and hide the status bar:

```csharp
public void OnShowStatusBar()
{
    Screen.fullScreen = false;
    AGUIMisc.ShowStatusBar();
}

public void OnHideStatusBar()
{
    Screen.fullScreen = true;
    AGUIMisc.HideStatusBar();
}
```

### Immersive mode


To enable immersive mode invoke:

```csharp
AndroidGoodiesMisc.EnableImmersiveMode();
```

?> Unity 5 has immersive mode enabled by default, so if your using Unity 5 or higher, this method is redundant.


---

## AGShortcutManager.cs

This class allows you to create and manage shortcuts in your app. For guidance about using shortcuts, see [App shortcuts](https://developer.android.com/guide/topics/ui/shortcuts/).

This class exposes the API of https://developer.android.com/reference/android/content/pm/ShortcutManager

---

# **Retrieving Info**

## AGDeviceInfo.cs

---

## AGEnvironment.cs

---

## AGNetwork.cs

---

## AGTelephony.cs

---

# **Hardware**

## AGBattery.cs

---

## AGFlashLight.cs

---

## AGGPS.cs

---

## AGVibrator.cs

---

## AGCamera.cs

---

## AGFingerprintScanner.cs

---

# **Storage**

## AGFileUtils.cs

---

## AGSharedPrefs.cs

---

# **Other**

## AGPermissions.cs

---

## AGPrintHelper.cs

---

 # **FAQ**

 ## Can't Build my project after importing Android Goodies

**Q:** I can't Build my project after importing Android Goodies, how do I fix this?

When your build fails, please check the console, it will have a very long error message why the build failed. Find the part with **stderr[** and read the message.

**A #1:** Most likely you can't build because you already have Android Support Library Version 4 in your project, try removing **android-support-v4.jar** that comes with my package in Android/Plugins folder and rebuilding the project.

**A #2:** The error is *java.lang. UnsupportedClassVersionError: com/android/dx/command/Main : Unsupported major.minor version 52.0* - this means you need to update your Java to the latest JDK8 ([Download here and install](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)). **Don't forget to set the JDK inside Unity -> Preferences -> External Tools -> JDK to newly downloaded version**

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

**A:** Every class has permissions listed in xml reference docs, also check the documenation for specific class in the right column of this documentation. Also check the permissions provided in `AndroidManifest.xml` in `Plugins/Android` folder - it contains all the required permissions for all the functions.

---

## Overriding `UnityPlayerActivity` 

**Q:** Do I have to override `UnityPlayerActivity` for any of the functions to work properly?

**A:** No, Android Native Goodies does not require to override `UnityPlayerActivity` .
