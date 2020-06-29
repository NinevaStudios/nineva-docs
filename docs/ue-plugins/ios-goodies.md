**iOS Goodies Unreal Engine plugin documentation**

Welcome to iOS Goodies Documentation for Unreal Engine

**Join our [Discord server](https://bit.ly/nineva_support_discord) and ask us anything!**

# Changelog

v.1.3.1

+ ADDED Unreal Engine 4.25 support
+ UPDATED Image pickers

v.1.3.0

+ ADDED Added possibility to change size of image picked from gallery
+ ADDED Status bar show/hide functionality
+ FIXED Issue with image picker rotation in lanscape mode
+ FIXED Native UI elements appearence when dark theme is enabled
+ FIXED Some threading issues on iOS 13

v.1.2.3

+ ADDED Unreal Engine 4.24 support
+ FIXED Added zero-initializer for enumerations to avoid build issues

v.1.2.2

+ ADDED Fetching contacts info
+ UPDATED Improved mechanism for handling iOS native callbacks

v.1.2.1

+ ADDED Unreal Engine 4.22 support
+ UPDATED Removed some limitations for sharing via default apps

v.1.2.0

+ ADDED Screenshot taking
+ ADDED Picking and deleting images from gallery
+ ADDED Capturing photos with device camera
+ ADDED Calendar events and reminders

v.1.1.0

+ ADDED Haptic feedback

v.1.0.0

+ Initial release

# Features

* Native UI
	* [Native dialogs](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Native-dialogs)
	* [App Store rate dialog](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/App-Store-rate-dialog)
	* [Date and time picker](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Date-and-time-picker)
	* [Native sharing](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Native-Sharing)

* Hardware
	* [Flashlight](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Flashlight)
	* [Haptic Feedback](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Haptic-Feedbacks)

* App interaction
	* [App interaction](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Apps)
	* [Apple Maps interaction](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Apple-Maps-Interaction)
	* [Camera and Gallery](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Camera-and-Gallery)
	* [Events](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Events)
	* [Contacts](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Contacts)

*  Getting info
	* [Get device info and check supported features](https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/wiki/Device-info)

# FAQ

## .plist

iOS Goodies plugin in order to make all of its features work properly adds extra entries to .plist file. Note that those entries overwrite ones that were added in project settings. In order to remove/edit those entries, you have to edit IOSGoodies_UPL.xml (included in the plugin sources) accordingly.

## Input dialog and date-time picker theming on iOS 13

On iOS 13 native date picker and input field have some appearance issues when using dark theme. To fix this problem please follow these instructions:
- Go to /[UE4 Root]/Engine/Plugins/ and copy iOSGoodies plugin to /[Project Root]/Plugins/
- In /[Project Root]/Plugins/iOSGoodies/Source/iOSGoodies/Private/IGDialogBPL.cpp uncomment lines 353-364 and comment line 369
- In /[Project Root]/Plugins/iOSGoodies/Source/iOSGoodies/Private/IOS/IGDateTimePicker.cpp uncomment lines 68-79 and comment line 84
- Remove Binaries and Intermediate folders in /[Project Root]/Plugins/iOSGoodies/
- Remove Binarie, Intermediate, Build and Saved folders in /[Project Root]/
- Run the engine and rebuild your project
	As a result those native dialogs should have appropriate theming when running on device with iOS 13 installed.

# [Native dialogs](#TODO)

You can show standard iOS native dialogs with:

* [Single button](#dialog-with-single-button)
* [Two buttons (i.e. Yes/No, Ok/Cancel)](#dialog-with-two-buttons)
* [Three buttons (chooser)](#dialog-with-three-buttons)
* [Input dialog](#input-dialog)
* [Action sheet](#action-sheet)
* [Action sheet with destructive button](#action-sheet-with-destructive-button)
* [Loading dialog](#loading-dialog)

	Native dialogs support callbacks for different events like a button click or dialog cancellation. The user must provide appropriate event handlers for those callbacks.

## Dialog with single button

Show this dialog by calling `ShowSingleButtonDialog` function.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_SingleButtonDialog.png" width="600">

Result:

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/SingleButtonDialog.png" width="300">

## Dialog with two buttons

Show this dialog by calling `ShowTwoButtonsDialog` function.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_TwoButtonsDialog.png" width="600">

Result:

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/TwoButtonsDialog.png" width="300">

## Dialog with three buttons

Show this dialog by calling `ShowThreeButtonsDialog` function.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_ThreeButtonsDialog.png" width="600">

Result:

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/ThreeButtonsDialog.png" width="300">

## Input dialog

Call `ShowInputFieldDialog` to show a native dialog with an input field. You can customize it by passing title, body text, confirmation and cancellation button titles. After the user confirms input, you will receive a callback with the typed string.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_InputDialog.png" width="600">

## Action sheet

Show action sheet by calling `ShowActionSheetSimpleDialog` function. It takes an array of strings as a parameter which represents names of action sheet buttons. When some action button is clicked user will receive a callback with the index of clicked button.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_ActionSheet.png" width="600">

Result:

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/SimpleActionSheet.png" width="300">

***Note!*** If you plan your application to be run on iPad, pass additional parameters (posX and posY), which represent the position of the popover view on iPads. Default values (0; 0) will cause the view to appear in the top left corner of the screen. These values will be ignored on iPhone.

## Action sheet with destructive button

Show action sheet with destructive button by calling `ShowActionSheetComplexDialog` function. It takes an array of strings as a parameter which represents names of action sheet buttons. When some action button is clicked user will receive a callback with the index of clicked button.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_ActionSheetComplex.png" width="600">

Result:

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/ComplexActionSheet.png" width="300">

***Note!*** If you plan your application to be run on iPad, pass additional parameters (posX and posY), which represent the position of the popover view on iPads. Default values (0; 0) will cause the view to appear in the top left corner of the screen. These values will be ignored on iPhone.

## Loading dialog

This function fades screen and displays native loading spinner at the middle. Call `ShowLoadingDialog` to begin showing a loading screen. Call `DismissLoadingDialog` to dismiss it after the background work is done. 

!> All user interface iteractions are blocked during the lifetime of the loading screen

# [App Store rate dialog](#TODO)

Use `RequestRating` function to produce a dialogue asking the user to rate your app on the AppStore.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_AppRating.png" width="300">

# [Date and time pickers](#TODO)
You can show the default iOS date and time picker.

## Showing date picker

To show the default iOS date picker call `ShowDatePicker` function.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_Date.png" width="600">

There is an option to show date picker but with a limited range of dates that can be selected (i.e. only dates between 15-Oct-1991 and 16-Nov-2018). Use `ShowDatePickerWithLimits` function to do that.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_DateLimited.png" width="600">

Result:

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/DatePicker.png" width="400">

## Showing time picker

To show the default iOS time picker call `ShowTimePicker` function. 

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_Time.png" width="600">

There is an option to show time picker but with a limited range of time values that can be selected (i.e. only time within 14:45 and 19:57 interval). Use `ShowTimePickerWithLimits` function to do that.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_TimeLimited.png" width="600">

Result:

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/TimePicker.png" width="400">

## Showing date and time picker

It possible to show picker that allows selecting both date and time. Use `ShowDateTimePicker` function to do that. 

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_DateTime.png" width="600">

Like with pickers described above you can limit the range of date-time values that can be selected with `ShowDateTimePickerWithLimits` function instead.

Result:

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/DateTimePicker.png" width="400">

## Showing timer

To show picker with countdown timer style use `ShowCoundownTimer` function. It allows picking values between 1 minute and 23 hours 59 minutes.

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/BP_timer.png" width="600">

Result:

<img src="https://github.com/NinevaStudios/iOSGoodiesUnreal-SampleProject/blob/master/Resources/TimerPicker.png" width="400">
# [Device Info](#TODO)
# [Native Sharing](#TODO)
# [App Interaction](#TODO)
# [Apple Maps Interaction](#TODO)
# [Camera and Gallery](#TODO)
# [Flashlight](#TODO)
# [Haptic Feedback](#TODO)
# [Events](#TODO)
# [Contacts](#TODO)
