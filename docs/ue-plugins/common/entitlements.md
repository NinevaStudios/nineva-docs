===

Here is a way to add entitlements to the project with modernized XCode workflow suggested by one of our customers:

It appears that Modern Xcode currently relies solely on premade entitlement files, for which the options are only exposed for Mac in the Project Settings). The settings however exist for iOS under the hood and can be leveraged by adding:

```ini
[/Script/MacTargetPlatform.XcodeProjectSettings]
PremadeIOSEntitlements=(FilePath="/Game/PremadeEntitlements/PremadeIOS.entitlements")
ShippingSpecificIOSEntitlements=(FilePath="/Game/PremadeEntitlements/ShippingSpecificIOS.entitlements")
```

To the project's `DefaultEngine.ini`. These entitlements would need to be authored outside of UE or copying the from Legacy Xcode's output. This solution isn't great as it won't update as the dependent flags that used to drive entitlement file generation, however at least it would ensure that the setup need only be done once and subsequent project file generation wouldn't overwrite the entitlement settings.

===