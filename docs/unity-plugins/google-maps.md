<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/icon.png" width="256">

The plugin allows you to embed **Native [GoogleMapsView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView)** into your Android-Unity game. Note, this is **NOT** a Web View and **NOT** a Texture, its native interactive Google Map View.

* **Join our [Discord server](https://discord.gg/SuJP9fY) and ask us anything!**
* **[Download Demo from Play Store](https://play.google.com/store/apps/details?id=com.deadmosquitogames.gmaps.demo)**

# **Limitations**

**Please read the limitations carefully before purchasing:**

!> The view is **ALWAYS** shown on top of everything in your unity game. The view is implemented as an Android native dialog/iOS native view on top of Unity Activity.

!> Minimum supported Android version is **API level 16 (Jelly Bean)**

!> Minimum supported iOS version is **9.0**. 

!> **The plugin does NOT work in Editor! Only on iOS and Android** It is a native Android/iOS view (not a web view) so performance is awesome but there is no way to get native Android/iOS view working in Unity Editor.

!> **You can't move the view as a part of scene hierarchy (e.g scroll in Unity view)**. However, you can change the view size and position. 

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/map_demo.png">

# **Setup (Android)**

## 1. Change the default Bundle id to your Bundle Id (package name)

Go to Unity Android Player Settings and set the Bundle Id as your package name, e.g. `gmaps.deadmosquitogames.com.googlemaps` and save it. **This is important as Google API Key in the next step will be restricted to the package you set.**

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/bundle_id.png">

## 2. Obtaining Google API Key

This part is a bit tricky so please follow instructions carefully.

If you don't already have a [Google Console](https://console.developers.google.com) account create one and login.

* Go to [https://cloud.google.com/maps-platform/](https://cloud.google.com/maps-platform) and click `GET STARTED` button.

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-1.PNG">

* Select `Maps` product and continue

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-2.PNG">

* Select an already existing project or create a new one in the dropdown

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-3.PNG">

* If you do not have the billing account yet, you would be asked to create one, please follow the instructions

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-4.PNG">

* Click next to finally create an API key

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-5.PNG">

You will be now presented with the API key, please save it as we will need it later. Click on `API Console` link below the key to go your API key settings.

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-6.PNG">

What we need to do now and its very important that we restrict usage of this key to only our Android application (So other people can't use it if the obtain your key). In `Key restriction section` choose `Android apps` and click on `+ Add package name and fingerprint` button.

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/get_key/get_key_5.png">

The form that appears maps app package names to SHA-1 certificate fingerprints. Put **Package name** from your unity project that you set up in **Step 1*** into the package field.

### Obtaining SHA-1 certificate fingerprint

To obtain **SHA-1 certificate fingerprint** run this command in your terminal pointing to your keystore that application is signed with:

`keytool -list -v -keystore mystore.keystore` 

For example in my case it is `keytool -list -v -keystore ~tarasleskiv/.android/debug.keystore` as I am using default debug keystore. (Default option in Unity).

If you use your Windows machine its very similar, in my case I specified full path to keytool: `"C:\Program Files\Java\jdk1.8.0_91\bin\keytool" -list -v -keystore C:\Users\tarasleskiv\.android\debug.keystore` .

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/get_key/get_key_7.png" width = "512">

* Now copy your **SHA-1 certificate fingerprint** into the form in Google Console. After you filled in all the information click `Save` . 

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/get_key/get_key_6.png" width = "512">

* Repeat adding package-**SHA-1 certificate fingerprint** pairs for all keystores that you sign the app with. For example I usually have two entries - one with my debug keystore to develop locally and another pair for Google Play publishing.

!> **Note: It may take up to 5 minutes for settings to take effect after you save them**

### Adding Google Play **SHA-1 certificate fingerprint** if you use [Google Play App Signing](https://support.google.com/googleplay/android-developer/answer/7384423)

If you are not using new Google Play App Signing mechanism please skip this part.

If you are using new Google Play App Signing mechanism you also have to create another entry that mps your package name to app signing certificate that Google generates for you. You can find your app signing certificate and **SHA-1 certificate fingerprint** under Release Management -> App Signing. This is the **SHA-1 certificate fingerprint** that you have to use.

You have to do this because with the new [Google Play App Signing](https://support.google.com/googleplay/android-developer/answer/7384423) Google re-signs your app with another certificate. If you forget this place picker functionality will not work when downloaded from Google Play.

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/new-signing.png">

## 3. Set the API key in Unity Project settings.

Once you have your key (it starts with "AIza"), go to _Window -> Google Maps View -> Edit Settings_ and replace the value in the corresponding input field with the API key that you recently retrieved.

<img src="https://github.com/NinevaStudios/unity-google-maps-docs/blob/master/images/android_api_key_settings.png">

## 4. Run the Demo Scene

* Open Unity Build Settings and switch Platform to Android
* Add `GoogleMapsView/Example/ExampleScene.unity` to `Scenes in Build` 
* Connect Android device and run the scene (Device must have Google Play Services installed)

After running the demo you will see the demo scene, now you can play around with settings. Don't forget to click "Refresh" each time you change settings.

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/map_demo.png">a

# **Setup (iOS**)

## 1. Change the default Bundle id to your Bundle Id (package name)

Go to Unity Android Player Settings and set the Bundle Id as your package name, e.g. `gmaps.deadmosquitogames.com.googlemaps` and save it. **This is important as Google API Key in the next step will be restricted to the package you set.**

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/bundle_id.png">

## 2. Obtaining Google API Key

This part is a bit tricky so please follow instructions carefully.

If you don't already have a [Google Console](https://console.developers.google.com) account create one and login.

* Go to [https://cloud.google.com/maps-platform/](https://cloud.google.com/maps-platform) and click `GET STARTED` button.

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-1.PNG">

* Select `Maps` product and continue

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-2.PNG">

* Select an already existing project or create a new one in the dropdown

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-3.PNG">

* If you do not have the billing account yet, you would be asked to create one, please follow the instructions

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-4.PNG">

* Click next to finally create an API key

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-5.PNG">

You will be now presented with the API key, please save it as we will need it later. Click on `API Console` link below the key to go your API key settings.

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/setup_new/get-api-key-6.PNG">

What we need to do now and its very important that we restrict usage of this key to only our Android application (So other people can't use it if the obtain your key). In `Key restriction section` choose `Android apps` and click on `+ Add package name and fingerprint` button.

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/get_key/get_key_5.png">

The form that appears maps app package names to SHA-1 certificate fingerprints. Put **Package name** from your unity project that you set up in **Step 1*** into the package field.

### Obtaining SHA-1 certificate fingerprint

To obtain **SHA-1 certificate fingerprint** run this command in your terminal pointing to your keystore that application is signed with:

`keytool -list -v -keystore mystore.keystore` 

For example in my case it is `keytool -list -v -keystore ~tarasleskiv/.android/debug.keystore` as I am using default debug keystore. (Default option in Unity).

If you use your Windows machine its very similar, in my case I specified full path to keytool: `"C:\Program Files\Java\jdk1.8.0_91\bin\keytool" -list -v -keystore C:\Users\tarasleskiv\.android\debug.keystore` .

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/get_key/get_key_7.png" width = "512">

* Now copy your **SHA-1 certificate fingerprint** into the form in Google Console. After you filled in all the information click `Save` . 

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/get_key/get_key_6.png" width = "512">

* Repeat adding package-**SHA-1 certificate fingerprint** pairs for all keystores that you sign the app with. For example I usually have two entries - one with my debug keystore to develop locally and another pair for Google Play publishing.

> **Note: It may take up to 5 minutes for settings to take effect after you save them**

### Adding Google Play **SHA-1 certificate fingerprint** if you use [Google Play App Signing](https://support.google.com/googleplay/android-developer/answer/7384423)

If you are not using new Google Play App Signing mechanism please skip this part.

If you are using new Google Play App Signing mechanism you also have to create another entry that maps your package name to app signing certificate that Google generates for you. You can find your app signing certificate and **SHA-1 certificate fingerprint** under Release Management -> App Signing. This is the **SHA-1 certificate fingerprint** that you have to use.

You have to do this because with the new [Google Play App Signing](https://support.google.com/googleplay/android-developer/answer/7384423) Google re-signs your app with another certificate. If you forget this place picker functionality will not work when downloaded from Google Play.

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/new-signing.png">

## 3. Set the API key in Unity Project settings.

Once you have your key (it starts with "AIza"), go to _Window -> Google Maps View -> Edit Settings_ and replace the value in the corresponding input field with the API key that you recently retrieved.

<img src="https://github.com/NinevaStudios/unity-google-maps-docs/blob/master/images/android_api_key_settings.png">

## 4. Run the Demo Scene

* Open Unity Build Settings and switch Platform to Android
* Add `GoogleMapsView/Example/ExampleScene.unity` to `Scenes in Build` 
* Connect Android device and run the scene (Device must have Google Play Services installed)

After running the demo you will see the demo scene, now you can play around with settings. Don't forget to click "Refresh" each time you change settings.

<img src="https://github.com/TarasOsiris/unity-google-maps-docs/blob/master/images/map_demo.png">

# Getting Started

# Drawing elements

# Marker Clustering

# Drawing Heatmaps

# Map Events and Listeners

# Showing User Location

# Changing UI Settings

# Animating/Moving Camera

# JSON Map Styling

# Taking Map Snapshot

# BONUS! Opening Google Maps app

***

# Issues & FAQ

# Troubleshooting
