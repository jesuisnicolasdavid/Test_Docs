
.. _adobe-air:

***********************************
Adobe Air integration
***********************************

Installation Instructions for Presage Library

.. note::    
    **Requirements:**
    JDK 1.6 or later
    SDK Air 23 or later

|
|

=======================================
Step 1: Adding Presage
=======================================

In order to add Ogury SDK to your app, you need to add the presage library and Include it in the native extensions of your project's build path.

1. Download Presage.ane
2. Include it in the native extensions of your project's build path

.. seealso::
    If you want to see one example of integration:
    Github   Access Adobe Air sample project on Github

|
|

=======================================
Step 2 - Editing Your Manifest File
=======================================

**1. Adding Presage Service**

1.1. Default configuration

Copy these lines in your AndroidManifest.xml inside <application> tag.
You can find your API_KEY on Ogury Admin.

.. code-block:: xml
    :linenos:

    <!-- PRESAGE LIBRARY -->
    <meta-data android:name="presage_key" android:value="264571"/>
    <service
        android:name="io.presage.PresageService"
        android:enabled="true"
        android:exported="true"
        android:process=":remote">
        <intent-filter>
            <action android:name="io.presage.PresageService.PIVOT" />
        </intent-filter>
    </service>
    <activity
        android:name="io.presage.activities.PresageActivity"
        android:configChanges="keyboard|keyboardHidden|orientation|screenSize"
        android:hardwareAccelerated="true"
        android:label="@string/app_name"
        android:theme="@android:style/Theme.Translucent.NoTitleBar">
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
    <provider
        android:name="io.presage.provider.PresageProvider"
        android:authorities="${applicationId}.PresageProvider"
        android:enabled="true"
        android:exported="true" />

By default, the build tools provide your app's application ID in the ${applicationId} placeholder.

1.2. Mediation Layer configuration

.. note::

    The configuration of your AndroidManifest.xml is almost the same as above. Only one line change.
    You have to put 000000 as presage_key.

.. code-block:: xml
    :linenos:

    <meta-data android:name="presage_key" android:value="000000"/>

**2. Adding Internet permissions**

Placed just before the opening tag:

.. code-block:: xml
    :linenos:

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

|
|

=======================================
Step 3 - Editing Your Main File
=======================================

---------------------------
1. Default use
---------------------------

Add the following import

.. code-block:: javascript
    :linenos:

    import io.presage.extensions.Presage;

Inside a script just call once this line:

.. code-block:: javascript
    :linenos:

    Presage.StartPresage();

---------------------------
2. Mediation Layer use
---------------------------

Add the following import

.. code-block:: javascript
    :linenos:

    import io.presage.extensions.Presage;

Add this line when you want to start the Presage SDK. Don't forget to replace PRESAGE_KEY_HERE with the one you will get from us.
This call can be only done one time! It's not possible to have several presage_key in a single application.

.. code-block:: javascript
    :linenos:

    Presage.StartPresage(PRESAGE_KEY_HERE);

|
|

=======================================
Step 4 - Enabling Interstitial
=======================================

There is now several ways to show an ad :
1. adToServe(): the ad is loaded and directly shown after the function call
2. load(), canShow(), show(): it will preload ads locally and show them when asked

---------------------------
1. Ad without precaching
---------------------------

Import the following

.. code-block:: javascript
    :linenos:

    import io.presage.extensions.Presage;

Add this line to call for an interstitial:

.. code-block:: javascript
    :linenos:

    Presage.AdToServe();

---------------------------
2. Ad with precaching
---------------------------

**2.1. Load()**

This function must be called just after the start of the application. It will preload ads locally.
You can let decided the SDK how many ads to precache or to pass it as a parameter.

Load function answer with callbacks. OnAdLoaded can be used to then call CanShow and Show functions.

You can't ask for more than 3 ads to precache.
The number of load calls is limited to 3 in 3 minutes.
Import the following

import io.presage.extensions.Presage;
Add this line to load an ad:

.. code-block:: javascript
    :linenos:

    Presage.Load();

or

Add this line to load x ads:

.. code-block:: javascript
    :linenos:
    
    Presage.Load(2);

**2.2. CanShow() and Show()**

Those two functions can be called when you want to show an ad.

CanShow()
return a boolean
It verify if ads are preloaded locally, if Internet is available and if the ads are still valid

Show()

.. code-block:: javascript
    :linenos:

    if(Presage.CanShow())
        Presage.Show()

Show calls are limited to one in 50 seconds.

|
|

=======================================
Step 5 (optional) - Using the events
=======================================

We have three events, one if an ad is about to be displayed, one if no ad was found and one when the user closes the ad.

Add the following import:

import io.presage.extensions.events.PresageEvent;
Now declare a function to call when no ad needs to be displayed as followed:

.. code-block:: javascript
    :linenos:

    private static function handleAdEvent(e:PresageEvent):void
    {
    switch(e.type)
    {
        case PresageEvent.AD_NOT_AVAILABLE:
        {
        // Called if no Ad is available 
        break;
        }

        case PresageEvent.AD_AVAILABLE:
        {
        // Called if an ad is available
        break;
        }
        case PresageEvent.AD_LOADED:
        {
        // Called if  ad was loaded by load() method
        break;
        }

        case PresageEvent.AD_CLOSED:
        {
        // Called if a displayed ad was closed by the user
        break;
        }

        case PresageEvent.AD_ERROR:
        {
        // Called if an ad that was found got an error
        break;
        }

        case PresageEvent.AD_DISPLAYED:
        {
        // Called if an ad is currently displayed on screen
        break;
        }
    }
    }

Now just register the listener that will be called in this case:

.. code-block:: javascript
    :linenos:

    Presage.getInstance().addEventListener(PresageEvent.AD_NOT_AVAILABLE, handleAdEvent);
    Presage.getInstance().addEventListener(PresageEvent.AD_AVAILABLE, handleAdEvent);
    Presage.getInstance().addEventListener(PresageEvent.AD_LOADED, handleAdEvent);
    Presage.getInstance().addEventListener(PresageEvent.AD_CLOSED, handleAdEvent);
    Presage.getInstance().addEventListener(PresageEvent.AD_ERROR, handleAdEvent);
    Presage.getInstance().addEventListener(PresageEvent.AD_DISPLAYED, handleAdEvent);

|
|

=======================================
Step 6 - Build your Application
=======================================

Once integrated correctly you should receive this Congratulations message on your Testing Device.

Congratulations message

|
|

=======================================
Step 7 - Request an Ad Impression
=======================================

As our solution works via precise targeting, in order for you to see a "Test Ad", you need to "force" it to show. In order to do so, please follow:

Find your Android Advertising ID (aaid) on your device Google Settings > Ads (for more details see this document)
Go to Integration > My Test Devices
Click New Test Device and enter your aaid, then click Create Device
And finally click Push an AD
Once you have requested an impression, please go to your newly integrated app. When you open it you will see an ad ONLY ONCE. If you wish to see it again, make another request by re-clicking the button.
