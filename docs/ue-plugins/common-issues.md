# FAQ

## Is the source included in the plugin?

Yes, all the source code is inside the plugin and you can modify it as you see fit.

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

## How do I get a logcat log from my Android device?

Get Android Studio and [follow these instructions](https://developer.android.com/studio/debug/am-logcat) or [run it from the command line using ADB](https://developer.android.com/studio/command-line/logcat).

## How do I get an XCode log from an iOS device when running my UE project?

* Run your project from the Editor at least once
* Open your project folder in Finder and go to `Intermediate/ProjectFilesIOS` and open the project by double-clicking the `YourProject.xcodeproj` file.
* Run the project on your iOS device and get the logs in the log window