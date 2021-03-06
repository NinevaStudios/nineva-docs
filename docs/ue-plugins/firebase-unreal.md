# **Firebase Goodies**

Welcome to Firebase Goodies Documentation for Unreal Engine

?> **Join our [Discord server](https://bit.ly/nineva_support_discord) and ask us anything!**

---
Create a Firebase project, follow the instructions in this [video](https://www.youtube.com/watch?v=6juww5Lmvgo).

In the Project Settings of your Firebase console, create an Android/iOS app providing the required information.
You will have to provide the SHA1 fingerprints for the keystores that will be used to sign the application on Android. The information on how to generate a keystore and read its fingerprint can be found [here](https://stackoverflow.com/questions/15727912/sha-1-fingerprint-of-keystore-certificate).
After this, you should download the *google-services.json* (for Android) and/or *GoogleService-info.plist* files.

![](images/firebase/auth/AuthConfigGoogleServices.png)

After the setup is complete in the Firebase console, you can open your UE4 project and go to Project Settings -> Firebase Goodies. You should provide the path to previously downloaded *google-services.json* (for Android) and/or *GoogleService-info.plist* files for them to be parsed by the plugin. This path needs to be absolute for the plugin to properly use this file so it is advised not to commit this setting to your VCS.

![](images/firebase/Settings.png)

!> If you do not provide a valid path to the *google-services.json* and/or *GoogleService-info.plist* file your application will crash on start. A descriptive error message in the device logs will indicate that it was not able to find these files.

?> Most of the API tries to be as close as possible to the official Firebase API, because of this we advise to look at the [official documentation](https://firebase.google.com/docs) from Google as this might help you understand some concepts better with the examples they provide.

# **Analytics**

## Initial Setup

Analytics collects usage and behavior data for your app. There are two types of information you can log:
* Events - any event that is happening in your app with or without parameters
* User properties - attributes you assign to a certain group of your users (e. g. language preference)

After enabling analytics in your Firebase console you're all set to start logging events.

?> Events usually take up to an hour to appear on the Firebase console dashboard. For testing, you can tell your device to send debug analytics events. Please follow the official instruction [here](https://support.google.com/firebase/answer/7201382) to activate this feature.

## Functions

### State control

* Enable/Disable analytics collection
* Reset analytics data
* Specify session timeout duration (default is 30 minutes)

![](images/firebase/analytics/AnalyticsStateControl.png)

### User properties

* Set user ID

  Sets the user ID property

* Set user property

  Sets a user property to a given value

![](images/firebase/analytics/AnalyticsUserProp.png)

### Screen tracking

* Set current screen

![](images/firebase/analytics/AnalyticsScreenTracking.png)

### Event loging

* Log event with parameter

  You can log an event with one of the following parameter types: Integer, Float, String. Use a respective CreateParameter node to get the desired type.

![](images/firebase/analytics/AnalyticsCreateEventParams.png)

* Parameter value

  You can change the parameter's value.

![](images/firebase/analytics/AnalyticsSetParamValue.png)

* Log event with parameters

  You can log an event with multiple parameters by putting them in an array.

![](images/firebase/analytics/AnalyticsParamArray.png)

# **Authentication**

## Initial Setup

You have to setup the user authentication feature in the firebase console for your firebase project to be able to use the auth functionality of the plugin. Go to the Authentication section on your Firebase console, and there you can enable all the required sign in providers for your application.

![](images/firebase/auth/AuthSetup.png)

!> Some of the providers may require additional setup. Please, check the official Firebase documentation on how to enable the different sign in methods if you are having any problems with it.

## Functions

Auth library of the plugin allows manipulating users and sessions.

First of all, you should activate the listeners that will be triggered whenever user or token authentication state changes, by calling the `InitListeners` method. This will allow you to react to these changes accordingly during the lifetime of the application.

![](images/firebase/auth/AuthInit.png)

### User registration and login
You can create a new user with an email and password by calling the `CreateUser` method.

![](images/firebase/auth/AuthCreateUser.png)

There are several options for sign in. You can either do it with email and password, a custom token, or with a specific credential. There are also methods for obtaining these credentials, but they require an additional integration of the corresponding provider's SDK. For example, you can use Facebook credentials to sign the user in, but you will need a valid Facebook token that can only be obtained if you integrate the Facebook SDK into your project.

We have integrated the Google Sign In SDK as a part of the plugin in order to show an example of how this should be done. Call the `PromptGoogleSignIn` method to show a native sign in with Google dialog. After the user successfully signs in, you will be able to use the AuthCredentials object to either sign the user in Firebase, or link them to an existing account.

![](images/firebase/auth/AuthGoogleSignIn.png)

You can also use the anonymous sign in by calling the `SignInAnonymously` method.

!> All sign in methods return the FirebaseUser object when successful, that can be used later to read and modify the user's data.

You can also obtain a reference to the current user object at any time by calling the `CreateUser` method.

You can also link a phone number to the existing user. You have to call the `VerifyPhoneNumber` function to start the phone number verification process. On most devices the user experience is the following: when you call this method after obtaining the user's phone number, they will either receive an SMS or a push notification with the code, while the application will receive OnSmsSent callback with the Verification ID needed for creating the AuthCredentials object along with the verification code, obtained by the user. Sometimes on Android devices, the operation will be performed silently by the operating system, allowing you to use the AuthCredentials object from the OnSuccess callback. Your application should always be able to handle both cases.

![](images/firebase/auth/AuthVerifyPhoneNum.png)

!> You have to add the test phone numbers in the Firebase console in order to test the phone verification functionality during development.

Call `SignOut` function to sign the current user out.

### User management

Whenever you get access to the FirebaseUser object, you should check if it is valid by calling the `IsNotNull` function on it. If the function returns false, you should not perform any operations with it. By calling this function on the `GetCurrentUser` object, you can check whether there is a user signed in to your application and prompt the user to sign in otherwise.

You can get some of the user data, for example, unique user ID, display name, email, phone number, avatar URL, etc.
You can also update this information by calling the respective methods, for example, `UpdateEmail`.

!> You can only update the user's phone number after successful phone verification with the obtained AuthCredentials object.

You can also link additional credentials to the user, for example, Google and Facebook credentials. You can also get a list of all providers for user (`FetchProvidersForEmail`) and unlink them (`UnlinkProvider`).

If there is a need, you can reauthenticate or reload the current user by calling the respective methods.

Call `Delete` on the FirebaseUser object to delete the user from your user database.

# **Cloud Storage**

## Initial Setup

After the project is created you can go to the Cloud Storage Section in console, and setup the storage security rules. Refer to this [page](https://firebase.google.com/docs/storage/security/start) for more info.

![](images/firebase/cloud-storage/CloudStorageSetup.png)

## Functions

Cloud Storage library of the plugin allows manipulating files.

!> In order to work with cloud storage user needs to be signed in to firebase. Please refer to [Auth page](#Authentication) for setup of authentication.

### Upload files

You can upload files from device to firebase cloud storage by calling `UploadFromLocalFile` method.

![](images/firebase/cloud-storage/CloudStorageUploadFromLocal.png)

Or upload from memory buffer by calling `UploadFromDataInMemory` method.

![](images/firebase/cloud-storage/CloudStorageUploadFromMemory.png)

Both methods return progress of uploading in % during the whole process.

### Download files

For downloading files from cloud storage to the device use `DownloadToLocalFile` method.

![](images/firebase/cloud-storage/CloudStorageDownloadToLocal.png)

For Android, you can choose the directory to download a file to by selecting the corresponding environment from the dropdown list.

![](images/firebase/cloud-storage/CloudStorageDownloadToLocalEnv.png)

For downloading a file into a memory buffer use `DownloadInMemory` method.

![](images/firebase/cloud-storage/CloudStorageDownloadToMemory.png)

You can limit the size of a memory buffer by providing custom value to FileSizeLimit variable in Bytes.

In case you need to get Url for downloading the file call `GetDownloadUrl` method.

![](images/firebase/cloud-storage/CloudStorageGetDownloadUrl.png)

### File metadata

After uploading a file to cloud storage, you can obtain the file's metadata by calling `GetFileMetadata` method.

![](images/firebase/cloud-storage/CloudStorageGetFileMetadata.png)

Successful callback returns metadata object reference, which you can use to retrieve metadata properties, for a full list of properties, refer to [this section](https://firebase.google.com/docs/storage/android/file-metadata#file_metadata_properties).

You can update file metadata at any time after the file is uploaded by using `UpdateFileMetadata` method.

![](images/firebase/cloud-storage/CloudStorageUpdateFileMetadata.png)

First, you need to create a `NewStorageMetadataValues` object, and after that set all the metadata properties needed. For the list of all the properties that can be set, you can refer to [this section](https://firebase.google.com/docs/reference/android/com/google/firebase/storage/StorageMetadata.Builder#public-method-summary).

!> Method `SetContentLanguage` accepts language abbreviations consisting of two letters, we weren't able to find the exact list, but most of [ISO 639-1 Language Codes](http://www.mathguide.de/info/tools/languagecode.html) work.

### Delete files

To remove files from cloud storage use `DeleteFile` method.

![](images/firebase/cloud-storage/CloudStorageDeleteFile.png)

# **Realtime Database**

## Initial Setup

Firebase supports two types of databases: realtime database and firestore. This plugin, currently, only supports the realtime database. You can find the differences between these databases and which one will suit you better on the [official site](https://firebase.google.com/docs/database/rtdb-vs-firestore).

If you are set on using the realtime database you will need to first create it in your Firebase console:

![](images/firebase/db/DbCreate.png)

?> This plugin can connect only to the default realtime database. Multiple databases are not supported.

Setup rules to be able to read/write data from your database by following the official [instructions](https://firebase.google.com/docs/database/security).

!> If you decide to setup rules that anyone can read/write data do not forget to modify them when releasing your app.

## Settings

* Enable data persistence

  If enabled clients will cache synchronized data and keep track of all writes you've initiated while your application is running.

* Cache size

  Size of the cache used for persistence storage (default is 10 MB).

## Functions

!> The plugin demo contains examples on how to use most of these nodes. The demo will write to your database and some examples require specific data to be already present in the database. Please examine the blueprints before executing any demo functionality.

### Database Reference

The database reference is the entry point from where you modify data.

* Create database root ref

  Get a reference to the root of your database.

* Create database ref from path

  Get a reference to a specific node in your database. Path can be a name or an actual path separated by **/**.

* Root

  Get a reference to the root of your database.

* Child

  Get a reference to a specific child node in your database.

* Parent

  Get a reference to the parent node.

!> You must maintain a reference to your DB if you are subscribing to update events for it to not automatically disconnect after the certain time. Please see the demo for reference implementation.

![](images/firebase/db/DbMakeRef.png)

### Read/Write Data

All data is written and read as database variants. This is a special type that can be transformed into a primitive type (integer, float, bool, string) or a container of other variants. You can convert supported data types to a variant by simply dragging the value out pin to the variant in pin. You will be notified that a conversions is possible, you will see an error otherwise.

#### Write

* Set value

![](images/firebase/db/DbSetVal.png)

* Set array of values

![](images/firebase/db/DbSetArray.png)

* Set map of values

![](images/firebase/db/DbSetMap.png)

* Composite containers

  A variant can be a container that can also contain other containers (e. g. a map where one of its values in an array). Try to avoid deep nesting of containers as the more complicated data you are trying to write the more room for error you introduce.

![](images/firebase/db/DbCompositeContainer.png)

!> UE4 does not support automatic conversion of collections to collection items. When trying to do this you will see the following error:

![](images/firebase/db/DbCompositeContainerError.png)

?> At any point where automatic conversion to a variant does not work you can invoke the variant conversion node manually. Simply search the node by name *variant* and you should see it.

![](images/firebase/db/DbManualVariantConv.png)

* Set priority

  Sets the priority for the current database node.

![](images/firebase/db/DbSetPriority.png)

* Update children

  Update the specific child keys to the specified values.

![](images/firebase/db/DbUpdateChildren.png)

* Push

  Create a new child at the current location. The name is auto-generated.

![](images/firebase/db/DbPush.png)

#### Read

* Get value

  You can request the value once.

![](images/firebase/db/DbGetValue.png)

If the value was retrieved properly you will receive it in a callback in the form of a Data Snapshot object. You can get the following data from this object:

* Get value
* Get key
* Get priority

![](images/firebase/db/DbSnapshotGetters.png)

Value and priority are variant objects. You can find out what type of value the variant holds by breaking it and checking the Type field.

![](images/firebase/db/DbVariantTypeEnum.png)

There are two ways to get the actual values.

* Direct access by type
  These nodes return the value if the provided type is the same as the variant or a default value for the type requested. This never fails.
* Try get type nodes
  These nodes try to get the value of a specified type and will fail if the variant does not hold this type.

![](images/firebase/db/DbVariantRead.png)

Data snapshots support navigation and these checks:

* Exists

  Returns true if the snapshot contains data.

* Has children

  Returns true if the snapshot contains any children,

* Has child

  Return true if the snapshot contains a specific child.

* Child

  Get a snapshot of a specific child node.

* Get children count

  Get the count of children of this node.

* Get children

  Get all child snapshots as an array.

![](images/firebase/db/DbSnapshotNav.png)

If an error occurs during value retrieval you can react to it in the error callback.

![](images/firebase/db/DbErrorCallback.png)

#### Subscribe

* Value listener

  Subscribe/unsubscribe to receive events when a value changes at the current path.

![](images/firebase/db/DbSubValueChange.png)

* Child listener

  Subscribe/unsubscribe to receive events when a child node changes at the current path.

![](images/firebase/db/DbSubChildChange.png)

* Child event

  When subscribed to child events you may receive the following event types:
  - Added
  - Changed
  - Removed
  - Moved

![](images/firebase/db/DbChildChangeEventType.png)

!> If you subscribe to the same database location multiple times you will receive multiple callbacks when values change at that locations. If this happens unintentionally double check if you are not subscribing from a database object that was created by a Make function directly as pure blueprint nodes can execute multiple times. In these cases always store the database reference as available.

### Queries

!> Some sort and filtering options cannot be combined. These combinations are checked during runtime so you will spot errors when your app launched or executed a query. On Android, you should see an error message in the device console. On IOS your app will crash if an invalid query is found, refer to the XCode debug console to find the reason.

#### Sort data

* Order By Child
* Order By Value
* Order By Key
* Order By Priority

![](images/firebase/db/DbOrderBy.png)

#### Filter data

* StartAt

  Return items greater than or equal to the specified key or value.

![](images/firebase/db/DbStartAt.png)

* EndAt

  Return items less than or equal to the specified key or value.

![](images/firebase/db/DbEndAt.png)

* EqualTo

  Return items equal to the specified key or value.

![](images/firebase/db/DbEqualTo.png)

* Limit

  Sets the maximum number of items to return from the beginning/end of the ordered list of results.

![](images/firebase/db/DbLimitQuery.png)

### Transactions

When working with data that could be corrupted by concurrent modifications, such as incremental counters, you can use a transaction.

* Run transaction

  This will execute a transaction handler.

![](images/firebase/db/DbRunTransaction.png)

#### Transaction Handler

To create a transaction handler follow the steps:

1. Create a blueprint and inherit from FGTransactionHandler

![](images/firebase/db/DbTransactionInherit.png)

2. Override the DoTransaction method

![](images/firebase/db/DbTransactionOverride.png)

3. Implement the handler

![](images/firebase/db/DbTransactionHandlerExample.png)

The handler operates on a MutableData object. After all of the operations are done you need to return Success for the transaction operation to take effect.

Mutable data supports the following operations:

* Get value
* Get key
* Get priority

![](images/firebase/db/DbMutableDataGet.png)

* Set value
* Set priority

![](images/firebase/db/DbMutableDataSet.png)

* Has children
* Has child
* Child
* Get children count
* Get children

![](images/firebase/db/DbMutableDataNav.png)

### Connection state

Manually disconnect/connect the Firebase Database client from/to the server.

![](images/firebase/db/DbConnState.png)

### Keep synced

By executing this node on a location, the data for that location will be automatically downloaded and kept in sync, even when no listeners are attached for that location.

![](images/firebase/db/DbKeepSynced.png)

# **Remote Config**

## Initial Setup

To use Firebase Remote Config in your application you need to login to firebase console and add parameters you need, in Remote Config tab. This plugin can be used in many different ways, for more details refer to [Remote Config use cases](https://firebase.google.com/docs/remote-config/use-cases).

![](images/firebase/remote-config/FirebaseRCSetup.png)

## Functions

Remote Config of the plugin allows you to define parameters in your app and update their values in the cloud, letting you modify the appearance and behavior of your app without distributing an app update. The official documentation can be found here for [Android](https://firebase.google.com/docs/remote-config/use-config-android) and [iOS](https://firebase.google.com/docs/remote-config/use-config-ios)

First of all you should set the minimum fetch interval and fetch timeout of config settings by calling `SetConfigSettings`.

![](images/firebase/remote-config/FirebaseRCSetConfigSettings.png)

### Fetch remote config values

To fetch parameter values from the Remote Config backend, call `Fetch` method. Any values that you set in the backend are fetched, adhering to the default minimum fetch interval

![](images/firebase/remote-config/FirebaseRCFetch.png)

In case you need to set a custom interval for fetching, call `FetchWithInteval` method. It will start fetching configs, adhering to the specified minimum fetch interval.

![](images/firebase/remote-config/FirebaseRCFetchWithInterval.png)

Ta make fetched parameter values available to your app, call `Activate` method.

![](images/firebase/remote-config/FirebaseRCActivate.png)

If you want to fetch and activate values in one call, use `FetchAndActivate` method.

![](images/firebase/remote-config/FirebaseRCFetchAndActivate.png)

### Read config values

If you set values in the backend, fetch them, and then activate them, those values are available to your app. Now you can get parameter values from the Remote Config. To get the values, call the method listed below that maps to the data type expected by your app, providing the parameter key as an argument:
 - `GetString`;
 - `GetLong`;
 - `GetFload`;
 - `GetBoolean`;

### Default values

You can set in-app default parameter values in Remote Config, so that your app behaves as intended before it connects to the Remote Config backend, and so that default values are available if none are set in the backend.

Before setting the default values, you first need to make a map of parameter names and default parameter values, then call the `SetDefaults` methods with this map.

![](images/firebase/remote-config/FirebaseRCSetup.png)

# **Cloud Messaging**

Official documentation regarding the Firebase Cloud Messaging can be found [here](https://firebase.google.com/docs/cloud-messaging).

?> The iOS setup is quite complex and it's easy to miss a step. Please, follow the instructions very carefully. You also have to go to Project Settings -> Feirebase Goodies and set the bEnableAPNForIOS to true.

![](images/firebase/cloud-messaging/CloudMessagingSettings.png)

!> If you are using UE 4.23 or 4.24, you will have to manually add the `Push Notifications` capability for the resulting XCode project of your iOS build.

## Manage device ID

Call `GetInstanceIdData` to receive Firebase Cloud Messaging (FCM) token and Instance ID.
Those values are used to target specific devices.

![](images/firebase/cloud-messaging/CloudMessagingGetInstanceId.png)

To generate another instance ID and token, call the `DeleteInstanceId` function.

?> This will also delete all unsent upstream messages.

## Set up the callbacks

Once you have set up the firebase console part of the application, you can start implementing the Cloud Messaging features by binding the important callbacks.

Call the `BindEventToOnMessageReceived` function to handle the remote messages on Android devices, and the `BindEventToOnRemoteNotificationReceived` for iOS.

?> The remote message on iOS is received as a JSON payload that you will need to parse yourself.

!> The above-mentioned callbacks are only invoked when the message is received by the application in foreground. On both Android and iOS the system notification is shown to user, if the application is in the background or killed.

You can setup additional callbacks to handle the upstream message sending from the application: `BindEventToOnMessagesDeleted, BindEventToOnMessageSent, BindEventToOnMessageSendError`.

?> These callbacks are only invoked on Android devices.

You can also subscribe to the token change events (`BindEventToOnNewToken`). This event is fired when the application is firstly opened on the device and after every `DeleteInstanceId` call.

## Send and receive messages

You can send upstream messages using the `SendMessage` method. You need to call the `NewRemoteMessageBuilder` to create a message builder object, set the necessary fields before sending it.

?> Note!!! Sending upstream messages only works if  your app server implements the XMPP Connection Server protocol.

![](images/firebase/cloud-messaging/CloudMessagingSendMessage.png)

You can also subscribe to and unsubscribe from specific topics for the downstream messages:

![](images/firebase/cloud-messaging/CloudMessagingSubscribeToTopic.png)

# **Crashlytics**

Official documentation regarding the Firebase Crashlytics can be found [here](https://firebase.google.com/docs/crashlytics).

The crashes of the application are automatically uploaded to Firebase once you have setup the Crashlytics in the Firebase console.

## Android Setup

If you are using Android NDK version 14 (UE4.24 and earlier) everything should work. For Android NDK version 21 (UE4.25 and newer) you will have to pass an additional linker flag `-Wl,--no-rosegment` for proper crash stack trace symbolication. Unfortunately it is impossible to pass this flag in the binary release of Unreal Engine. If you do not want to build the whole engine it is possible to only build the *Unreal Built Tool* and update the linker flags in *AndroidToolchain.cs*. For further instructions on building Unreal Engine from source please refer to the official documentation.

!> Right now the crashlytics stack trace will correctly report the file name and function where the crash happened but the rest of the stack will report unrelated UE source files. We are monitoring this issue and looking for a valid fix.

## iOS Setup

You will have to upload the Debug Symbols for your iOS application in order to see the reports.

You can download the required tool [here](https://drive.google.com/file/d/1L4O7wdkCatOQx_mgyJesw2TLYdk0cxIR/view?usp=sharing) or find it in the Plugin's Resources folder (`upload-symbols`).
In order for UE4 to generate these files during build, you have to go to Project Settings -> iOS -> Build and enable the following options:

![](images/firebase/crashlytics/CrashlyticsSettings.png)

Then you should run the following command in Terminal: `<PATH-TO-UPLOAD-SYMBOLS> -gsp <PATH-TO-GoogleService-Info.plist> -p ios <PATH-TO-PROJECT/Binaries/IOS/PROJECT_NAME.dSYM>` to upload the Debug Symbols to Firebase (for example, `/Users/Borsch/Downloads/Firebase/FirebaseCrashlytics/upload-symbols -gsp /Users/Borsch/Downloads/GoogleService-Info.plist -p ios /Users/Borsch/Projects/playground-unreal/Binaries/IOS/NinevaPlayground.dSYM`).
On some machines you will have to add permission using the `chmod +x` argument before the filepath (`chmod +x <command>`).

## Configuring session parameters

You can add custom parameters to add to the crashlytics reports, for example, User ID (`SetUserId`), and/or other custom parameters (`SetCustomBoolKey, SetCustomFloatKey, SetCustomIntKey, SetCustomLongKey, SetCustomStringKey`).

## Reporting errors

Use the `LogMessage` and `RecordException` functions to send error and exception reports.

?> On iOS `RecordException` is the same as `LogMessage`

You can toggle automatic data collection by calling `SetCrashlyticsCollectionEnabled`.
If it is disabled, you can use the `CheckForUnsentReports`, `SendUnsentReports` and `DeleteUnsentReports` functions to control the flow.

You can also check if the application crashed on previous launch using the `DidCrashOnPreviousExecution` function.

# **Cloud Functions**

In order to write and deploy cloud functions you need to set up Node.js and Firebase CLI, for more details refer to [functions setup guide](https://firebase.google.com/docs/functions/get-started#set-up-node.js-and-the-firebase-cli) and [project initialization](https://firebase.google.com/docs/functions/get-started#initialize-your-project).

!> Note: Use only the [functions.https](https://firebase.google.com/docs/reference/functions/providers_https_) backend API to write callable functions. The [HTTP trigger API](https://firebase.google.com/docs/reference/functions/cloud_functions_#httpsfunction) is entirely separate and not interoperable with callable functions.

After cloud functions are deployed you can view them in **Functions** tab in Firebase console.

![](images/firebase/cloud-functions/Scr_CloudFunctionsFunctionsConsoleTab.png)

In order to call them from your app use Cloud Function method of respective return type.

![](images/firebase/cloud-functions/Scr_CloudFunctions.png)

!> Note: If you don't specify region functions run in the ```us-central1``` region by default.

You can pass any amount of parameters of different types inside the ```Parameters``` map, including arrays and maps. To pass them use Value Variant convertor.

![](images/firebase/cloud-functions/Scr_CloudFunctionsValueVariantConv.png)

In case cloud function returns map in a callback use ```Break MapWrapper``` to extract map from incoming ```MapWrapper``` struct.

![](images/firebase/cloud-functions/Scr_CloudFunctionsBreakMapWrapper.png)
___

# Changelog

v 1.3.4

+ FIXED Issue on add child listerner not being triggered
+ FIXED Small blueprint issues

v.1.3.3

* FIXED iOS crash when no realtime database was created
* ADDED Separate settings for Crashlytics debug/release symbols upload for Android
* FIXED Crashes related to `OnNewToken` method on Cloud Messaging for Android
* IMPROVED Better CFBundleURLTypes plist handling in iOS UPL file
* IMPROVED Added logging to UPL files for easier debugging

v.1.3.0

* ADDED Cloud functions
* FIXED Crashlytics did not report crashes for native code on Android

v.1.2.1

* FIXED Crash on startup when JNI modules were not initialized
* FIXED Deprecation warning in Auth module

v.1.2.0

* ADDED Crashlytics
* ADDED Cloud messaging

v.1.1.0

* UPDATE Android SDK to version 17.5.0
* FIXED Proguard errors in shipping builds
* FIXED Various crashes

?> This release uses AndroidX libraries and may cause conflicts with other Android plugins

v.1.0.2

* UPDATE Settings screen

v.1.0.0

* Initial release

___

