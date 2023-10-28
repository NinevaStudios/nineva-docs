# FAQ

## Is the source included in the plugin?

Yes, all the source code is inside the plugin and you can modify it as you see fit.

---

## How can I find out the versions of the native libs that are used in your plugins?

- Android - check the Android `.UPL` file in the plugin source code, all the dependencies are lister there
- iOS - go to `<PluginName>/Source/ThirdParty/IOS/versions.txt` file

---

## Where is the demo project?

The demo level and the blueprints are included in the plugin `Content` folder, to see them, in the inspector, you must enable `Show Plugin Content` option in the View Options of the Content Browser tab.

![](/images/issues/show-plugin-content.png)

---

## What information should I provide when reporting the issue?

Hi there, if you are reading this, this means you did not provide enough information to troubleshoot your issue :) Please provide the information below so we could better help you resolve the issue:

* Platform on which an issue occurs (Android, iOS, editor?)
* Steps to reproduce the issue
* Does this also happen if you run our demo level? (We always have a demo level packaged inside the plugin folder)
* Engine version and plugin version
* The text file containing the **COMPLETE** log from the editor if your issue occurs when building the plugin, or complete **Logcat or XCode** log from a device if the problem happens at runtime. (See the questions below if you don't know how to do this)
* All other relevant info:
  * Are you using remote build?
  * Are there other 3rd party plugins that might conflict with our plugins?

---

## Why do I need to copy the plugin to my project?

- You need to make source code changes
- Some features require this for them to work, UE Marketplace team uses a very old XCode version to package the plugins and sometimes you need to wrap features with `#if FEATURE_ENABLED #endif` defines for it to pass the processing on their end.

---

## How to correctly copy the Marketplace plugin to my project folder?

1. **First of all, your project must be a C++ project, you can easily convert your Blueprint-only project to C++ project by creating a random C++ class in the inspector**

2. Find your plugin in the Engine folder:
  - MAC: `/Users/Shared/Epic Games/UE_4.26/Engine/Plugins/Marketplace/YourPluginName`
  - Windows: `D:\UnrealEditors\UE_4.26\Engine\Plugins\Marketplace`

- Copy the plugin into the `Plugins` folder of the root of your project. So the folder structure is like `YourProjectFolder/Plugins/YourPluginName`
- Change the engine version in the `YourPluginName.uplugin` file to the UE version you are using.

Example:

![](/images/issues/plugin.png)

Now you can make changes to the plugin source code and they should be reflected every time you run the project on your device.

!> Don't forget to delete `Build`, `Intermediate`, `Binaries` folders **both from project folder and plugin folder** as you make changes to the plugin!

---

## How do I get a logcat log from my Android device?

Get Android Studio and [follow these instructions](https://developer.android.com/studio/debug/am-logcat) or [run it from the command line using ADB](https://developer.android.com/studio/command-line/logcat).

---

## How do I get an XCode log from an iOS device when running my UE project?

* Run your project from the Editor at least once
* Open your project folder in Finder and go to `Intermediate/ProjectFilesIOS` and open the project by double-clicking the `YourProject.xcodeproj` file.
* Run the project on your iOS device and get the logs in the log window

![](/images/issues/xcode-proj.png)
![](/images/issues/logs.png)

---