
.. _ionic-phonegap-cordova:

***********************************
PhoneGap / Ionic / Cordova
***********************************

Cordova - Phonegap - Ionic
The core of this following plugin will work for Cordova, Phonegap and Ionic. The instructions may however slightly change.

You can find more infos about Presage lib in the Android documentation.

Installation Instructions
Cordova
Phonegap 3.5 min
Ionic 1.0.0 min
The syntax on the config.xml file may change between each version.

=======================================
Step 1: Adding Presage
=======================================

Current version is 2.1.11

presage-lib.jar : Download Link
presage-phonegap.zip : Download Link

Download presage-lib.jar
Download presage-cordova-2.1.11.zip
Unzip presage-files.zip to get presage-files folder
Add the presage-lib.jar file to the platforms/android/libs folder
Merge the presage-files content in platforms/android folder
If you want to see one example of integration:
Github   Access AppIntegration sample project on Github

=======================================
Step 2 - Editing Your Manifest File
=======================================

Copy these lines in your AndroidManifest.xml inside <application> tag.
Don't forget your API Key

.. code-block:: xml
    :linenos:

    <!-- PRESAGE LIBRARY -->
    <meta-data android:name="presage_key" android:value="YOUR API KEY" />
    <service android:enabled="true" android:exported="true" android:name="io.presage.PresageService" android:process=":remote">
    <intent-filter>
    <action android:name="io.presage.PresageService.PIVOT" />
    </intent-filter>
    </service>
    <activity android:configChanges="keyboard|keyboardHidden|orientation|screenSize" android:hardwareAccelerated="true" android:label="@string/app_name" android:name="io.presage.activities.PresageActivity" android:theme="@android:styl\
    e/Theme.Translucent.NoTitleBar">
    <intent-filter>
    <action android:name="io.presage.intent.action.LAUNCH_WEBVIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
    </activity>
    <receiver android:name="io.presage.receiver.NetworkChangeReceiver">
    <intent-filter>
    <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
    <action android:name="android.net.wifi.WIFI_STATE_CHANGED" />
    <action android:name="io.presage.receiver.NetworkChangeReceiver.ONDESTROY" />
    </intent-filter>
    </receiver>
    <receiver android:name="io.presage.receiver.AlarmReceiver" />
    <provider android:authorities="${applicationId}.PresageProvider" android:enabled="true" android:exported="true" android:name="io.presage.provider.PresageProvider" />
    Mandatory Permission

        <uses-permission android:name="android.permission.INTERNET" />                                     
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    Optional Permission

        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="com.android.launcher.permission.INSTALL_SHORTCUT" />
        <uses-permission android:name="com.android.launcher.permission.UNINSTALL_SHORTCUT" />

============================================
Step 3 - Editing Your Main config.xml file
============================================

Just add the Presage declaration to your <YOUR_APP_FOLDER>/config.xml file as followed:

.. code-block:: xml
    :linenos:

    <gap:platform name="android">
        <gap:feature name="CPresage">
            <param name="android-package" value="io.presage.CPresage"/>
            <param name="onload" value="true" />
        </gap:feature>
    </gap:platform>

The call to the line below ensure us of the initialization on page load.

.. code-block:: javascript
    :linenos:

    <param name="onload" value="true" />

===========================================
Step 4 - Editing Your Main index.html file
===========================================

Just add the Presage declaration to your index.html file as followed:

.. code-block:: xml
    :linenos:

    <script type="text/javascript" src="CPresage.js"></script>

=======================================
Step 5 - Enabling Interstitial
=======================================

---------------------------
1. Ad without precaching
---------------------------

Just make this call, whenever you want to try displaying the interstitial as shown in this small sample:

.. code-block:: javascript
    :linenos:

    CPresage.adToServe(app.onAdEvent, app.onAdNotFound);

You can see that we are using two callbacks here that enable you to do specific treatments depending on the result of the call.

---------------------------
2. Ad with precaching
---------------------------

2.1. Load()

This function must be called just after the start of the application. It will preload ads locally. You can let decided the SDK how many ads to precache or to pass it as a parameter.

You can't ask for more than 3 ads to precache. The number of load calls is limited to 3 in 3 minutes.

Load function answer with IADHandler callbacks. OnAdLoaded can be used to then call CanShow and show functions.

.. code-block:: javascript
    :linenos:

    CPresage.load(app.onAdEvent, app.onAdNotFound);

2.2. CanShow() and Show()

Those two functions can be called when you want to show an ad.

CanShow() return a boolean It verify if ads are preloaded locally, if Internet is available and if the ads are still valid

Show(IADHandler iADHandler) answer with IADHandler callbacks

Show calls are limited to one in 50 seconds.

.. code-block:: javascript
    :linenos:

    CPresage.canShow();
    CPresage.show(app.onAdEvent, app.onAdNotFound);

==============================================
Step 5 (optional) - "Backfill" with other ads
==============================================

To maximize your revenues, you can choose to backfill our SDK when we have no ads to show with ads from an other network. To do this, make the call to another interstitial when the OnAdNotAvailable or the OnAdError event is triggered.

=======================================
Step 6 - Build your Application
=======================================

Once integrated correctly you should receive this Congratulations message on your Testing Device.

Congratulations message

Request an Ad Impression

As our solution works via precise targeting, in order for you to see a "Test Ad", you need to "force" it to show. In order to do so, please follow:

Find your Android Advertising ID (aaid) on your device Google Settings > Ads (for more details see this document)
Go to Integration > My Test Devices
Click New Test Device and enter your aaid, then click Create Device
And finally click Push an AD
Once you have requested an impression, please go to your newly integrated app. When you open it you will see an ad ONLY ONCE. If you wish to see it again, make another request by re-clicking the button.
