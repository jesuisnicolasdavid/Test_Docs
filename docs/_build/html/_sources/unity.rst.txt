.. _unity:

***********************************
Unity
***********************************
    Installation Instructions for Presage Unity3D Library

.. note::
    **Requirements:**
    Unity 4.5 or later

|
|

=======================================
Step 1: Adding Presage
=======================================

In order to add Ogury SDK to your app, you need to add the presage library and merge all the assets in your project.

1. Download presage-unity.zip
2. Merge all the Assets in your project

.. seealso::
    If you want to see one example of integration:
    Github   Access Unity3D C# sample project on Github

|
|

=======================================
Step 2 - Replace Default values
=======================================

**1. Your API Key**

Please change the value of API_KEY_HERE in Assets/Plugins/Android/AndroidManifest.xml file with your own.
You can find your API_KEY on Ogury Admin.

**2. The name of your app**

Please change the value of NAME_OF_YOUR_GAME in Assets/Plugins/Android/res/values/strings.xml file with your own.

|
|

=======================================
Step 3 - Starting the SDK
=======================================

In your Unity app, you have to call this method to Initialize and Start the Presage SDK:

.. code-block:: c++
    :linenos:

    #if UNITY_ANDROID
        // Initializing WPresage
        WPresage.Initialize();
    #endif

Usually it's on the app start.

|
|

=======================================
Step 4 - Enabling Interstitial
=======================================

There is now several ways to show an ad :
1. AdToServe(): the ad is loaded and directly shown after the function call
2. Load(), CanShow(), Show(): it will preload ads locally and show them when asked

The Initialize() function have to be called before using this.

When you want to enable the interstitial, implements the IADHandler interface:

.. code-block:: java
    :linenos:

    /** 
    * Implementation of the Handler used for AdToServe
    */
    public class WHandlerImpl : WPresage.IADHandler {

            public void OnAdAvailable() {
                Debug.Log("ad available");
            }

            public void OnAdNotAvailable() {
                Debug.Log("no ad available");
            }

            public void OnAdLoaded() {
                Debug.Log("an ad in loaded, ready to be shown");
            }

            public void OnAdDisplayed() {
                Debug.Log("ad displayed");
            }

            public void OnAdClosed() {
                Debug.Log("ad closed");
            }

            public void OnAdError(int code) {
                Debug.Log("error with code");
            }
        }
    }

---------------------------
1. Ad without precaching
---------------------------

Enable the interstitial that way:

.. code-block:: c++
    :linenos:

    #if UNITY_ANDROID
        // Creation of our WHAndlerImpl to Handle Events
        WHandlerImpl handlerImpl = new WHandlerImpl();

        // Making AdToServe call to try to show an Ad if available
        WPresage.AdToServe(handlerImpl);
    #endif


---------------------------
2. Ad with precaching
---------------------------

**2.1. Load()**

This function must be called just after the start of the application. It will preload ads locally.
You can let decided the SDK how many ads to precache or to pass it as a parameter.

You can't ask for more than 3 ads to precache.
The number of load calls is limited to 3 in 3 minutes.

Load function answer with IADHandler callbacks. OnAdLoaded can be used to then call CanShow and show functions.

.. code-block:: c++
    :linenos:

    #if UNITY_ANDROID
        // Creation of our WHAndlerImpl to Handle Events
        WHandlerImpl handlerImpl = new WHandlerImpl();

        // Making AdToServe call to try to show an Ad if available
        WPresage.Load(handlerImpl);
    #endif

or ask for x ads

.. code-block:: c++
    :linenos:

    #if UNITY_ANDROID
        // Creation of our WHAndlerImpl to Handle Events
        WHandlerImpl handlerImpl = new WHandlerImpl();

        // Making AdToServe call to try to show an Ad if available
        WPresage.Load(2, handlerImpl);
    #endif

**2.2. CanShow() and Show()**

Those two functions can be called when you want to show an ad.

CanShow()
return a boolean
It verify if ads are preloaded locally, if Internet is available and if the ads are still valid

Show(IADHandler iADHandler)
answer with IADHandler callbacks

Show calls are limited to one in 50 seconds.

.. code-block:: c++
    :linenos:

    #if UNITY_ANDROID
        // Creation of our WHAndlerImpl to Handle Events
        WHandlerImpl handlerImpl = new WHandlerImpl();

        // Making AdToServe call to try to shox an Ad if available
        if (WPresage.CanShow())
            WPresage.Show(handlerImpl);
    #endif

|
|

===============================================
Step 5 (optional) - "Backfill" with other ads
===============================================

To maximize your revenues, you can choose to backfill our SDK when we have no ads to show with ads from an other network.
To do this, make the call to another interstitial when the OnAdNotAvailable or the OnAdError event is triggered.

|
|

===============================================
Step 6 - Build your Application
===============================================

Once integrated correctly you should receive this Congratulations message on your Testing Device.

|
|

===============================================
Step 7 - Request an Ad Impression
===============================================

As our solution works via precise targeting, in order for you to see a "Test Ad", you need to "force" it to show. In order to do so, please follow:

Find your Android Advertising ID (aaid) on your device Google Settings > Ads (for more details see this document)
Go to Integration > My Test Devices
Click New Test Device and enter your aaid, then click Create Device
And finally click Push an AD
Once you have requested an impression, please go to your newly integrated app. When you open it you will see an ad ONLY ONCE. If you wish to see it again, make another request by re-clicking the button.

|
|
