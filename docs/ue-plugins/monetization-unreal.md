[filename](common/common_ue_header.md ':include')
[filename](common/copy_plugin.md ':include')

# **Monetization Goodies**

Welcome to Monetization Goodies Documentation for Unreal Engine.

This plugin wraps native payments for Android and iOS. On Android it wraps the [Billing library](https://developer.android.com/google/play/billing/integrate), and on iOS - [Original In-App-Purchase API](https://developer.apple.com/documentation/storekit/original_api_for_in-app_purchase?language=objc).

?> The plugin functionality wraps around native calls and should be used with the same recommendations and flows, as the original SDKs.

Due to the native API's being different, the implementations and functions are different for each platform, meaning that you will have to write your own implementation for each platform separately.

# Setup

The plugin contains demo levels for both Android and iOS in the `Content` folder.

Disable `Online Subsystem Google Play` in project Plugins window.

![](images/monetization/DisableGooglePlaySubsystem.jpg)

You also need to remove the dependency by removing these lines if you have it in your `Target.cs` and `Build.cs` files:

* `PrivateDependencyModuleNames.Add("OnlineSubsystemGooglePlay");`
* `ExtraModuleNames.Add("OnlineSubsystemGooglePlay");`

Due to low XCode version on Marketplace packaging servers, we had to add a workaround to get the plugin published. If you plan to use it for iOS, you have to copy the plugin from the engine directory to the `[Project]/Plugins/` directory.

Go to the Project Settings -> Monetization Goodies and switch the Enable In App Purchases toggle.

![](images/monetization/monetization-0.png)

After that close the Editor, go to the `[Project]/Plugins/MonetizationGoodies` directory and delete the `Binaries` and `Intermediate` folders. This will force the plugin to be rebuilt and include the required dependencies. 

![](images/monetization/monetization-1.png)

## Android

?> Please, follow the recommendations from the [official documentation](https://developer.android.com/google/play/billing/integrate) for most use cases and best practices.

### Initialize Client

Android Billing Client class is the main entry point for communication with the billing library. It is recommended to create an instance of it as early as possible, and to keep a reference to it for future calls.

![](images/monetization/monetization-2.png)

While doing so, you also provide the main purchase callback handler delegate. It gets invoked every time a purchase transaction is added, updated or removed. It is a right place to handle the transactions depending on their status.

?> Billing library requires every successful purchase to be acknowledged before consuming it or granting content to the user.

You can also check whether some features are supported on the device, and act accordingly:

![](images/monetization/monetization-4.png)

Once you have created the billing client, you can call the start connection function to start the actual communication with the Play Store. You also provide delegate function to handle the result of the connection, as well a function to be invoked when the connection is disrupted. It is recommended to write your reconnect logic for such cases.

![](images/monetization/monetization-5.png)

You can also check the connection state at any time.

![](images/monetization/monetization-6.png)

It is recommended to query pending purchases after the connection is successfully established.

![](images/monetization/monetization-7.png)

### Make purchase

You have to obtain SKU details before launching the purchase flow. Use the `Query SKU Details` function for that.

![](images/monetization/monetization-8.png)

You can either fetch the SKU details for the required item before the purchase, or fetch the SKU details for all the required items beforehand and store them for future use.

Once you have the required SKU details, you can call the `Launch Billing Flow` function and providing a valid `BillingFlowParameters` object.

![](images/monetization/monetization-9.png)

The result of the purchase will be received in the `OnPurchasesUpdated` callback, provided during Billing Client creation.

?> Billing library requires every successful purchase to be acknowledged before consuming it or granting content to the user.

You can also check if the purchase is already acknowledged:

![](images/monetization/monetization-3.png)

### Launch Price Confirmation Flow

If a user has an active subscription the price of which has recently changed, it is recommended to let them know and to confirm their acceptance of the new price. Use the following call providing the SKU details of the item with new price.

![](images/monetization/monetization-10.png)

### Query data

You can query the purchase history for Items or Subscriptions at any time.

![](images/monetization/monetization-11.png)

### Stop connection

It is recommended to stop connection after you are done working with the Billing Client to free system resources.

![](images/monetization/monetization-12.png)

## iOS

?> Please, follow the recommendations from the [official documentation](https://developer.apple.com/documentation/storekit/original_api_for_in-app_purchase?language=objc) for most use cases and best practices.

Make sure to follow the setup instructions at the beginning of this page before implementing In-App-Purchases for iOS.

### Initialize

`PaymentQueue` is main entry point for communicationg with App Store.

You should get and store a reference to the default queue as soon as possible to initialize callbacks.
It is also important to check if the user is authorized to make purchases.

![](images/monetization/monetization-13.png)

Once you get a reference to the queue object, you can add transaction observer and bind delegate functions to handle the native callbacks.

![](images/monetization/monetization-14.png)

!> Due to the threading issues (delegate functions have to be executed in the main game thread), some delegates can not be overriden in blueprints, and you have to write the logic for them in the code.

These are two methods in the `MGIosPaymentQueue.cpp` file:

![](images/monetization/monetization-15.png)

And one method in the `MGIosTransactionObserver.cpp` file.

![](images/monetization/monetization-16.png)

### Purchase items

Before making purchases you have to query and store the available product's data.

![](images/monetization/monetization-17.png)

After that you can use the product details objects to request the purchases.

![](images/monetization/monetization-18.png)

### Other functionality

Restore purchases:

![](images/monetization/monetization-19.png)

Present code redemption sheet:

![](images/monetization/monetization-20.png)

The plugin wraps all of the native [Original In-App-Purchase API](https://developer.apple.com/documentation/storekit/original_api_for_in-app_purchase?language=objc), so you can view most of the use cases and references in the official documentation.

# Changelog

1.0.0

* Initial release

---
