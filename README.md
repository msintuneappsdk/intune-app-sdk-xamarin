# intune-app-sdk-xamarin
Microsoft Intune App SDK xamarin component for enabling mobile app management and data protections policies with iOS and Android Xamarin apps


## Using Intune App SDK in beta

Once released, the Intune App SDK will be accessible directly from the Xamarin component store. While in beta, developers will be able to add the component locally through the following steps:

1.	Download Xamarin-component.exe from [here](https://components.xamarin.com/submit/xkg) and extract it.

2.	Download the Intune App SDK Xamarin component folder and extract it. Both files downloaded from step 1 and step 2 should be in the same directory level.

3.	In the command line as an admin, run Xamain.Component.exe install <.xam> file.
  
4.	In Visual Studio, right click `components` in your previously created Xamarin project.

5.	Select `Edit Components` and add the Intune App SDK component you’ve downloaded locally to your computer.

Note:  Once released, you will be able to add the component directly from the component store in Visual Studio.
You can now begin building the component into your iOS and Android Xamarin apps.

## Enabling MAM in your iOS mobile app
1.	In order to initialize the Intune App SDK, you will need to make a call for any API in AppDelegate.cs class e.g.
      ```C#
      public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
      {
            Console.WriteLine ("Is Managed: {0}", IntuneMAMPolicyManager.Instance.PrimaryUser != null);
            return true;
      }
      ```
2.	Now that the component is added an initialized, you can follow the general steps required for building the App SDK into an iOS mobile app. You can find the full documentation for enabling native iOS apps [here](https://msdn.microsoft.com/en-us/library/mt627812.aspx). For the purpose of your iOS Xamarin app, follow steps 5 through 9 from the Intune App SDK for iOS guide.
Note: There are several modifications specific to Xamarin iOS apps. For instance, in step 7, when enabling keychain groups, you will need to add the following to include the Xamarin sample app we included in the component. Below is an example of the groups you would need to have in your Keychain Access groups:
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

You have completed the steps necessary to build the component into your iOS Xamarin app. You can now follow additional steps included in the sample we’ve included in IntuneMAMSample, you should be able to begin testing your iOS mobile app. If you are utilizing Xcode for building your project, you can use the Intune App SDK Settings.bundle. This will allow you to toggle Intune policy settings on and off as you build your project to diagnose and debug issues. To take advantage of this bundle, follow the steps in the iOS guide under Debugging the Intune App SDK in Xcode.

## Enabling MAM in your Android mobile app
For Xamarin apps not using a UI framework, you will need to follow the steps the Intune App SDK for Android. For the purposes of your Xamarin Android app, you will need to replace class, methods, and activities with their MAM equivalent based on the table included in the guide. If your app doesn’t define an android.app.Application class, you will need to create one and ensure that you inherit from MAMApplication.

For Xamarin Forms and other UI frameworks, we have provided a tool called MAM.Remapper. The tool will accomplish the class replacement for you. However, you will need to do the following steps:

1.	Add a reference to the Microsoft.Intune.MAM.Remapper.Tasks nuget package version 0.1.0.0 or greater.

2.	Add the following line to your Android csproj: `<Import Project="$(NugetPack)\Microsoft.Intune.MAM.Remapper.Tasks.0.1.X.X\build\MonoAndroid10\Microsoft.Intune.MAM.Remapper.targets" />`

3.	Set the build action of the added remapping-config.json file to RemappingConfigFile. The included remapping-config.json only works with Xamarin.Forms. For other UI frameworks, refer to the Readme included with the Remapper nuget package.

You have completed the basic steps of building the component into your app. Now you can follow the steps included in the Xamarin Android sample app. We have provided two samples, one for Xamarin.Forms and another for Android.
