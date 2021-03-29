# **Mobile Google Places Goodies**

Helping you to use native Android and iOS Google Places API, mainly the places autocomplete screen.

To get the idea what this plugin is capable of please read [the SDK overview](https://developers.google.com/places/android-sdk/overview)

* **Join our [Discord server](https://bit.ly/nineva_support_discord) and ask us anything!**

# **Setup (Android)**

## Obtaining API key

Please follow these instructions to obtain your API key:
1. [Setting up in the Google Cloud Console](https://developers.google.com/places/android-sdk/cloud-setup)
2. [Get an API Key](https://developers.google.com/places/android-sdk/get-api-key)

## Paste your newly obtained API key into Unity project

Go to `Window -> Mobile Places SDK -> Edit Settings` and place your API key there (into Android section). 

![](/images/places/api_key_settings.png)

## Change your package name

Go to `Player Settings` for Android and change your package name to the one you have restricted the API key to

# **Setup (iOS)**

## Obtaining API key

Please follow these instructions to obtain your API key:
1. [Setting up in the Google Cloud Console](https://developers.google.com/places/ios-sdk/cloud-setup)
2. [Get an API Key](https://developers.google.com/places/ios-sdk/get-api-key)

## Paste your newly obtained API key into Unity project

Go to `Window -> Mobile Places SDK -> Edit Settings` and place your API key there (into iOS section). 

## Change your bundle identifier

Go to `Player Settings` for Android and change your bundle identifier to the one you have restricted the API key to

# Run the demo

Build and run on the device the example scene from the `Example` project. You can now test your implementation.

# Functionality

## Initialize the plugin

Call the `Places.Init()` method to properly initialize the plugin. It should be called before any other plugin calls. 

## Place autocomplete screen

You can open the native autocomplete screen for the user to be able to pick a place using the specified criteria.
To do so, call the `Places.ShowPlaceAutocomplete()` method, providing the autocomplete options and three callbacks (for successful pick, for error handling, and for user cancellation handling).

Use the `AutocompleteOptions.Builder` object to initialize the autocomplete parameters for the autocomplete screen. This object allows you to customize the following parameters: whether to show the native view in fullscreen or not (works on Android only), the types of fields you wish to obtain when the user successfully picks a Place, the country/-ies to restrict the results to (using the ISO 3166-1 Alpha-2 country codes), the hint text to display in the search input field when there is no text entered (works on Android only), the initial query in the search input (works on Android only), the place type filter (`address`, `cities`, `establishment`, `geocode`, or `regions`), as well as location bias (preferred location of the results) ***or*** location restriction (strict location limitations for the results).

``` csharp
var europeBounds = new Place.LatLngBounds(new Place.LatLng(40, -10), new Place.LatLng(70, 40));
var options = new AutocompleteOptions.Builder( /* show a full screen mode activity */ AutocompleteMode.Fullscreen, new List<Place.Field> {Place.Field.Id})
    .SetTypeFilter(TypeFilter.Cities)
    .SetLocationBias(europeBounds)
    .Build();
Places.ShowPlaceAutocomplete(options, HandlePlace, ShowText, () => { ShowText("Autocomplete was cancelled.");

...

void HandlePlace(Place place)
{
    //TODO: handle place data here.
}

void ShowText(string message)
{
    debugText.text = message;
}
```
## Find current place

You can check the probabilities of the device being located in the nearby places using the `Places.FindCurrentPlace()` method, providing the fields you wish to fetch for the likely places, as well as the success callback to handle the list of place likelihood objects, and the error handling callback.

This method requires the location permission to be granted by the user. You can request it by calling the `PlacesUtils.RequestPermission()` on Android. On iOS the `Places.FindCurrentPlace` method requests the location permission automatically, if it is not yet granted by the user.

``` csharp
public void OnFindPlaceClicked()
{
    PlacesUtils.RequestPermission(PlacesUtils.LocationPermission, (permission, granted) =>
    {
        if (permission != PlacesUtils.LocationPermission)
        {
            ShowText("Wrong permission.");
            return;
        }

        if (granted)
        {
            Places.FindCurrentPlace(new List<Place.Field> {Place.Field.Id, Place.Field.Name, Place.Field.LatLng}, HandlePlaceLikelihoods, print);
        }
        else
        {
            ShowText("Permission was not granted by the user.");
        }
    });
}

void HandlePlaceLikelihoods(List<PlaceLikelihood> likelihoods)
{
    likelihoods = likelihoods.OrderByDescending(likelihood => likelihood.likelihood).ToList();
    var mostLikelyPlace = likelihoods[0];
    ShowText($"Most likely place: {mostLikelyPlace.place.name} ({mostLikelyPlace.likelihood * 100f}%). Place data: {mostLikelyPlace.place}");
}

void ShowText(string message)
{
    debugText.text = message;
}
```

## Fetch place

Call `Places.FetchPlace()` to get the place data using the place ID, and providing the fields you wish to fetch for the place, as well as the success callback to handle place data, and the error handling callback.

```csharp
public void OnGetPlaceClicked()
{
    if (string.IsNullOrEmpty(placeIdInput.text))
    {
        ShowText("Place ID is empty. Please, fill it before calling GetPlace.");
        return;
    }

    var allFields = Enum.GetValues(typeof(Place.Field)).OfType<Place.Field>().ToList();
    Places.FetchPlace(placeIdInput.text, allFields, HandlePlace,
        ShowText);
}

void HandlePlace(Place place)
{
    //TODO: handle place data here.
}

void ShowText(string message)
{
    debugText.text = message;
}
```

## Get place photos

This is a two-step process. First, you should call the `Places.GetPlacePhotos()` method, providing the place ID, as well as the success callback to handle place photo metadatas, and the error handling callback.

```csharp
PlacePhotoMetadata _selectedMetadata;

public void OnGetPlacePhotosClicked()
{
    if (string.IsNullOrEmpty(placeIdInput.text))
    {
        ShowText("Place ID is empty. Please, fill it before calling GetPlacePhotos.");
        return;
    }

    Places.GetPlacePhotos(placeIdInput.text, OnGetPlacePhotosSuccess, OnGetPlacePhotosError);
}

void OnGetPlacePhotosSuccess(List<PlacePhotoMetadata> metadatas)
{
    if (metadatas.Count < 1)
    {
        OnGetPlacePhotosError("Photo metadata list is empty.");
        return;
    }

    _selectedMetadata = metadatas.First();
    ShowText($"Selected photo metadata. Width: {_selectedMetadata.Width}, height: {_selectedMetadata.Height}, attributions: {_selectedMetadata.Attributions}.");
}

void OnGetPlacePhotosError(string error)
{
    ShowText(error);
}

void ShowText(string message)
{
    debugText.text = message;
}
```

Then you can use the metadata object as a parameter to the `Places.FetchPhoto()` method, as well as the success callback to handle place photo as `Texture2D` object, and the error handling callback.

```csharp
public void OnFetchPhotoClicked()
{
    Places.FetchPhoto(_selectedMetadata, OnPhotoReceived, OnPhotoFetchError);
}

void OnPhotoReceived(Texture2D photo)
{
    // TODO: handle the texture here.
}

void OnPhotoFetchError(string error)
{
    ShowText(error);
}

void ShowText(string message)
{
    debugText.text = message;
}
```
