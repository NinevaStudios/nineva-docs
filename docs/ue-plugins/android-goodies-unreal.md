# **Native UI**

## Native dialogs

You can show standard Android AlertDialog with:
* [Only positive button](#message-dialog-with-positive-button)
* [Positive and negative buttons](#message-dialog-with-positive-and-negative-buttons)
* [Positive, negative and neutral buttons](#message-dialog-with-positive-negative-and-neutral-buttons)
* [Dialog with simple items chooser](#dialog-with-simple-items-chooser)
* [Dialog with radio buttons items chooser](#dialog-with-radio-buttons-items-chooser)
* [Dialog with checkboxes buttons items chooser](#dialog-with-check-boxes-buttons-items-chooser)
* [Progress dialog (spinner)](#progress-dialog-spinner)
* [Progress dialog (progress bar)](#progress-dialog-horizontal-progress-bar)

It is also possible to set native dialog theme (`Light`, `Dark` or `Default`, which will apply global device's dialog theme).

Native dialogs support callbacks for different events like button click or dialog cancellation. User must provide appropriate event handlers for those callbacks.

#### *Message dialog with positive button*{docsify-ignore}

Show this dialog by calling `ShowSingleButtonDialog` function.

![](images/android-goodies/native-ui/Scr_positiveDlg.png)

#### *Message dialog with positive and negative buttons*{docsify-ignore}

Show this dialog by calling `ShowTwoButtonsDialog` function.

![](images/android-goodies/native-ui/Scr_twoBtnDlg.png)

#### *Message dialog with positive, negative and neutral buttons*{docsify-ignore}

Show this dialog by calling `ShowThreeButtonsDialog` function.

![](images/android-goodies/native-ui/Scr_threeBtnDlg.png)

#### *Dialog with simple items chooser*{docsify-ignore}

Show this dialog by calling `ShowChooserDialog` function.

![](images/android-goodies/native-ui/Scr_chooserDlg.png)

#### *Dialog with radio buttons items chooser*{docsify-ignore}

Show this dialog by calling `ShowSingleItemChoiceDialog` function.

![](images/android-goodies/native-ui/Scr_singleOptDlg.png)

#### *Dialog with checkboxes buttons items chooser*{docsify-ignore}

Show this dialog by calling `ShowMultipleItemChoiceDialog` function.

![](images/android-goodies/native-ui/Scr_multiOptDlg.png)

#### *Progress dialog (spinner)*{docsify-ignore}

To show this dialog it first should be created with `CreateProgressDialog` function (pass `AGProgressDialogData` structure with specified `Spinner` style as a parameter). Then just call `Show` method of the received object interface instance. Call `Dismiss` method to close this dialog.

![](images/android-goodies/native-ui/Scr_spinnerDlg.png)

#### *Progress dialog (horizontal progress bar)*{docsify-ignore}

To show this dialog it first should be created with `CreateProgressDialog` function (pass `AGProgressDialogData` structure with specified `Progress Bar` style as a parameter). Then just call `Show` method of the received object interface instance. Call `Dismiss` method to close this dialog.

![](images/android-goodies/native-ui/Scr_progressDlg.png)

To update progress value use dialog's `SetProgress` method.

___

## Date and time picker

You can show standard Android Date and Time pickers.

#### *Showing Date Picker*{docsify-ignore}

To show the default Android date picker call `ShowDatePicker` function.

![](images/android-goodies/date-picker/Scr_DatePicker.png)

Result:

![](images/android-goodies/date-picker/Scr_DateRes.png)

There is a possibility to limit a range of dates that can be picked by a user. To do that call `ShowDatePickerWithLimits` function instead. The only difference here is that it takes two additional parameters - start and end dates of picking range.

#### *Showing Time Picker*{docsify-ignore}

To show the default Android time picker call `ShowTimePicker` function.

![](images/android-goodies/date-picker/Scr_TimePicker.png)

Result:

![](images/android-goodies/date-picker/Scr_TimeRes.png)

___

## Show Toasts

You can show standard Android toast messages.

To show toast call `ShowToast` function and pass message text and toast duration length as parameters.

![](images/android-goodies/toast/Scr_Toast.png)

Duration can be either `Short` or `Long` (corresponds to Android [default values](https://developer.android.com/reference/android/widget/Toast.html#constants)).






