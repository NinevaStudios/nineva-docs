# **iOS Goodies Documentation**

# Features

This section is a brief overview of the functionality provided by API. Click on a class name to visit full class documentation.

* App Interaction
	+ [IGApps.cs](#igappscs) - Opening various installed app, currently contains YouTube and FaceTime
	+ [IGMaps.cs](#igmapscs) - Opening Apple Maps application on specified address, location, performing search on map or displaying directions
	+ [IGShare.cs](#igsharecs) - Social sharing text+image, sending SMS or E-mail etc.
	+ [IGShare.cs](#igsharecs) - Social sharing text+image, sending SMS, E-mail, Facebook & Twitter sharing.
	+ [IGImagePicker.cs](#igimagepickercs) - Picking image from Camera/Photo Library/Photos Album and getting `Texture2D` as a result.
	+ [IGCalendar.cs](#igcalendarcs) - Creating, modifying and deleting calendar events and reminders.
	+ [IGVideoEditor.cs](#igvideoeditorcs) - Open default iOS video editor for local video
	+ [IGFilePicker.cs](#igfilepickercs) - Interact with files in other applications (Files, OneDrive, etc.)

* UI
	+ [IGDialogs.cs](#igdialogsccs) - Opening standard 1-2-or-3 button dialogs and getting a callback which button has been clicked
	+ [IGActionSheet.cs](#igactionsheetcs) - Displays UIActionSheet to the user and allows to receive callbacks for clicked buttons.
	+ [IGDateTimePicker.cs](#igdatetimepickercs) - Showing native date, time, date-time, countdown timer pickers and getting callbacks

* Hardware
	* [IGFlashlight.cs](#igflashlightcs) - Using the flashlight
	* [IGHapticFeedback.cs](#ighapticfeedbackcs) - Using the haptic feedback and vibration
	* [IGLocalAuthentication.cs](#iglocalauthenticationcs) - using Face Id/Touch Id

* Other
	* [IGDevice.cs](#igdevicecs) - The functionality of iOS [UIDevice](https://developer.apple.com/reference/uikit/uidevice) class: 
	+ Battery State and Level
	+ Proximity sensor
	+ Device UUID
	+ Device name, localized name, system name, os version
	+ More...


**[Troubleshooting & FAQ](#/Troubleshooting-&-FAQ)**

# Getting started

After importing the unitypackage add the demo scene `IOSGoodies/Example/ExampleScene.unity` to your Build Settings, switch platform to iOS and export the XCode project.

![](/images/ig/build_settings.png ':size=512')

Open the XCode project afterwards and run it on your device.

# Configure settings

You can use new editor window to disable unused features to avoid unneeded dependencies in the XCode project. Please go to `Window -> iOS Goodies -> Edit Settings`.

![](/images/ig/editorSettings.jpg ':size=512')

Here is the full list of features that can be enabled/disabled:
* Social Sharing / Email /SMS (`MessageUI.framework`)
* App rating dialog (`StoreKit.framework`)
* Image picker from Photos / Library / Camera, saving image to gallery
* Contact picker (`ContactsUI.framework`)
* Fingerprint / Face ID authentication (`LocalAuthentication.framework`)
* Calendar / Reminder access (`EventKit.framework`)

# **App Interaction**

## IGApps.cs

### Showing a dialer

Sometimes you need to prompt the user to call certain phone number.

Example:

```csharp
IGApps.OpenDialer("123456789");
```

Result:

![](/images/ig/dialer.png ':size=512')

### Opening YouTube video

You can open YouTube video on your device in YouTube app or fallback to Safari if app is not installed.

!> If the YouTube video cannot be viewed on the device, iOS displays an appropriate warning message to the user.**

Example:

```csharp
const string videoId = "rZ2csdtP440";
IGApps.OpenYoutubeVideo(videoId);
```

Result:

![](/images/ig/youtube.png ':size=512')

### Starting FaceTime Audio Call

Starts FaceTime audio call with provided email

Example:

```csharp
IGApps.StartFaceTimeAudioCall("user@example.com");
```

Result:

![](/images/ig/facetime.png ':size=512')

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

![](/images/ig/label.png ':size=512')

### Opening address (string address)


**Example:**

```csharp
const string address = "1,Infinite Loop,Cupertino,California";
IGMaps.OpenMapAddress(address, "Label-Y", IGMaps.MapViewType.Hybrid);
```

Result:

![](/images/ig/address.png ':size=512')

### Preforming simple search

**Example:**

```csharp
const string searchQuery = "Eiffel tower, Paris";
IGMaps.PerformSearch(searchQuery);
```

Result:

![](/images/ig/location.png ':size=512')

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

![](/images/ig/search.png ':size=512')

### Getting directions to a location


**Example:**

```csharp
// show route from Baker Street in London to Manchester
const string sourceAddress = "221B Baker Street, London, Great Britain";
const string destAddress = "Manchester, Great Britain";
IGMaps.GetDirections(destAddress, sourceAddress, IGMaps.TransportType.ByCar, IGMaps.MapViewType.Hybrid);
```

Result:

![](/images/ig/directions.png ':size=512')

## IGShare.cs

### Requirements

#### Sharing setting for images

!> If you want to share an image (`Texture2D`) from your project assets you have to set **Texture Type** to **Advanced** plus set **Read/Write Enabled** checkbox to true and **Compression Format** to **RGBA 32 bit** otherwise you will get errors.

![](/images/ag/share-image-settings-read-write.png ':size=512')
![](/images/ag/share-image-settings.png ':size=512')

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

![](/images/ig/share.jpg ':size=512')
![](/images/ig/twitter.jpg ':size=512')

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

![](/images/ig/email.png ':size=512')

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

![](/images/ig/message.jpg ':size=512')

### Sending SMS using modal controller

Brings up the modal window to send SMS on top of your application (not launching Messages app).

**Example:**

```csharp
IGShare.SendSmsViaController("123456789", "Hello worksadk wa dwad !!!", () => Debug.Log("Success"),
() => Debug.Log("Cancel"), () => Debug.Log("Failure"));
```

Result:

![](/images/ig/message.jpg ':size=512')


## IGImagePicker.cs

This class allows you to get a `Texture2D` into Unity by [Taking Photo with Camera](#/IGImagePicker.cs#taking-photo-with-camera) / [Choosing Photo from Photo Library](#/IGImagePicker.cs#choosing-photo-from-photo-library) / [Choosing Photo from Photos Album](#/IGImagePicker.cs#choosing-photo-from-photos-album)

**Please read the Requirements & Setup section below carefully before contacting support.**

### Requirements & Setup

In order to use Camera/Photo Gallery you need to add permission descriptions to your `Info.plist` file because of [iOS 10 Privace Settings](http://useyourloaf.com/blog/privacy-settings-in-ios-10/). 

Currently plugin contains a postprocessing script `IOSGoodies/Editor/Postprocessing/IGProjectPostprocessor.cs` that will add descriptions automatically for you. Modify the file to set descriptions to something meaningful and relevant to your project.

### Important Notes

Each of the methods accepts `compressionQuality` float variable (0 - mins minimum quality, 1 - means maximum quality). Use this to reduce memory pressure on your app, the lower the quality of the image the less memory it takes.

### Taking Photo with Camera

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
compressionQuality, allowEditing, cameraType, flashMode);
```

### Choosing Photo from Photo Library

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

### Choosing Photo from Photos Album

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

### Choosing video from photos album

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

### Saving image to gallery

You can save the specified image to iOS gallery.

!> If you want to save an image (`Texture2D`) from your project assets you have to set **Texture Type** to **Advanced** plus set **Read/Write Enabled** checkbox to true and **Compression Format** to **RGBA 32 bit** otherwise you will get errors.

```csharp
public void SaveImageToPhotoLibrary()
{
    Texture2D testImage = GetSomeImage();
    IGImagePicker.SaveImageToGallery(testImage);
}
```

## IGAppStore.cs

### Showing App Store rate popup dialog

Tells StoreKit to ask the user to rate or review your app, if appropriate.

Example:

```csharp
IGAppStore.RequestReview();
```

Result:

![](/images/ig/rating.png ':size=512')


## IGContactPicker.cs

### Picking a Contact from Address Book

Using this class user can pick a contact from his address book and receive contact as a callback. For the information that a contact contains please see [CNContact documentation](https://developer.apple.com/documentation/contacts/cncontact?language=objc)

Example:

```csharp
public void PickContact()
{
    IGContactPicker.PickContact(Debug.Log, () => { Debug.Log("Picking contact was cancelled");});
}
```

![](/images/ig/contact-picker.png ':size=512')

## IGCalendar.cs

### Open calendar

You can open the calendar app at a specified date using `IGCalendar.OpenCalendar()` method. A calendar app will be opened at the current date if you don't pass a date parameter.

```csharp
var startDate = new DateTime(2018, 12, 8, 10, 0, 0);
IGCalendar.OpenCalendar(startDate);
```

### Create calendar event

Use `IGCalendar.CreateCalendarEvent()` method to create a calendar event, specifying actions to be performed after a successful/unsuccessful creation of the event, event title, start and end dates. You can also optionally specify the event notes:

```csharp
IGCalendar.CreateCalendarEvent(id => { Debug.Log(string.Format("Calendar event was created with identifier: {0}", id)); },
error => { Debug.Log("An error occured: " + error); }, title, startDate, endDate, notes);
```

The `id` you get from successful callback of the event creation can be used later to delete the event from the calendar.

### Create repeating calendar event

Call `IGCalendar.CreateRepeatingCalendarEvent()` method to create a repeating calendar event. It takes additional parameters such as date to repeat event until, a recurrence rule type(daily, weekly, monthly, or yearly) and repeat interval. For example, if you choose `RecurrenceRuleType.Daily` and pass 5 as interval, the event will be repeated every 5 days.

### Delete a calendar event

You can use `IGCalendar.RemoveCalendarEvent()` method to cancel a calendar event. This method takes the unique event identifier as a parameter (can be obtained from callback of successful calendar event creation).

### Creating a reminder

To create a reminder, call `IGCalendar.CreateReminder()` method, passing actions to be performed after a successful/unsuccessful creation of the reminder, title, start and end dates as parameters.

```csharp
IGCalendar.CreateReminder(id => { Debug.Log(string.Format("Calendar event was created with identifier: {0}", id)); },
error => { Debug.Log("An error occured: " + error); }, title, reminderDate, reminderDate);
```

### Mark a reminder as complete/incomplete

Call `IGCalendar.CompleteReminder()` method to complete a reminder with a specified unique identifier, passing a boolean value, indicating whether to mark the reminder as completed/incomplete.

```csharp
IGCalendar.CompleteReminder(calendarItemId, true);
```

### Delete a reminder

Use the `IGCalendar.RemoveReminder()` method do remove a reminder with a specified unique identifier.

## IGVideoEditor.cs

Class to open default iOS Video crop editor provided by video path. You can get video path for example by picking video using `IGImagePicker.PickVideoFromPhotoLibrary()` method.

### Checking if video can be edited

To check if the video can be edited use `IGVideoEditor.CanEditVideoAtPath` method

### Open video editor

To open video editor use `IGVideoEditor.EditVideo(path)` providing path to the video file on the local storage as a parameter

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
	() => Debug.Log("Picking video from photos album cancelled"), allowEditing, screenPosition);
}
```

## IGFilePicker.cs

Class for manipulating documents on the file system.

### Import files

You can import files from other applications into your application's sandbox using `IGFilePicker.Import()` method.
You can specify the types of files you wish the user is able to pick, providing the array of [Uniform Type Identifiers](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_conc/understand_utis_conc.html#//apple_ref/doc/uid/TP40001319-CH202-BCGJGJGA) and whether the selection of multiple files is allowed. 
You should also specify the action to do with the list of obtained files' URLs. 
You must also provide an action to perform if the user cancels file picking.


!> These files are temporary. They remain available only until your application terminates. To keep a permanent copy, move these files to a permanent location inside your sandbox. 

```csharp
    IGFilePicker.Import(new[] { "public.data" },
    list =>
    {
        foreach (var path in list)
        {
            print(path);
        }
        _filePaths = list;
        IGDialogs.ShowOneBtnDialog("Success", "File was downloaded", "Ok", () => { });
    },
    () =>
    {
        IGDialogs.ShowOneBtnDialog("Cancelled", "File pick was cancelled", "Ok", () => { });
    }, true);
```

### Open files

You can get the security-scoped URLs for the external documents using the `IGFilePicker.Open()` method.
You can specify the types of files you wish the user is able to pick, providing the array of [Uniform Type Identifiers](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_conc/understand_utis_conc.html#//apple_ref/doc/uid/TP40001319-CH202-BCGJGJGA) and whether the selection of multiple files is allowed. 
You should also specify the action to do with the list of the files' URLs. 
You must also provide an action to perform if the user cancels file picking.

```csharp
IGFilePicker.Open(new[] {"public.image"},
list =>
{
    foreach (var path in list)
    {
        print(path);
        var bytes = File.ReadAllBytes(path);
        var tex = new Texture2D(2, 2);
        tex.LoadImage(bytes);
        image.sprite = SpriteFromTex2D(tex);
    }
},
() => { IGDialogs.ShowOneBtnDialog("Cancelled", "File pick was cancelled", "Ok", () => { }); }, true);
```
### Export/Move files

Use the `IGFilePicker.ExportToService()` method to export a copy of the file to an external service. Use the `IGFilePicker.MoveToService()` to move the original file without creating a copy. 
Specify the list of full file paths, as well as the actions to perform with the resulting file URLs and if the user cancels the exporting/moving of the file.

The methods will not be executed if none of the paths in the list is valid.

```csharp
if (_filePaths.Count == 0)
{
    IGDialogs.ShowOneBtnDialog("Paths list is empty", "Please, import files before exporting", "Ok", () => { });
    return;
}

IGFilePicker.ExportToService(list => { IGDialogs.ShowOneBtnDialog("Success", "File was exported", "Ok", () => { }); },
() => { IGDialogs.ShowOneBtnDialog("Cancelled", "File pick was cancelled", "Ok", () => { }); }, _lastPaths);
```


# **UI**

## IGDialogs.cs

This class is used to show standard native iOS dialogs.

### Confirmation Dialog (1 Button)

Confirmation dialog is a standard dialog with a single button. You can provide the title, message, button text and a callback.

**Example:**

```csharp
IGDialogs.ShowOneBtnDialog("Title", "Message", "Confirm", () => Debug.Log("Button clicked!"));
```

Result:

![](/images/ig/dialog_1.png ':size=512')

### Yes/No Dialog (2 Buttons)

Yes/No is a standard dialog with a two buttons, one for confirmation and second for cancelling. You can provide the title, message, buttons text and callbacks.


**Example:**

```csharp
IGDialogs.ShowTwoBtnDialog("Title", "My awesome message!",
"Confirm", () => Debug.Log("Confirm button clicked!"),
"Cancel", () => Debug.Log("Cancel clicked!"));
```

Result:

![](/images/ig/dialog_2.png ':size=512')

### Chooser Dialog (3 Buttons)

Yes/No is a standard dialog with three buttons, one for cancelling and two other for whatever you need. You can provide the title, message, buttons text and callbacks.


**Example:**

```csharp
IGDialogs.ShowThreeBtnDialog("Title", "My awesome message!",
"Option 1", () => Debug.Log("Option 1 button clicked!"),
"Option 2", () => Debug.Log("Option 2 button clicked!"),
"Cancel", () => Debug.Log("Cancel clicked!")
);
```

Result:

![](/images/ig/dialog_3.png ':size=512')


## IGDateTimePicker.cs
### Date and Time Pickers

This class allows you to show Date & Time Picker in these forms:

* [Date Picker](#/IGDateTimePicker.cs#date-picker)
* [Time Picker](#/IGDateTimePicker.cs#time-picker)
* [Date & Time Picker](#/IGDateTimePicker.cs#date--time-picker)
* [Countdown Timer](#/IGDateTimePicker.cs#countdown-timer)

	To each of this methods you must pass a callback which passes a `System.DateTime` object as a parameter and a cancel callback which is invoked when user cancelled picking. **If you want to nothing happen on cancel callback just pass and empty lambda as a callback: `() => {}`**

### Date Picker

To show date picker:

```csharp
public void OnShowDatePicker()
{
    var year = 1991;
    var month = 8; // August
    var day = 11;
    IGDateTimePicker.ShowDatePicker(year, month, day,
    OnDateSelected,
    () => Debug.Log("Picking date was cancelled"));
}

void OnDateSelected(DateTime date)
{
    Debug.Log(string.Format("Date selected: year: {0}, month: {1}, day {2}", 
    date.Year, date.Month, date.Day));
    var pickedDate = date.ToString("yyyy MMMMM dd");
    dateText.text = string.Format("Date Picker\n{0}", pickedDate);
}
```

Result:

![](/images/ig/date_picker.png ':size=512')

### Time Picker

To show time picker:

```csharp
public void OnShowTimePicker()
{
    var hourOfDay = 15;
    var minute = 42;
    IGDateTimePicker.ShowTimePicker(hourOfDay, minute,
    OnTimeSelected,
    () => Debug.Log("Picking time was cancelled"));
}

void OnTimeSelected(DateTime time)
{
    Debug.Log(string.Format("Time selected: hour: {0}, minute: {1}", 
    time.Hour, time.Minute));
    var pickedTime = time.ToString("hh:mm");
    timeText.text = string.Format("Time Picker\n{0}", pickedTime);
}
```

Result:

![](/images/ig/time_picker.png ':size=512')

!> You can optionally specify the minute interval of the values that can be picked in time and date-time picker methods.

### Date & Time Picker

To show date and time picker:

```csharp
public void OnShowDateAndTimePicker()
{
    var year = 1991;
    var month = 8; // August
    var day = 11;
    var hourOfDay = 15;
    var minute = 42;
    IGDateTimePicker.ShowDateAndTimePicker(year, month, day, hourOfDay, minute,
    OnDateAndTimeTimeSelected,
    () => Debug.Log("Picking date and time was cancelled"));
}

void OnDateAndTimeTimeSelected(DateTime dateTime)
{
    Debug.Log(string.Format("Date & Time selected: year: {0}, month: {1}, day {2}, hour: {3}, minute: {4}",
    dateTime.Year, dateTime.Month, dateTime.Day, dateTime.Hour, dateTime.Minute));

    var pickedDate = dateTime.ToString("G");
    dateAndTimeText.text = string.Format("Date & Time Picker\n{0}", pickedDate);
}
```

Result:

![](/images/ig/date_time_picker.png ':size=512')

### Countdown Timer

To show countdown timer:

```csharp
public void OnShowCountdownTimer()
{
    IGDateTimePicker.ShowCountDownTimer(OnCountDownTimeSelected,
    () => Debug.Log("Picking date and time was cancelled"));
}

void OnCountDownTimeSelected(DateTime countdownTime)
{
    Debug.Log(string.Format("Countdown time selected: hour: {0}, minute: {1}", 
    countdownTime.Hour, countdownTime.Minute));
    var pickedTime = string.Format("{0}:{1}", countdownTime.Hour, countdownTime.Minute);
    countdownTimeText.text = string.Format("Time Picker\n{0}", pickedTime);
}
```

Result:

![](/images/ig/countdown_timer.png ':size=512')

## IGActionSheet.cs

This class displays [UIActionSheet](https://developer.apple.com/reference/uikit/uiactionsheet) to the user and allows to receive callbacks for clicked buttons. This UI Element is perfect if you want to display a list of items and let user choose/pick only one of them.

### Simple Action Sheet

You must provide an array with the list of options for user to select from as well as Cancel button text and callback. The result of picking item from the list will be received in a callback as index of the element which user selected.

Example usage:

```csharp
static readonly string[] ActionSheetOptions = { "Option 1", "Option 2", "Option 3" };

public void OnShowActionSheet()
{
    IGActionSheet.ShowActionSheet("Title", "Cancel", () => Debug.Log("Cancel Clicked"), 
    ActionSheetOptions, index => Debug.Log(ActionSheetOptions[index] + " Clicked"));
}
```

Result:

![](/images/ig/action_sheet_simple.png ':size=512')

### Complex Action Sheet

Complex action sheet also contains "Destructive Button" which can be used to highlight some destructive action and will be displayed in red.

Example usage:

```csharp
static readonly string[] ActionSheetMoreOptions = { "Option 1", "Option 2", "Option 3", "Extra 1", "Extra 2" };

public void OnShowActionSheetWithDestructiveButton()
{
    IGActionSheet.ShowActionSheet("Title", 
    "Cancel", () => Debug.Log("Cancel Clicked"), 
    "Destroy All!", () => Debug.Log("Destroy All Clicked"), 
    ActionSheetMoreOptions, index => Debug.Log(ActionSheetMoreOptions[index] + " Clicked"));
}
```

Result:

![](/images/ig/action_sheet_complex.png ':size=512')



# **Hardware**

## IGFlashlight.cs

This class allows you to toggle the flashlight and change it's intensity

### Checking if device has a torch

To check if the device has a flashlight use `IGFlashlight.HasTorch` property

### Toggling the flashlight

To enable the flashlight use `IGFlashlight.EnableFlashlight(true)` and `IGFlashlight.EnableFlashlight(false)` to disable it

### Setting the intensity

Use `IGFlashlight.SetFlashlightIntensity(intensity)` to modify the intensity, values should be from 0 to 1.0f

## IGHapticFeedback.cs

This class allows you to send haptic feedbacks and vibrate the device.

### Requirements
Haptic feedbacks do not work on iPads, because they do not have the Taptic engine.
They can only be used on iPhone 7 and newer, with iOS version 10 and higher. All haptic feedback methods simply vibrate the device, if they are not supported.

You can check if the device supports haptic feedbacks using `IsHapticFeedbackSupported`.

```csharp
if (IGHapticFeedback.IsHapticFeedbackSupported)
{
    //Your logic here
}
```

!> All haptic methods have this check built-in.

### Haptic feedbacks

There are three types of haptic feedbacks: impact, notification and selection. You can call them using the respective methods:

```csharp
IGHapticFeedback.NotificationOccurred(IGHapticFeedback.NotificationType.Error);
IGHapticFeedback.SelectionChanged();
IGHapticFeedback.ImpactOccurred(IGHapticFeedback.ImpactType.Light);
```
You can use one of three notification types for `IGHapticFeedback.NotificationOccurred` method and modify the strength of impact for `IGHapticFeedback.ImpactOccurred`.

### Vibrate
To make the device vibrate with default pattern, use `IGHapticFeedback.Vibrate` method:

```csharp
IGHapticFeedback.Vibrate();
```

## IGLocalAuthentication.cs

This class allows you to use Face Id/Touch Id features

To check if the authentication is available use `IGLocalAuthentication.IsLocalAuthenticationAvailable`

`IGLocalAuthentication.AuthenticateWithBiometrics()` method is used to authenticate user with Face Id/Touch id. You will receive a success callback if authentication was successful or failure callback with the failure reason if authentication failed for some reason.

Example usage:

```csharp
if (IGLocalAuthentication.IsLocalAuthenticationAvailable)
{
    const IGLocalAuthentication.Policy policy = IGLocalAuthentication.Policy.DeviceOwnerAuthenticationWithBiometrics;
    IGLocalAuthentication.AuthenticateWithBiometrics("Please, confirm your identity", policy,
        () => { Debug.Log("Authentication was successful"); }, 
        error => Debug.Log("Authentication failed: " + error));
}
else
{
    Debug.Log("Device does not support biometric authentication.");
}
```

# **Other**

## IGDevice.cs

This class provides to all the functionality of [UIDevice](https://developer.apple.com/reference/uikit/uidevice) class on iOS devices. Please refer to its documentation how the class works on iOS device.

### Device Info

+ Check if multitasking is supported: `IGDevice.IsMultitaskingSupported`
+ Get the name identifying the device: `IGDevice.Name`
+ Get the name of the operating system running on the device represented by the receiver: `IGDevice.SystemName`
+ Get the current version of the operating system: `IGDevice.SystemVersion`
+ Get the model of the device as a localized string: `IGDevice.LocalizedModel`
+ Get the style of interface to use on the current device: `IGDevice.UserInterfaceIdiom`
+ Get an alphanumeric string that uniquely identifies a device to the app’s vendor (UUID): `IGDevice.UUID`

### Battery

+ Get the battery charge level for the device: `IGDevice.BatteryLevel`. **To get the battery level first you have to set `IGDevice.IsBatteryMonitoringEnabled = true`**
+ Get or set boolean value indicating whether battery monitoring is enabled: `IGDevice.IsBatteryMonitoringEnabled`
+ Get device battery state: `IGDevice.DeviceBatteryState`. **To get the battery state first you have to set `IGDevice.IsBatteryMonitoringEnabled = true`**

### Proximity Sensor

+ Gets or sets a value indicating whether proximity monitoring is enabled: `IGDevice.IsProximityMonitoringEnabled`
+ Get proximity state of the device: `IGDevice.ProximityState`. **To get the proximity state first you have to set `IGDevice.IsProximityMonitoringEnabled = true`**

