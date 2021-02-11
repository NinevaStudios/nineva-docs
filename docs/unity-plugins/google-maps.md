# **Google Maps View**

![](/images/maps/icon.png ':size=128')

?> **[PLEASE READ THE FAQS BEFORE CONTACTING SUPPORT!!!](#faq)**

The plugin allows you to embed **Native [GoogleMapsView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView)** into your Android-Unity game. Note, this is **NOT** a Web View and **NOT** a Texture, its native interactive Google Map View.

* **Join our [Discord server](https://bit.ly/nineva_support_discord) and ask us anything!**
* **[Download Demo from Play Store](https://play.google.com/store/apps/details?id=com.deadmosquitogames.gmaps.demo)**

# **Limitations**

**Please read the limitations carefully before purchasing:**

!> The view is **ALWAYS** shown on top of everything in your unity game. The view is implemented as an Android native dialog/iOS native view on top of Unity Activity.

!> Minimum supported Android version is **API level 16 (Jelly Bean)**

!> UI elements of the map (zoom buttons, compass, etc.) are not displayed on Android 7.0 and older. It is the most annoying limitation, but after some internal changes in Unity 2020 I can't find the fix for this. If you are still using Unity 2019, please drop me a line, and I will send you the package without this issue.

![](/images/maps/ui_elements_bug.png ':size=512')

!> Minimum supported iOS version is **9.0**.

!> **The plugin does NOT work in Editor! Only on iOS and Android** It is a native Android/iOS view (not a web view), so performance is awesome, but there is no way to get native Android/iOS view working in Unity Editor.

!> **You can't move the view as a part of scene hierarchy (e.g scroll in Unity view)**. However, you can change the view size and position.

![](/images/maps/map_demo.png ':size=768')

# **Setup (Android)**

## 1. Change the default Bundle id to your Bundle Id (package name)

Go to Unity Android Player Settings and set the Bundle Id as your package name, e.g. `gmaps.deadmosquitogames.com.googlemaps` and save it. **This is important as Google API Key in the next step will be restricted to the package you set.**

![](/images/maps/bundle_id.png ':size=768')

## 2. Obtaining Google API Key

This part is a bit tricky so please follow instructions carefully.

If you don't already have a [Google Console](https://console.developers.google.com) account create one and login.

* Go to [https://cloud.google.com/maps-platform/](https://cloud.google.com/maps-platform) and click `GET STARTED` button.

![](/images/maps/get-api-key-1.png ':size=768')

* Select `Maps` product and continue

![](/images/maps/get-api-key-2.png ':size=768')

* Select an already existing project or create a new one in the dropdown

![](/images/maps/get-api-key-3.png ':size=768')

* If you do not have the billing account yet, you would be asked to create one, please follow the instructions

![](/images/maps/get-api-key-4.png ':size=768')

* Click next to finally create an API key

![](/images/maps/get-api-key-5.png ':size=768')

You will be now presented with the API key, please save it as we will need it later. Click on `API Console` link below the key to go your API key settings.

![](/images/maps/get-api-key-6.png ':size=768')

What we need to do now and its very important that we restrict usage of this key to only our Android application (So other people can't use it if the obtain your key). In `Key restriction section` choose `Android apps` and click on `+ Add package name and fingerprint` button.

![](/images/maps/get_key_5.png ':size=768')

The form that appears maps app package names to SHA-1 certificate fingerprints. Put **Package name** from your unity project that you set up in **Step 1*** into the package field.

### Obtaining SHA-1 certificate fingerprint

To obtain **SHA-1 certificate fingerprint** run this command in your terminal pointing to your keystore that application is signed with:

`keytool -list -v -keystore mystore.keystore`

For example in my case it is `keytool -list -v -keystore ~tarasleskiv/.android/debug.keystore` as I am using default debug keystore. (Default option in Unity).

If you use your Windows machine its very similar, in my case I specified full path to keytool: `"C:\Program Files\Java\jdk1.8.0_91\bin\keytool" -list -v -keystore C:\Users\tarasleskiv\.android\debug.keystore` .

![](/images/maps/get_key_7.png ':size=768')

* Now copy your **SHA-1 certificate fingerprint** into the form in Google Console. After you filled in all the information click `Save` .

![](/images/maps/get_key_6.png ':size=768')

* Repeat adding package-**SHA-1 certificate fingerprint** pairs for all keystores that you sign the app with. For example I usually have two entries - one with my debug keystore to develop locally and another pair for Google Play publishing.

!> **Note: It may take up to 5 minutes for settings to take effect after you save them**

### Adding Google Play *SHA-1 certificate fingerprint* if you use Google Play App Signing

?> If you are not using new [Google Play App Signing](https://support.google.com/googleplay/android-developer/answer/7384423) mechanism please skip this part.

If you are using new Google Play App Signing mechanism you also have to create another entry that maps your package name to app signing certificate that Google generates for you. You can find your app signing certificate and **SHA-1 certificate fingerprint** under Release Management -> App Signing. This is the **SHA-1 certificate fingerprint** that you have to use.

You have to do this because with the new [Google Play App Signing](https://support.google.com/googleplay/android-developer/answer/7384423) Google re-signs your app with another certificate. If you forget this place picker functionality will not work when downloaded from Google Play.

![](/images/maps/new-signing.png ':size=768')

## 3. Set the API key in Unity Project settings.

Once you have your key (it starts with "AIza"), go to _Window -> Google Maps View -> Edit Settings_ and replace the value in the corresponding input field with the API key that you recently retrieved.

![](/images/maps/android_api_key_settings.png ':size=768')

## 3.1 Set the proguard file for the release build

There is a file included in the asset in `Assets/Plugins/Android/proguard-user.txt` or you can copt this configuration to your proguard file:

```text
-keep public class com.deadmosquitogames.gmaps.** { *; }
-keep public class com.google.android.gms.maps.** { *; }
-keep public class com.google.maps.** { *; }
```

![](/images/maps/proguard_settings.png ':size=768')

## 4. Run the Demo Scene

* Open Unity Build Settings and switch Platform to Android
* Add `GoogleMapsView/Example/ExampleScene.unity` to `Scenes in Build`
* Connect Android device and run the scene (Device must have Google Play Services installed)

After running the demo you will see the demo scene, now you can play around with settings. Don't forget to click "Refresh" each time you change settings.

![](/images/maps/map_demo.png ':size=768')a

# **Setup (iOS)**

## 1. Make sure that Google Maps is enabled for iOS SDK

![](images/maps/enabled_ios_1.png)
![](images/maps/enabled_ios_2.png)
![](images/maps/enabled_ios_3.png)

## 2 Getting Google Maps API Key from Google Developer Console

Please [follow the same steps from Android setup](#setup-android) but choose `iOS Apps` when you get to the **Credentials** screen. Here you can optionally restrict key usage to certain application packages.

## 3. Set the API key in Unity Project settings.

Once you have your key (it starts with "AIza"), go to _Window -> Google Maps View -> Edit Settings_ and replace the value in the corresponding input field with the API key that you recently retrieved.

![](/images/maps/ios_api_key_settings.png ':size=768')

## 4. Run the Demo Scene

* Open Unity Build Settings and switch Platform to Android
* Add `GoogleMapsView/Example/ExampleScene.unity` to `Scenes in Build`
* Connect Android device and run the scene (Device must have Google Play Services installed)

After running the demo you will see the demo scene, now you can play around with settings. Don't forget to click "Refresh" each time you change settings.

![](/images/maps/map_demo.png ':size=768')

# **Getting Started**

For reference see the Example folder inside unitypackage. It must provide exhaustive overview of plugin functionality.

## Creating, Showing and Dismissing GoogleMapView

To create the google map view simply create an object and pass the created options to the constructor. After this call `Show` method passing rectangle that represents the position on the screen as a parameter. You can create and show multiple map view instances at the same time.

Minimum code to show the map:

``` csharp
var options = new GoogleMapsOptions();
var map = new GoogleMapsView(options);
var screenPosition = new Rect(10, 10, 300, 300);
map.Show(screenPosition);
```

Dismissing the map:

``` csharp
map.Dismiss();
```

## Google Map Options

 To customize what location is shown on the map and many many more settings, set all the configurations on the options object that you pass to view constructor. For more information on each method please refer to [GoogleMapOptions Documentation](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) where each method is explained in detail.

Example of creating map view configuration:

``` csharp
GoogleMapsOptions CreateMapViewOptions()
{
    var options = new GoogleMapsOptions();

    options.MapType(GoogleMapType.Hybrid);

    // Camera position
    options.Camera(new CameraPosition(
            new LatLng(camPosLat.value, camPosLng.value),
            camPosZoom.value,
            camPosTilt.value,
            camPosBearing.value));

    // Specifies a LatLngBounds to constrain the camera target
    // so that when users scroll and pan the map, the camera target does not move outside these bounds.
    var southWest = new LatLng(_boundsSouthWestPosLat, _boundsSouthWestPosLng);
    var northEast = new LatLng(_boundsNorthEastPosLat, _boundsNorthEastPosLng);
    options.LatLngBoundsForCameraTarget(new LatLngBounds(southWest, northEast));

    // Other settings
    options.AmbientEnabled(ambientToggle.isOn);
    options.CompassEnabled(compassToggle.isOn);
    options.LiteMode(liteModeToggle.isOn);
    options.MapToolbarEnabled(mapToolbarToggle.isOn);
    options.RotateGesturesEnabled(rotateGesturesToggle.isOn);
    options.ScrollGesturesEnabled(scrollGesturesToggle.isOn);
    options.TiltGesturesEnabled(tiltGesturesToggle.isOn);
    options.ZoomGesturesEnabled(zoomGesturesToggle.isOn);
    options.ZoomControlsEnabled(zoomControlsToggle.isOn);

    options.MinZoomPreference(float.Parse(minZoom.text));
    options.MaxZoomPreference(float.Parse(maxZoom.text));

    return options;
}
```

## Showing and Hiding GoogleMapView

To show/hide the map without losing state call `_map.IsVisible = true;` or `_map.IsVisible = false;`

## Handling orientation changes

If you would like to handle orientation changes please subscribe to `OnOrientationChange` event on `GoogleMapView` :

``` csharp
_map.OnOrientationChange += () => { _map.SetRect(RectTransformToScreenSpace(rect)); };
```

# **Drawing elements**

You can draw elements on the map after it has finished loading. When calling `Show()` method, pass a callback that will be invoked when the map has finished loading, that means the map is ready to draw on:

```csharp
public void Show()
{
    _map = new GoogleMapsView(CreateMapViewOptions());
    _map.Show(RectTransformToScreenSpace(rect), OnMapReady);
}

void OnMapReady()
{
    // Map is ready to draw on
    AddCircle();
    AddMarker();
    AddGroundOverlay();
}
```

Currently supported elements are:
+ [Markers](#markers)
+ [Circles](#circles)
+ [Ground Overlays](#ground-overlay)
+ [Polylines](#polylines)
+ [Polygons](#polygons)

![](/images/maps/draw_on_map.png ':size=768')

## Markers

The API is mirrored from [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) and [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) Google API, please refer it for all the reference and also see the detailed examples in the demo project.

If you want to register click listener for marker please refer to [Listening to map events](#map-events-and-listeners)

Example:

```csharp
Marker marker = _map.AddMarker(CreateInitialMarkerOptions());

static MarkerOptions CreateInitialMarkerOptions()
{
    const float LondonLatitude = 51.5285582f;
    const float LondonLongitude = -0.2417005f;

    // Create a amrker in London, Great Britain
    return new MarkerOptions()
        .Position(new LatLng(LondonLatitude, LondonLongitude))
        .Icon(ImageDescriptor.FromAsset("map-marker-icon.png")) // image must be in StreamingAssets folder!
        .Alpha(0.5f) // make semi-transparent image
        .Anchor(0.5f, 1f) // anchor point of the image
        .InfoWindowAnchor(0.5f, 1f)
        .Draggable(true)
        .Flat(false)
        .Rotation(30f) // Rotate marker a bit
        .Snippet("Snippet Text")
        .Title("Title Text")
        .Visible(true)
        .ZIndex(1f);
}
```

![](/images/maps/markers.png ':size=768')

## Circles

The API is mirrored from [CircleOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions) and [Circle](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Circle) Google API, please refer it for all the reference and also see the detailed examples in the demo project.

If you want to register click listener for circle please refer to [Listening to map events](#map-events-and-listeners)

Example:

```csharp
Circle circle = _map.AddCircle(CreateInitialCircleOptions());

static CircleOptions CreateInitialCircleOptions()
{
    const float SydneyLatitude = -34;
    const float SydneyLongitude = 151;

    // Create a circle in Sydney, Australia
    return new CircleOptions()
        .Center(new LatLng(SydneyLatitude, SydneyLongitude))
        .Radius(100000f)
        .StrokeWidth(5f)
        .StrokeColor(Color.red)
        .FillColor(Color.green)
        .Visible(true)
        .Clickable(true)
        .ZIndex(1);
}
```

![](/images/maps/circles.png ':size=768')

## Ground Overlay

The API is mirrored from [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions) and [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) Google API, please refer it for all the reference and also see the detailed examples in the demo project.

Example:

```csharp
GroundOverlay groundOverlay = _map.AddGroundOverlay(CreateInitialGroundOverlayOptions());

static GroundOverlayOptions CreateInitialGroundOverlayOptions()
{
    return new GroundOverlayOptions()
//                .Position(new LatLng(BerlinLatitude, BerlinLongitude), 303000, 150000)
        .PositionFromBounds(BerlinLatLngBounds)
        .Image(ImageDescriptor.FromAsset("overlay.png")) // image must be in StreamingAssets folder!
        .Anchor(1, 1)
        .Bearing(45)
        .Clickable(true)
        .Transparency(0)
        .Visible(true)
        .ZIndex(1);
}
```

![](/images/maps/overlay.png ':size=768')

## Polylines

The API is mirrored from [PolylineOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/PolylineOptions) and [Polyline](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline) Google API, please refer it for all the reference and also see the detailed examples in the demo project.

Example:

```csharp
var polyline = _map.AddPolyline(CreateInitialPolylineOptions());

public static PolylineOptions CreateInitialPolylineOptions()
{
    return new PolylineOptions()
        .Add(new LatLng(10, 10), new LatLng(30, 30), new LatLng(-30, 30), new LatLng(50, 50))
        .Clickable(true)
        .Color(Color.red)
        .StartCap(new CustomCap(ImageDescriptor.FromAsset("cap.png"), 16f))
        .EndCap(new RoundCap())
        .JointType(JointType.Round)
        .Geodesic(false)
        .Visible(true)
        .Width(20)
        .ZIndex(1f);
}
```

![](/images/maps/polyline.png ':size=768')

## Polygons

The API is mirrored from [PolygonOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/PolygonOptions) and [Polygon](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polygon) Google API, please refer it for all the reference and also see the detailed examples in the demo project.

Example:

```csharp
var coloradoPolygon = _map.AddPolygon(CreateColoradoStatePolygonOptions());

public static PolygonOptions CreateColoradoStatePolygonOptions()
{
    return new PolygonOptions()
        .Add(ColoradoBorders)
        .FillColor(new Color(0.5f, 0.5f, 0.5f, 0.2f))
        .StrokeColor(new Color(0.5f, 0f, 0f, 0.8f))
        .StrokeWidth(20f)
        .StrokeJointType(JointType.Round)
        .AddHole(ColoradoHole)
        .Visible(true)
        .Clickable(true)
        .Geodesic(false)
        .ZIndex(1f);
}
```
![](/images/maps/polygon.png ':size=768')

# **Marker Clustering**

![](/images/maps/clustering.jpg ':size=768')

This implements the marker clustering functionality from Android iOS and Android utility libraries, for more background information please check out the official Google documentation:

* **Android**: https://developers.google.com/maps/documentation/android-api/utility/marker-clustering
* **iOS**: https://developers.google.com/maps/documentation/ios-sdk/utility/marker-clustering

## Available features

If after reading the information below you are still not sure if the feature you need is there, please [contact me](mailto:support@ninevastudios.com).

Please read what subset of features is available in the plugin (for now it is only the subset of all clustering features):
* Adding items to the cluster with a location, title and snippet text
* Adding a single item or a list of items to the cluster is both supported
* Clearing all the cluster items from the map
* When marker/cluster is clicked the [OnMarkerClickListener](#marker-click) would be called if you set it before.
* When marker info window is clicked the [OnInfoWindowClickListener](#marker-info-window-click) would be called if you set it before.

## Creating marker cluster

To create a cluster you have to create cluster manager with `GoogleMapsView` as the constructor parameter:

```csharp
void AddMarkerCluster()
{
    var clusterManager = new ClusterManager(_map);
    var myClusterItems = new List<ClusterItem>
    {
        new ClusterItem(new LatLng(0, 0), "Title", "Snippet"),
        new ClusterItem(new LatLng(1, 1), "Title", "Snippet")
    };
    _clusterManager.AddItems(myClusterItems);
}
```

## Clearing items

You can clear all items from the cluster by calling `clusterManager.ClearItems();`

Please see the [demo on Google Play Store](https://play.google.com/store/apps/details?id=com.deadmosquitogames.gmaps.demo) and look at London area to see clustering in action.


# **Drawing Heatmaps**

This implements the heatmaps functionality from Android iOS and Android utility libraries, for more background information please check out the official Google documentation:

* **Android**: https://developers.google.com/maps/documentation/android-api/utility/heatmap
* **iOS**: https://developers.google.com/maps/documentation/ios-sdk/utility/heatmap

![](/images/maps/heatmaps.jpg ':size=768')

## Adding heatmap with default settings

To add a heatmap with default settings you can use a convenience method, where you only need to provide a set of locations:

```csharp
var data = new List<LatLng>;
// add your locations here
var heatmap = _map.AddHeatmapWithDefaultLook(data);
```

## Adding heatmap with fine-grained control

You can also customize a lot of things if you need:

```csharp
var heatmapData = DemoUtils.DeserializeLocations(heatmapDataJsonMedicare.text);

var heatmapGradient = new HeatmapGradient(new Color[] {
    new Color32(255, 0, 255, 0),// transparent
    new Color32(255, 0, 255, 255 / 3 * 2),
    new Color32(0, 0, 191, 255),
    new Color32(0, 0, 0, 127),
    new Color32(0, 255, 0, 0)
}, new[] {
    0.0f, 0.10f, 0.20f, 0.60f, 1.0f
}, 1000);
TileProvider tileProvider = new HeatmapTileProvider.Builder()
    .Radius(30)
    .Gradient(heatmapGradient)
    .Data(heatmapData)
    .Build();
var medicareHeatmapOptions = new TileOverlayOptions()
    .FadeIn(true)
    .Transparency(0.0f)
    .ZIndex(1)
    .Visible(true)
    .TileProvider(tileProvider);
_map.AddTileOverlay(medicareHeatmapOptions);
```

# **Map Events and Listeners**

You can register listeners for certain map events. Currently supported events (the names should be self-explaining):

+ Map click (`SetOnMapClickListener()`)
+ Map long click (`SetOnLongMapClickListener()`)
+ Marker click (`SetOnMarkerClickListener()`)
+ Marker drag (`SetOnMarkerDragListener()`)
+ Marker info window click (`SetOnInfoWindowClickListener()`)
+ Circle click (`SetOnCircleClickListener()`)
+ Polyline click (`SetOnPolylineClickListener()`)
+ Polygon click (`SetOnPolygonClickListener()`)
+ Ground overlay click (`SetOnGroundOverlayClickListener()`)
+ Map camera events (`SetOnCameraMoveStartedListener()`, `SetOnCameraIdleListener()`)

## Map Click Listener

You can respond to click events when the user clicks the map. To do so set the listener on the `GoogleMapsView` object. You will be passed the location of where the user clicked in the callback.

Example:

```csharp
_map.SetOnMapClickListener(point =>
{
    Debug.Log("Map clicked: " + point);
});
```

## Map Long Click Listener

You can respond to click events when user long clicks the map. To do so set the listener on the `GoogleMapsView` object. You will be passed the location of where user long clicked in the callback.

Example:

```csharp
_map.SetOnLongMapClickListener(point =>
{
    Debug.Log("Map long clicked: " + point);
});
```

## Marker click

You can respond to click events when user long clicks the marker. To do so set the listener on the `GoogleMapsView` object. You will be passed the marker that was clicked in the callback.

Example:

You can respond to click events when the user clicks the marker info popup window.

```csharp
var defaultClickBeahaviour = false;
_map.SetOnMarkerClickListener(marker => Debug.Log("Marker clicked: " + marker), defaultClickBehaviour);
```

## Marker info window click

Example:

```csharp
_map.SetOnInfoWindowClickListener(marker => Debug.Log("Marker info window clicked: " + marker));
```

## Circle click

You can respond to click events when user long clicks the circle. To do so set the listener on the `GoogleMapsView` object. You will be passed the circle that was clicked in the callback.

Example:

```csharp
_map.SetOnCircleClickListener(circle => Debug.Log("Circle clicked: " + circle));
```

## Camera movement

You can register listener to receive an even when the camera started moving receiving the reason of this movement into a callback, this can be because of `ApiAnimation`, `DeveloperAnimation` or `Gesture`

```csharp
_map.SetOnCameraMoveStartedListener(moveReason => Debug.Log("Camera move started because: " + moveReason));
```

# **Showing User Location**

You can show users location on the map view.

![](/images/maps/my_location.png ':size=768')

## Setup

1. Add **ACCESS_FINE_LOCATION** permission to your `AndroidManifest.xml` file under under `Plugins/Android` folder. See [Android Permissions](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) for more details.

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

NOTE: Since Android 6.0 (API level 23) dangerous permissions are required to be requested at runtime. Although Unity will request the location permission at the game start this might be not something you want. The plugin does not include functionality for this. I recommend using [Android Native Goodies Pro](https://assetstore.unity.com/packages/tools/integration/android-native-goodies-66662) for managing runtime permissions.

2. Call `_map.IsMyLocationEnabled = true;` on your map object after the map has finished loading. If you did not provide the permission in the app manifest the app will crash.

3. Call `_map.UiSettings.IsMyLocationButtonEnabled = true;` on the map object if you want "My Location Button" to be displayed.

## Getting GPS location

To get current GPS location use `_map.Location` property.

# **Changing UI Settings**

You can also modify the maps UI settings after the map has been shown.

Example:

```csharp
public void OnTestUiSettingsButtonClick(bool enable)
{
    EnableAllSettings(_map.UiSettings, enable);
}

static void EnableAllSettings(UiSettings settings, bool enable)
{
    Debug.Log("Current Ui Settings: " + settings);

    // Buttons/other
    settings.IsCompassEnabled = enable;
    settings.IsIndoorLevelPickerEnabled = enable;
    settings.IsMapToolbarEnabled = enable;
    settings.IsMyLocationButtonEnabled = enable;
    settings.IsZoomControlsEnabled = enable;

    // Gestures
    settings.IsRotateGesturesEnabled = enable;
    settings.IsScrollGesturesEnabled = enable;
    settings.IsTiltGesturesEnabled = enable;
    settings.IsZoomGesturesEnabled = enable;
    settings.SetAllGesturesEnabled(enable);
}
```

# **Animating/Moving Camera**

You can animate the camera to show a particular place, move or zoom etc. To animate camera create `CameraUpdate` and pass it to `GoogleMapsView` method `AnimateCamera()` as parameter. Check the demo in the package for examples. See the example code for reference below.

## Example

See the examples below for different ways to animate camera

```csharp
public void AnimateCameraNewCameraPosition()
{
    _map.AnimateCamera(CameraUpdate.NewCameraPosition(CameraPosition));
}

public void AnimateCameraNewLatLng()
{
    _map.AnimateCamera(CameraUpdate.NewLatLng(new LatLng(camPosLat.value, camPosLng.value)));
}

public void AnimateCameraNewLatLngBounds1()
{
    _map.AnimateCamera(CameraUpdate.NewLatLngBounds(Bounds, 10));
}

public void AnimateCameraNewLatLngBounds2()
{
    _map.AnimateCamera(CameraUpdate.NewLatLngBounds(Bounds, 100, 100, 10));
}

public void AnimateCameraNewLatLngZoom()
{
    const int zoom = 10;
    _map.AnimateCamera(CameraUpdate.NewLatLngZoom(new LatLng(camPosLat.value, camPosLng.value), zoom));
}

public void AnimateCameraScrollBy()
{
    const int xPixel = 250;
    const int yPixel = 250;
    _map.AnimateCamera(CameraUpdate.ScrollBy(xPixel, yPixel));
}

public void AnimateCameraZoomByWithFixedLocation()
{
    const int amount = 5;
    const int x = 100;
    const int y = 100;
    _map.AnimateCamera(CameraUpdate.ZoomBy(amount, x, y));
}

public void AnimateCameraZoomByAmountOnly()
{
    const int amount = 5;
    _map.AnimateCamera(CameraUpdate.ZoomBy(amount));
}

public void AnimateCameraZoomIn()
{
    _map.AnimateCamera(CameraUpdate.ZoomIn());
}

public void AnimateCameraZoomOut()
{
    _map.AnimateCamera(CameraUpdate.ZoomOut());
}

public void AnimateCameraZoomTo()
{
    const int zoom = 10;
    _map.AnimateCamera(CameraUpdate.ZoomTo(zoom));
}
```


# **JSON Map Styling**

The plugin also supports styling the map with provided style json.

To create custom style please use [Google Map Styling Wizard](https://mapstyle.withgoogle.com/)

![styling](/images/maps/style_json.png ':size=768')

You will download the json file which represents your map style. Now to set the style just read your json to string and pass to `SetMapStyle` method

```csharp
var isSyleUpdateSuccess = _map.SetMapStyle(customStyleJsonAsString);
if (isSyleUpdateSuccess)
{
    Debug.Log("Successfully updated style of the map");
}
else
{
    Debug.LogError("Setting new map style failed.");
}
```

![styling](/images/maps/style.png ':size=768')


# **Taking Map Snapshot**

You can take a snapshot of the map view and receive a result as `Texture2D`. It useful if want to animate a map view or display some content on top of the map.

To take a snapshot use `GoogleMapsView.TakeSnapshot` method:

```csharp
_map.TakeSnapshot(texture =>
{
    Debug.Log("Snapshot captured: " + texture.width + " x " + texture.height);
    snapshotImage.GetComponent<Image>().sprite = Sprite.Create(texture, new Rect(0, 0, texture.width, texture.height), Vector2.zero);;
});
```

# **BONUS! Opening Google Maps app**

Using `OpenGoogleMapHelper.cs` class you can open Google Maps application on the device (location, directions, search, street view). The call will fallback to browser if Google Maps app is not installed. Also works on Desktop platforms. Check `OpenGoogleMapsHelperDemo.cs` script for more examples

## Displaying the map

Example:

```csharp
public void OnOpenMapDefault()
{
    OpenGoogleMapHelper.DisplayMapDefault();
}

public void OnOpenMapCustom()
{
    OpenGoogleMapHelper.DisplayMap(new LatLng(-33.8569, 151.2152), 10, OpenGoogleMapHelper.MapType.Satellite, OpenGoogleMapHelper.MapLayer.Bicycling);
}
```

## Opening search

Launch a Google Map that displays a pin for a specific place, or perform a general search and launch a map to display the results

Example:

```csharp
// search by name
OpenGoogleMapHelper.OpenSearch("CenturyLink Field");

// by coordinates and place id
OpenGoogleMapHelper.OpenSearch("47.5951518,-122.3316393", "ChIJKxjxuaNqkFQR3CK6O1HNNqY");

// by coordicates
OpenGoogleMapHelper.OpenSearch("47.5951518,-122.3316393");

// by generic query
OpenGoogleMapHelper.OpenSearch("pizza seattle wa");
```

## Getting directions

Examples:

```csharp
public void OnDirectionsSearchByPlaceNames()
{
    var directionParams = new DirectionsParams();
    directionParams.SetOrigin("Space Needle Seattle");
    directionParams.SetDestination("Pike Place Marker Seattle");
    directionParams.SetTravelMode(DirectionsParams.TravelModeType.Bicycling);

    OpenGoogleMapHelper.OpenDirections(directionParams);
}

public void OnDirectionsSearchByPlaceIds()
{
    var directionParams = new DirectionsParams();
    directionParams.SetOrigin("Google Pyrmont NSW");
    directionParams.SetDestination("QVB");
    directionParams.SetDestinationPlaceId("ChIJISz8NjyuEmsRFTQ9Iw7Ear8");
    directionParams.SetTravelMode(DirectionsParams.TravelModeType.Transit);

    OpenGoogleMapHelper.OpenDirections(directionParams);
}

public void OnDirectionsSearchByCoordinates()
{
    var directionParams = new DirectionsParams();
    directionParams.SetOrigin("47.5951518,-122.3316393");
    directionParams.SetDestination("47.5989513,-122.3374653");
    directionParams.SetTravelMode(DirectionsParams.TravelModeType.Driving);

    OpenGoogleMapHelper.OpenDirections(directionParams);
}

public void OnDirectionsSearchWithWaypoints()
{
    var directionParams = new DirectionsParams();
    directionParams.SetOrigin("CenturyLink Field");
    directionParams.SetDestination("City Hall Park Seattle");
    directionParams.SetTravelMode(DirectionsParams.TravelModeType.Walking);

    var waypoints = new List<string>
    {
        "Klondike Gold Rush National Historical Park Seattle",
        "Smith Tower",
        "Seattle Passport Agency"
    };

    directionParams.SetWaypoints(waypoints);

    OpenGoogleMapHelper.OpenDirections(directionParams);
}

public void OnDirectionsSearchWithWaypointIds()
{
    var directionParams = new DirectionsParams();
    directionParams.SetOrigin("CenturyLink Field");
    directionParams.SetDestination("City Hall Park Seattle");
    directionParams.SetTravelMode(DirectionsParams.TravelModeType.Walking);

    var waypoints = new List<string>
    {
        "Klondike Gold Rush National Historical Park Seattle",
        "Smith Tower",
        "Seattle Passport Agency"
    };

    var waypointIds = new List<string>
    {
        "ChIJBbqpNLtqkFQRyLNYioGRcF8",
        "ChIJ6x3yk7pqkFQRW2zXQJUlScA",
        "ChIJvZiGw-V3CEER2qnjz1yUdW4"
    };

    directionParams.SetWaypoints(waypoints);
    directionParams.SetWaypointPlaceIds(waypointIds);

    OpenGoogleMapHelper.OpenDirections(directionParams);
}
```

## Opening street view

The pano action lets you launch a viewer to display Street View images as interactive panoramas. Each Street View panorama provides a full 360-degree view from a single location.

Example:

```csharp
public void OnDisplayStreetViewPanaroma()
{
    OpenGoogleMapHelper.DisplayStreetViewPanorama("tu510ie_z4ptBZYo2BGEJg", new LatLng(48.8578261, 2.2952414), -45, 38, 80);
}
```

***

# **FAQ**

## Can I draw Unity elements on the map?

- **Q:** Can I draw Unity elements on the map such as UI or game objects?
- **A:** No, this is mentioned in the limitations, since this is a native Android and iOS view that is not possible

## Do I need a Google account?

- **Q:** Do I need a Google account to use this plugin?
- **A:** Yes, you need a Google account to obtain the API key

## Do I need to pay Google?

- **Q:** Do I need to pay Google for API usage?
- **A:** No, the Maps SDK usage is [free on Android and iOS](https://cloud.google.com/maps-platform/pricing). Plugin uses Android and iOS SDKs so you can check the pricing by following the link

## Map not showing up

- **Q:** I am getting a white blank canvas with the Google logo on the bottom left, what's happening?
- **A:** This means you didn't setup your API key correctly. Please follow [Setup Instructions for Android](#setup-android) or [Setup Instructions for iOS](#setup-ios) carefully.

## Can't build the project or my app crashes

- **Q:** I am getting errors when trying to build the project or crashes at runtime, what should I do?
- **A:** Contact us on [Discord server](https://bit.ly/nineva_support_discord) but collect and provide all the useful info that can help us troubleshoot the issue, such as: Logcat logs on Androud, XCode logs from iOS, the code that you use, etc.

# **Troubleshooting**

This page explains how to troubleshoot problems when you can't build your Android project.

## How to find the actual error why I can't build the project

When Unity can't build Android project it shows the error with the actual build error in Unity console. The problem is that the error log is very big and you have to scroll and look at it carefully to find the actual cause of build failure.

![](/images/maps/build_error.png ':size=768')

If you can't figure out what's wrong please send me the **FULL** log to check. **When you are sending me the log please make sure you copy the whole text.**

## Dealing with Google Play Services Dependencices/Versions

When you use my plugins along with other plugins that contain [Google Play Services](https://developers.google.com/android/guides/setup) there are two things you should take into account:

1. Version difference
2. Dependencies difference

What you basically need to achieve when trying to use multiple plugins that user Play Service in the same project is to have all of your Google Play Services libraries be:

1. Of the same version
2. Without duplications
3. Having all the libraries for your project needs

### Using the plugin along with Firebase

Firebase for Unity and a lot of other projects use [Play Services Resolver for Unity](https://github.com/googlesamples/unity-jar-resolver#android-resolver) project to manage its dependencies. Please read how it works to understand how dependencies are resolved.

### Step 1

**Remove all Google Play Services AAR files that come along with my package (everything that has a version in aar file name), you won't need them as resolver will automatically resolve them later!**

![](/images/maps/remove_libs_1.png ':size=768')

![alt text](/images/maps/remove_libs_1.png ':size=768')

### Step 2

Now you need to find the XML file with dependencies, usually it is `AppDependencies.xml`. It looks something like this (this particular one is from FirebaseAnalytics.unitypackage):

```xml
<!-- Copyright (C) 2017 Google Inc. All Rights Reserved.

FirebaseApp iOS and Android Dependencies.
-->

<dependencies>
  <iosPods>
    <iosPod name="Firebase/Core" version="4.3.0" minTargetSdk="7.0)
    </iosPod>
  </iosPods>
  <androidPackages>
    <androidPackage spec="com.google.android.gms:play-services-base:11.4.2)
      <androidSdkPackageIds>
        <androidSdkPackageId>extra-google-m2repository</androidSdkPackageId>
      </androidSdkPackageIds>
    </androidPackage>
    <androidPackage spec="com.google.firebase:firebase-common:11.4.2)
      <androidSdkPackageIds>
        <androidSdkPackageId>extra-google-m2repository</androidSdkPackageId>
        <androidSdkPackageId>extra-android-m2repository</androidSdkPackageId>
      </androidSdkPackageIds>
    </androidPackage>
    <androidPackage spec="com.google.firebase:firebase-core:11.4.2)
      <androidSdkPackageIds>
        <androidSdkPackageId>extra-google-m2repository</androidSdkPackageId>
        <androidSdkPackageId>extra-android-m2repository</androidSdkPackageId>
      </androidSdkPackageIds>
    </androidPackage>
    <androidPackage spec="com.google.firebase:firebase-app-unity:4.2.1)
      <repositories>
        <repository>Assets/Firebase/m2repository</repository>
      </repositories>
    </androidPackage>
  </androidPackages>
</dependencies>
```

Now find the `androidPackages` tag and see what version of play services your Firebase version uses, after that replace `com.google.android.gms:play-services-base` dependency for `com.google.android.gms:play-services-maps` or `com.google.android.gms:play-services-places` (depending what you use) with **THE SAME** version as the one Firebase uses in your project:

Example for Places API (don't forget to change the version):
```xml
<!-- Copyright (C) 2017 Google Inc. All Rights Reserved.

FirebaseApp iOS and Android Dependencies.
-->

<dependencies>
  <iosPods>
    <iosPod name="Firebase/Core" version="4.3.0" minTargetSdk="7.0)
    </iosPod>
  </iosPods>
  <androidPackages>
    <androidPackage spec="com.google.android.gms:play-services-places:11.4.2)
      <androidSdkPackageIds>
        <androidSdkPackageId>extra-google-m2repository</androidSdkPackageId>
      </androidSdkPackageIds>
    </androidPackage>
    <androidPackage spec="com.google.firebase:firebase-common:11.4.2)
      <androidSdkPackageIds>
        <androidSdkPackageId>extra-google-m2repository</androidSdkPackageId>
        <androidSdkPackageId>extra-android-m2repository</androidSdkPackageId>
      </androidSdkPackageIds>
    </androidPackage>
    <androidPackage spec="com.google.firebase:firebase-core:11.4.2)
      <androidSdkPackageIds>
        <androidSdkPackageId>extra-google-m2repository</androidSdkPackageId>
        <androidSdkPackageId>extra-android-m2repository</androidSdkPackageId>
      </androidSdkPackageIds>
    </androidPackage>
    <androidPackage spec="com.google.firebase:firebase-app-unity:4.2.1)
      <repositories>
        <repository>Assets/Firebase/m2repository</repository>
      </repositories>
    </androidPackage>
  </androidPackages>
</dependencies>
```

Example for Maps API (don't forget to change the version):
```xml
<!-- Copyright (C) 2017 Google Inc. All Rights Reserved.

FirebaseApp iOS and Android Dependencies.
-->

<dependencies>
  <iosPods>
    <iosPod name="Firebase/Core" version="4.3.0" minTargetSdk="7.0)
    </iosPod>
  </iosPods>
  <androidPackages>
    <androidPackage spec="com.google.android.gms:play-services-maps:11.4.2)
      <androidSdkPackageIds>
        <androidSdkPackageId>extra-google-m2repository</androidSdkPackageId>
      </androidSdkPackageIds>
    </androidPackage>
    <androidPackage spec="com.google.firebase:firebase-common:11.4.2)
      <androidSdkPackageIds>
        <androidSdkPackageId>extra-google-m2repository</androidSdkPackageId>
        <androidSdkPackageId>extra-android-m2repository</androidSdkPackageId>
      </androidSdkPackageIds>
    </androidPackage>
    <androidPackage spec="com.google.firebase:firebase-core:11.4.2)
      <androidSdkPackageIds>
        <androidSdkPackageId>extra-google-m2repository</androidSdkPackageId>
        <androidSdkPackageId>extra-android-m2repository</androidSdkPackageId>
      </androidSdkPackageIds>
    </androidPackage>
    <androidPackage spec="com.google.firebase:firebase-app-unity:4.2.1)
      <repositories>
        <repository>Assets/Firebase/m2repository</repository>
      </repositories>
    </androidPackage>
  </androidPackages>
</dependencies>
```

Shortly:

![](/images/maps/firebase_conflicts.png ':size=768')

Now the dependency resolver must resolve all the dependencies automatically.
