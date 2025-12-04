[filename](common/common_ue_header.md ":include")

# **Google Maps View**

!> **Please read the [limitations](#limitations) before purchasing**

---

[Download Demo from playstore](https://play.google.com/store/apps/details?id=com.ninevastudios.unrealmaps)

The plugin allows you to embed **Native [GoogleMapsView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView)** into your Unreal Engine 4 game on Android. Note, this is **NOT** a Web View and **NOT** a Texture, its native interactive Google Map View.

# **Limitations**

**Please read the limitations carefully before purchasing**

- Minimum supported Android version is **API level 16 (Jelly Bean)**

- The view is **ALWAYS** shown on top of everything in your UE4 game. The view is implemented as an Android native dialog on top of UE4 `GameActivity`.

- The plugin does NOT work in Editor! It is a native Android/iOS view (not a web view), so performance is awesome but there is no way to get native Android/iOS view working in Unreal Editor or desktop.

- You have to handle device orientation changes.

- **You can't move the view (e.g scroll in Unreal view)**. The view for now is static and can't be moved around.

# **Setup (Plugin)**

Follow these steps to use GoogleMapsView plugin in your project.

1. Copy folder with GoogleMapsView plugin to _Plugins_ folder in the root of your project.

![](images/google-maps/CopyFiles.png)

2. Go to Unreal _Editor Edit -> Plugins -> Code Plugins_ screen and make sure that GoogleMapsView is enabled.

![](images/google-maps/PluginEditor.png)

Now you should be able to use plugin functionality in blueprints.

To use it in C++ add GoogleMapsView module dependency to your project `*.Build.cs` file:

```csharp
PublicDependencyModuleNames.AddRange(new string[] { "Core", ... , "GoogleMapsView" });
```

3. Follow setup steps for:

- [Android](#setupandroid)
- [iOS](#setupios)

# **Setup (Android)**

## Enable _Maps SDK for Android_ API in Gooogle Console

Go to [Google Console dashboard](https://console.developers.google.com/apis/dashboard) and click on `ENABLE APIS AND SERVICES` button, then find **Maps SDK for Android** and make sure it is enabled.

![](images/google-maps/enable-api-1.png)
![](images/google-maps/enable-api-2.png)
![](images/google-maps/enable-api-3.png)

## Change the default Bundle id to your Bundle Id (package name)

Go to Unreal Editor Project Settings and set the Package Name, e.g. `com.ninevastudios.googlemapsview` and save it. **This is important as Google API Key in the next step is bound to the package you set.** In this document I will refer to your package as `${YOUR_PACKAGE_NAME}`

![](images/google-maps/PackageName.png)

## Obtaining Google API Key

This part is a bit tricky so please follow instructions carefully.

- If you don't already have a [Google Console](https://console.developers.google.com) account create one and login.
- Go to [Android API documentation](https://developers.google.com/maps/documentation/android-sdk/get-api-key#get-the-api-key) and follow the instructions on how to get an API key.

![](images/google-maps/get_api_key_1.png)

- Now copy your key and save it, you will need it later. After this click `Restrict key` button.

![](images/google-maps/get_api_key_2.png)

- What we need to do now and its very important that we restrict usage of this key to only our Android application (So other people can't use it if the obtain your key). In `Key restriction section` choose `Android apps` and click on `+ Add package name and fingerprint` button.

![](images/google-maps/get_api_key_3.png)

- The form that appears maps app package names to SHA-1 certificate fingerprints. Put **Package name** from your project that you set up in [Step 1](#change-the-default-bundle-id-to-your-bundle-id-package-name) into the package field.

### Obtaining _SHA-1 certificate fingerprint_

To obtain **SHA-1 certificate fingerprint** run this command in your terminal pointing to your keystore that application is signed with:

`keytool -list -v -keystore mystore.keystore`

For example in my case it is `keytool -list -v -keystore ~tarasleskiv/.android/debug.keystore` as I am using default debug keystore.

If you use your Windows machine its very similar, in my case I specified full path to keytool: `"C:\Program Files\Java\jdk1.8.0_91\bin\keytool" -list -v -keystore C:\Users\tarasleskiv\.android\debug.keystore`.

![](images/google-maps/get_key_7.png)

- Now copy your **SHA-1 certificate fingerprint** into the form in Google Console. After you filled in all the information click `Save`.

![](images/google-maps/get_key_6.png)

- Repeat adding package-**SHA-1 certificate fingerprint** pairs for all keystores that you sign the app with. For example I usually have two entries - one with my debug keystore to develop locally and another pair for Google Play publishing.

!> It may take up to 5 minutes for settings to take effect after you save them

### Adding Google Play _SHA-1 certificate fingerprint_ if you use Google Play App Signing

?> If you are not using new [Google Play App Signing](https://support.google.com/googleplay/android-developer/answer/7384423) mechanism please skip this part.

If you are using new Google Play App Signing mechanism you also have to create another entry that maps your package name to app signing certificate that Google generates for you. You can find your app signing certificate and **SHA-1 certificate fingerprint** under Release Management -> App Signing. This is the **SHA-1 certificate fingerprint** that you have to use.

You have to do this because with the new [Google Play App Signing](https://support.google.com/googleplay/android-developer/answer/7384423) Google re-signs your app with another certificate. If you forget this place picker functionality will not work when downloaded from Google Play.

![](images/google-maps/new-signing.png)

## Put the API key in the plugin setting in Project Settings.

Go to your Project Settings, find the plugin settings section and place your API keys into the corresponding Android/iOS fields.

## Run the Demo Project

- Open demo level in _Content Browser -> Content -> Levels_
- Connect Android device and launch the level (Device must have Google Play Services installed)

After running the application on your device you will see the demo scene, now you can play around with the map.

![](images/google-maps/AppScreen1.png)

# **Setup (iOS)**

## What's different on iOS?

There are a few differences and things to note about iOS implementation:

- Most of the functionality is identical but there are some methods and properties that can be used only on Android. This means referring this functionality will have no effect on iOS.
- API key is provided by calling `UGoogleMapsViewBlueprintLibrary::SetAPIKey(apiKey)` function unlike providing it in the manifest file on Android.

## Make sure that Google Maps is enabled for iOS SDK

![](images/google-maps/enabled_ios_1.png)
![](images/google-maps/enabled_ios_2.png)
![](images/google-maps/enabled_ios_3.png)

## Getting Google Maps API Key from Google Developer Console

Please [follow the same steps from Android setup](#setupandroid) but choose `iOS Apps` when you get to the **Credentials** screen. Here you can optionally restrict key usage to certain application packages.

## Setting API key in the app

Unlike Android where you have to provide the key in the `AndroidManifest.xml` file in iOS you have to call the method **BEFORE** any calls to the API.

Setting API key via blueprint:

![](images/google-maps/SetAPIKeyIOS.png)

or in C++:

```cpp
UGoogleMapsViewBlueprintLibrary::SetAPIKey("YOUR_API_KEY_HERE");
```

**_Note!_** When building for iPhone XS/XS Max, add these lines of code to the Engine/UE4/Source/UE4Game.Target.cs and Games/YourProjectName/Source/YourProjectName.Target.cs:

```csharp
if (Target.Platform == UnrealTargetPlatform.IOS)
{
     GlobalDefinitions.Add("FORCE_ANSI_ALLOCATOR=1");
}
```

Otherwise, you might experience app crashes when creating map view followed by error messages like "attempting to free pointer we didn't allocate".
More info about this issue can be found [here](https://www.artstation.com/alyamkin/blog/9VEQ/ue4-ios-and-pointer-being-freed-was-not-allocated).

# **Getting started**

## Creating, Showing and Dismissing GoogleMapView

To create GoogleMapView simply call `CreateMapView` function. After this call `Show` method passing position, dimensions and options as parameters. You can create and show multiple map view instances at the same time.

Displaying GoogleMapView via blueprint:

![](images/google-maps/CreateAndShow.png)

or in C++:

```cpp
TScriptInterface<IUnrealMapsViewInterface> MapView = UUnrealMapsBlueprintLibrary::CreateMapsView();

MapView->Show(100.0f, 150.0f, 300.0f, 300.0f, GoogleMapOtions);
```

Dismissing GoogleMapView via blueprint:

![](images/google-maps/Dismiss.png)

or in C++:

```cpp
MapView->Dismiss();
```

Be aware that you won't be able to work with map after dismissing it.

## Google Map Options

To customize what location is shown on the map and many many more settings, set all the configurations on the options object that you pass to view `Show` method. For more information on each option please refer to [GoogleMapOptions Documentation](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) where they are explained in detail.

Example of creating map view configuration via blueprint:

![](images/google-maps/GoogleMapOptionsInit.png)

or in C++:

```cpp
FGoogleMapOptions InitMapOtions()
{
	FGoogleMapOptions GoogleMapOtions;

	GoogleMapOtions.AmbientEnabled = false;

	// Camera position
	GoogleMapOtions.CameraPosition.Target = FLatLng{ 52.49f, 13.39f };
	GoogleMapOtions.CameraPosition.Tilt = 1.0f;
	GoogleMapOtions.CameraPosition.Zoom = 10.0f;

	// Specifies a FLatLngBounds to constrain the camera target so that when users
	// scroll and pan the map, the camera target does not move outside these bounds
	GoogleMapOtions.LatLngBounds = FLatLngBounds { FLatLng{ 53.0f, 14.0f } , FLatLng{ 47.0f, 6.0f } }

	GoogleMapOtions.LiteMode = false;

	GoogleMapOtions.MapType = GoogleMapType::Normal;

	GoogleMapOtions.MaxZoomPreference = 16.0f;
	GoogleMapOtions.MinZoomPreference = 1.0f;

	return GoogleMapOtions;
}
```

# **Map Callbacks**

## Using Map Callbacks

To handle map callbacks bind custom events to delegates exposed by GoogleMapsView plugin API.

Get `Map View Callback Proxy` reference from the map view and bind your handler to one of its delegates.

Add map callback handler via blueprint:

![](images/google-maps/Binding.png)

or in C++:

```cpp

// Declare your callback handler function in *.h file
// NOTE that it has to be UFUNCTION!

UFUNCTION()
void OnMapViewReady();

// Bind your function to map view delegate in *.cpp file
MapView->GetCallbackProxy()->OnMapReadyDynamicDelegate.AddDynamic(this, &UMyTestClass::OnMapViewReady);
```

## Available Callbacks

- **OnMapReadyDynamicDelegate** - Fires when Map View is fully initialized. Ensure that your application uses Map View only after this event is received.
- **OnMapClickDynamicDelegate** - Fires when user taps on Map View. Receives latitude and longitude of clicked location point.
- **OnMarkerClickDelegate** - Fires when user taps on the Marker. Receives clicked marker reference.

# **Markers**

To get more details about markers check [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) and [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) Google API.

![](images/google-maps/Markers.png)

## Adding markers

To add marker to map call `AddMarker` method on GoogleMapView and pass marker options as a parameter.

Adding marker via blueprint:

![](images/google-maps/AddMarker.png)

or in C++:

```cpp
FMarkerOptions MarkerOptions;

// Currently all MarkerOptions fields should be initialized explicitly (will be fixed if future releases)

MarkerOptions.Alpha = 1.0f;
MarkerOptions.AnchorU = 0.5f;
MarkerOptions.AnchorV = 1.0f;
MarkerOptions.InfoWindowAnchorU = 0.5f;
MarkerOptions.InfoWindowAnchorV = 0.0f;
MarkerOptions.Draggable = false;
MarkerOptions.Flat = false;
MarkerOptions.Position = latLng;
MarkerOptions.Rotation = 0.0f;
MarkerOptions.Title = "Title";
MarkerOptions.Snippet = "Description";
MarkerOptions.Visible = true;
MarkerOptions.ZIndex = 0.0f;

TScriptInterface<IMarkerInterface> NewMarker = MapView->AddMarker(MarkerOptions);
```

## Removing markers

To remove marker simply call its `Remove` method.

Removing marker via blueprint:

![](images/google-maps/RemoveMarker.png)

or in C++:

```cpp
Marker->Remove();
```

Note that you won't be able to use marker after it was removed.

## Reading and modifying marker options

You can read and modify marker properties by calling corresponding getters and setters.

For more details check out the Google Maps view markers documentation for [Android](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) and [iOS](https://developers.google.com/maps/documentation/ios-sdk/reference/interface_g_m_s_marker)

Usage of some marker properties is limited depending on the mobile platform:

- `GetAnchor` function not supported on Android
- `GetWindowAnchor` not supported on Android
- `GetIsVisible` function not supported on iOS
- `SetIsVisible` function not supported on iOS

## Setting marker icon

You can change the marker icon by calling the `SetIcon()` function and specifying the Texture2D to use as a new icon and scale factor (used for iOS). Leaving the Texture2D field empty will result in setting the default red marker icon. The scale factor for iOS is useful when you are using big textures. Providing the value of 10 will result in the marker image being ten times smaller, the value of 0.1 will make it ten times bigger.

**_Note!_** Textures used for marker icons should have the following settings: Compression Settings - VectorDisplacementMap, MipGenSettings - NoMipMaps, sRGB - false. Textures can sometimes display some artifacts when applied as marker icons. This can often be removed if you convert the image to .tga format before importing it in your Unreal project.
