# GIPHY Native Gif picker

This plugin allows you to use native GIPHY Android and iOS SDKs in your Unity app.

More about GIPHY SDK:
* https://developers.giphy.com
* https://developers.giphy.com/docs/sdk#ios
* https://developers.giphy.com/docs/sdk#android

# Setup

## Getting the API key

Go to https://developers.giphy.com/docs/sdk, create your account and click `Create an App` button. Follow the instruction until you get your API key created.

![](/images/giphy/api_key_1.png ':size=1024')
![](/images/giphy/api_key_2.png ':size=1024')
![](/images/giphy/api_key_3.png ':size=1024')
![](/images/giphy/api_key_4.png ':size=1024')

Open your Unity project and go to `Window -> Giphy SDK -> Edit Settings` and paste your API key there.

![](/images/giphy/api_key_unity.png)

When you are ready to go to production with your app, you must upgrade your API key in the developer portal.

![](/images/giphy/api_key_upgrade_prod.png ':size=1024')

# Functionality and Features

## Initialize the plugin

Call `Giphy.Init()` to initialize the SDK and providing whether to enter the verification mode. You will need to do this at least once to upgrade your API key to the production one. In that case you will see the verification code in the device logs.

## Pick GIF

The plugin allows to open native OS view with preview GIFs using the `Giphy.PickGif()` call.

You can customize the appearance of the view, as well as the Media search preferences by providing an instance of `GiphyPickSettings`. Please, note that some customizations are only available on Android devices.

You should also specify the respective callbacks to handle the picked media file, and to handle the user search input, as well as a callback to handle the cases when the user closes the picker.

```csharp
public void OnPickGifButtonClicked()
{
    var settings = new GiphyPickSettings
    {
        Rating = GiphyRating.Unrated,
        Theme = GiphyGridTheme.Dark,
        GiphyGridType = GiphyGridType.Carousel,
        ImageFormat = GiphyImageFormat.GIF,
        RenditionType = GiphyRenditionType.FixedWidthSmall,
        ShowAttribution = false,
        ConfirmationRenditionType = GiphyRenditionType.Original,
        EnableDynamicText = true,
        EnablePartnerProfiles = false,
        MediaTypeConfig = new List<GiphyContentType> {GiphyContentType.Gif},
        SelectedContentType = GiphyContentType.Gif,
        ShowCheckeredBackground = false,
        ShowConfirmationScreen = true,
        ShowSuggestionsBar = true,
        StickerColumnCount = 2,
        UseBlurredBackground = false,
        SuggestionsBarFixedPosition = false
    };

    Giphy.PickGif(settings, OnPickSuccess, OnTextSearched, OnViewDismissed);
}

void OnViewDismissed()
{
    print("Picker view was dismissed.");
}

void OnTextSearched(string text)
{
    print("Text " + text + " was searched.");
}

void OnPickSuccess(GiphyMedia media, GiphyContentType type)
{
    //TODO: handle the media data here.
}
```

After the media is picked, you have access to the information about the original resource (`GiphyMedia` object).

If you wish to get the download URL's and other data for the files, you should call the `GiphyMedia.GetMediaImage()` providing the [rendition type](https://developers.giphy.com/docs/optional-settings#rendition-guide) to obtain the `GiphyMediaImage` object that provides download URL's and other useful metadata for the MP4, GIF and WEBP files representing the selected media.

![](/images/giphy/Giphy0.jpeg)
![](/images/giphy/Giphy1.jpeg)
![](/images/giphy/Giphy2.jpeg)
![](/images/giphy/Giphy3.jpeg)

## Media handling

Unity does not support the GIF playback, but there are numerous solutions. We have tested [UniGif](https://github.com/WestHillApps/UniGif) package to play GIF files in the demo.

Unity's built-in [Video Player](https://docs.unity3d.com/ScriptReference/Video.VideoPlayer.html) handles the playback of MP4 files.

