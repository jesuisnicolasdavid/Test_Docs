.. _optin-video:

###########################################
Optin Video
###########################################


===========================================================
1. Before You Start
===========================================================

.. note::
    - Make sure you have correctly integrated the Ogury SDK into your application. Integration is outlined here.

    - If you want to display Ogury OptinVideo ads format through a mediation, you can go on this page to have some help.


===========================================================
2. Placement
===========================================================

Placement can be used for separating your revenue by placement in our dashboard and determining which placement earns you more money. Also you can configure your impression logic for each placement in the placement dashboard on admin. 
In order to use placement within your app, variable in source like <AD_UNIT_ID> must be replaced with your information. Ad Unit ID can be created and managed on Ogury admin. For more informations on Ogury Ad Units you can go on the :ref:`ad-placements` page.
If you want to use placement features, integrate you AD Unit ID within your placement object.

.. note::
    Ogury optin video are a special ad unit that requires to be configured on Ogury's admin. If you have no ad unit id, you will not be able to display an Optin Video ad unit. You must configure an optin video placement on Ogury admin and integrate this ad unit ID into the optin video object you are going to create.

 First, import Presage and declare a `Presage` instance variable:

.. code-block:: java
    :linenos:    

    import io.presage.Presage;
    import io.presage.ads.PresageOptinVideo;
   
    PresageOptinVideo placementOptin;


`Presage` provides a listener interface, `PresageOptinVideoCallback`, which you’ll use to stay informed about ad lifecycle events. Make sure your `Activity` implements the `PresageInterstitialCallback` interface. In your `Activity`’s `onCreate()` method, instantiate the `Presage` using the context and the Ad Unit ID you created on Ogury Admin.

.. code-block:: java
    :linenos:

    private PresageOptinVideo.PresageOptinVideoCallback presageOptinVideoCallback = new PresageOptinVideo.PresageOptinVideoCallback() {
        @Override
        public void onAdAvailable() {
            appendLog("Ad Available");
        }

        @Override
        public void onAdNotAvailable() {
            appendLog("Ad Not Available");
        }

        @Override
        public void onAdLoaded() {
            appendLog("Ad Loaded");
        }

        @Override
        public void onAdClosed() {
            appendLog("Ad Closed");
        }

        @Override
        public void onAdError(int code) {
            appendLog("Ad Error");
        }

        @Override
        public void onAdDisplayed() {
            appendLog("Ad Displayed");
        }

        @Override
        public void onAdRewarded() { appendLog("Rewarded +7"); }
    };


Next, you can create your placement object with your Ad Unit ID coming from Ogury admin :
 
.. code-block:: java
    :linenos:

    placementOptin = new PresageOptinVideo(this, "AD_UNIT_ID");

Then you can link callbacks with this placement object :

.. code-block:: java
    :linenos:

    placementOptin.setPresageOptinVideoCallback(presageOptinVideoCallback);

Great! Now you are ready to display an ad.


The Ogury SDK provides a custom class, `Presage`, that handles several ways to show fullscreen Optin video ad within a placement :

1. adToServe(): the ad is loaded and directly shown after the function call (best for splashscreen)
2. load(), canShow(), show(): it will preload ads locally and show them when asked (best for user experience)

.. note::
    The setContext() and the start() function explained in :ref:`step3` have to be called before using one of those methods.



===========================================================
3. Create and display an optin video ad with precaching
===========================================================

To ensure a smooth experience, you should pre-cache the content as soon as your `Activity` is created, then display it if the precaching was successful.
In the `Activity` in which you want to show the Optin video,

Displaying the ad with precaching takes four steps:

1. In your `onCreate()` method, call `load()` to begin precaching the ad. It’s important to precache Optin video ad content well before you plan to show it, since it often incorporates rich media and may take some time to load. We suggest precaching when your `Activity` is first created, but you may also choose to do it based on events in your app, like at the beginning of a game level.

2. When you’re ready to display your ad, use `placement.canShow()` to check whether the Optin video was successfully precached.

3. If `canShow()` returns true, display the Optin video by calling the `show()` method.

4. Lastly, remember to call `destroy()` on the Optin video in your `Activity`’s `onDestroy` method. Your completed `Activity` code should look something like this:


***************************************
    3.1. Load()
***************************************

This function must be called just after the start of the application. It will preload ads locally.
You can let decided the SDK how many ads to precache or to pass it as a parameter.

You can't ask for more than 3 ads to precache.
The number of load calls is limited to 3 in 3 minutes.

Load function answer with IADHandler callbacks. onAdLoaded can be used to then call canShow and show functions.

Import the following :

.. code-block:: java
    :linenos:

    placementOptin.load();

or ask for x ads :

.. code-block:: java
    :linenos:

    placementOptin.load(3);

***************************************
3.2. canShow() and show()
***************************************

Those two functions can be called when you want to show an ad.

canShow()
return a boolean
It verify if ads are preloaded locally, if Internet is available and if the ads are still valid


Show calls are limited to one each 10 seconds.

Import the following :

.. code-block:: java
    :linenos:

    if(placementOptin.canShow()) {
            placementOptin.show();
        }


===========================================================
4. Create and display an optin video ad without precaching
===========================================================

adToServe(): the ad is loaded and directly shown after the function call

This process is really useful if you want to display ads on splashscreen / launch of the app.

Import the following :

.. code-block:: java
    :linenos:
    
    placementOptin.adToServe();

===========================================================
5. Client Side callback & Server to Server callback
===========================================================









