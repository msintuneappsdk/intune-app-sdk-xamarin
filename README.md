# Microsoft Intune App SDK Xamarin Component

## Overview
The Intune App SDK Xamarin component enables [Intune mobile app management features](https://docs.microsoft.com/en-us/intune/deploy-use/protect-app-data-using-mobile-app-management-policies-with-microsoft-intune) in iOS and Android apps built with Xamarin. The component allows developers to easily build in app restriction and data protection features into their Xamarin-based app.

You will find that you can enable SDK features without changing your app’s behavior. Once you've built the component into your iOS or Android mobile app, the IT admin will be able to deploy policy via Microsoft Intune supporting a variety of features that enable data protection.

## Get started

1.	Download Xamarin-component.exe from [here](https://components.xamarin.com/submit/xpkg) and extract it.

2.	Download the Intune App SDK Xamarin Component folder from this repository and extract it. Both files downloaded from step 1 and step 2 should be in the same directory level.

3.	In the command line as an admin, run Xamain.Component.exe install <.xam> file.

4.	In Visual Studio, right click **components** in your previously created Xamarin project.

5.	Select **Edit Components** and add the Intune App SDK component you’ve downloaded locally to your computer.



## Enabling Intune MAM in your iOS mobile app
1.	In order to initialize the Intune App SDK, you will need to make a call for any API in the `AppDelegate.cs` class. For example:

      ```csharp
      public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
      {
            Console.WriteLine ("Is Managed: {0}", IntuneMAMPolicyManager.Instance.PrimaryUser != null);
            return true;
      }

      ```

2.	Now that the component is added and initialized, you can follow the general steps required for building the App SDK into an iOS mobile app. You can find the full documentation for enabling native iOS apps in the [Intune App SDK for iOS Developer Guide](https://docs.microsoft.com/en-us/intune/develop/intune-app-sdk-ios).
3. **Note**: There are several modifications specific to Xamarin-based iOS apps. For instance, when enabling keychain groups, you'll need to add the following to include the Xamarin sample app we included in the component. Below is an example of the groups you would need to have in your Keychain Access groups:

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
      <plist version="1.0">
            <dict>
                  <key>keychain-access-groups</key>
                  <array>
                        <string>$(AppIdentifierPrefix)com.xamarin.microsoftintunesample</string>
                        <string>$(AppIdentifierPrefix)com.xamarin.microsoftintunesample.intunemam</string>
                        <string>$(AppIdentifierPrefix)com.microsoft.intune.mam</string>
                        <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
                  </array>
            </dict>
      </plist>
      ```

You have completed the steps necessary to build the component into your Xamarin-based iOS app. If you are utilizing Xcode for building your project, you can use the `Intune App SDK Settings.bundle`. This will allow you to toggle Intune policy settings on and off as you build your project to test and debug. To take advantage of this bundle, follow the steps in the [Intune App SDK for iOS Developer Guide](https://docs.microsoft.com/en-us/intune/develop/intune-app-sdk-ios) and read the section on [debugging in Xcode](https://docs.microsoft.com/en-us/intune/develop/intune-app-sdk-ios#debug-information).

## Enabling MAM in your Android mobile app
For Xamarin-based Android apps not using a UI framework, you will need to read and follow the [Intune App SDK for Android Developer Guide]. For your Xamarin-based Android app, you will need to replace class, methods, and activities with their MAM equivalent based on the [table](https://docs.microsoft.com/en-us/intune/develop/intune-app-sdk-android#replace-classes-methods-and-activities-with-their-mam-equivalent-required) included in the guide. If your app doesn’t define an `android.app.Application` class, you will need to create one and ensure that you inherit from `MAMApplication`.

For Xamarin Forms and other UI frameworks, we have provided a tool called `MAM.Remapper`. The tool will accomplish the class replacement for you. However, you will need to do the following steps:

1.	Add a reference to the` Microsoft.Intune.MAM.Remapper.Tasks` nuget package version 0.1.0.0 or greater.

2.	Add the following line to your Android csproj:
  ```xml
  <Import
  Project="$(NugetPack)\\Microsoft.Intune.MAM.Remapper.Tasks.0.1.X.X\\build\\MonoAndroid10\\Microsoft.Intune.MAM.Remapper.targets" />
  ```

3.	Set the build action of the added `remapping-config.json` file to **RemappingConfigFile**. The included `remapping-config.json` only works with Xamarin.Forms. For other UI frameworks, refer to the Readme included with the Remapper nuget package.

You have completed the basic steps of building the component into your app. Now you can follow the steps included in the Xamarin Android sample app. We have provided two samples, one for Xamarin.Forms and another for Android.
