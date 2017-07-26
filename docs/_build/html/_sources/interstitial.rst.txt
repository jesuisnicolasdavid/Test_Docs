.. _interstitials:

Interstitials
===========================================================


Ogury Interstitial ad format is a full-screen ad unit, usually served at natural transition points during the app lifecycle. Our interstitials support both static and video. You can use the `Presage` object and its associated listeners to precache and display interstitial ads in your app.

###########################################
1. Before You Start
###########################################

.. note::
    - Make sure you have correctly integrated the Ogury SDK into your application. Integration is outlined here.

    - If you want to display Ogury Interstitials ad format through a mediation, you can go on the :ref:`mediations` page to have some help : 

***************************************
1.1 Integration
***************************************

Make sure that you have correctly integrated the Ogury SDK into your application with the :ref:`quickstart-integration` guide.

***************************************
1.2 Placement
***************************************

Placement can be used for separating your revenue by placement in our dashboard and determining which placement earns you more money. Also you can configure your impression logic for each placement in the placement dashboard on admin. 
If you have no placement, or call Presage.show with placement that do not exist, the impression will be tagged with 'default' placement and its settings will be applied.
In order to use placement within your app, variable in source like <AD_UNIT_ID> must be replaced with your information. Ad Unit ID can be created on Ogury admin. For more informations on Ogury Ad Units you can go on the :ref:`ad-placements` page
To display an ads with Ogury, you need to pass by a placement object even if you are not going to use them but AD unit ID can stay empty. If you want to use placement features, integrate you AD Unit ID within your placement object.

 First, import Presage and declare a `Presage` instance variable:

.. code-block:: java
    :linenos:    

    import io.presage.Presage;
    import io.presage.ads.PresageInterstitial;
   
    PresageInterstitial placement1;


`Presage` provides a listener interface, `InterstitialAdListener`, which you’ll use to stay informed about ad lifecycle events. Make sure your `Activity` implements the `InterstitialAdListener` interface. In your `Activity`’s `onCreate()` method, instantiate the `Presage` using the context and the Ad Unit ID you created on Ogury Admin.

.. code-block:: java
    :linenos:

    private PresageInterstitial.PresageInterstitialCallback presageInterstitialCallback = new PresageInterstitial.PresageInterstitialCallback() {
                @Override
                public void onAdAvailable() {
                    Log.i("PRESAGE", "ad available");
                }

                @Override
                public void onAdNotAvailable() {
                    Log.i("PRESAGE", "no ad available");
                }

                @Override
                public void onAdLoaded() {
                    Log.i("PRESAGE", "an ad in loaded, ready to be shown");
                }

                @Override
                public void onAdDisplayed() {
                    Log.i("PRESAGE", "ad displayed");
                }

                @Override
                public void onAdClosed() {
                    Log.i("PRESAGE", "ad closed");
                }

                @Override
                public void onAdError(int code) {
                    Log.i("PRESAGE", String.format("error with code %d", code));
        };


Next, you can create your placement object with your Ad Unit ID coming from Ogury admin :
 
.. code-block:: java
    :linenos:

    placement1 = new PresageInterstitial(this, "AD_UNIT_ID");

Then you can link callbacks with this placement object :

.. code-block:: java
    :linenos:

    placement1.setPresageInterstitialCallback(presageInterstitialCallback);

Nice job! Now you are ready to display an ad.


The Ogury SDK provides a custom class, `Presage`, that handles several ways to show fullscreen interstitial ad within a placement :

1. adToServe(): the ad is loaded and directly shown after the function call (best for splashscreen)
2. load(), canShow(), show(): it will preload ads locally and show them when asked (best for user experience)

.. note::
    The setContext() and the start() function explained in :ref:`step3` have to be called before using one of those methods.



################################################################
2. Create and display an interstitial ad with precaching
################################################################

To ensure a smooth experience, you should pre-cache the content as soon as your `Activity` is created, then display it if the precaching was successful.
In the `Activity` in which you want to show the interstitial ad,

Displaying the ad with precaching takes four steps:

1. In your `onCreate()` method, call `load()` to begin precaching the ad. It’s important to precache interstitial ad content well before you plan to show it, since it often incorporates rich media and may take some time to load. We suggest precaching when your `Activity` is first created, but you may also choose to do it based on events in your app, like at the beginning of a game level.

2. When you’re ready to display your ad, use `placement.canShow()` to check whether the interstitial was successfully precached.

3. If `canShow()` returns true, display the interstitial by calling the `show()` method.

4. Lastly, remember to call `destroy()` on the interstitial in your `Activity`’s `onDestroy` method. Your completed `Activity` code should look something like this:


***************************************
    2.1. Load()
***************************************

This function must be called just after the start of the application. It will preload ads locally.
You can let decided the SDK how many ads to precache or to pass it as a parameter.

You can't ask for more than 3 ads to precache.
The number of load calls is limited to 3 in 3 minutes.

Load function answer with IADHandler callbacks. onAdLoaded can be used to then call canShow and show functions.

Import the following :

.. code-block:: java
    :linenos:

    placement1 = new PresageInterstitial(this, "AD_UNIT_ID");
    placement1.setPresageInterstitialCallback(presageInterstitialCallback);
    placement1.load();

or ask for x ads :

.. code-block:: java
    :linenos:

    placement1 = new PresageInterstitial(this, "AD_UNIT_ID");
    placement1.setPresageInterstitialCallback(presageInterstitialCallback);
    placement1.load(3);

***************************************
2.2. canShow() and show()
***************************************

Those two functions can be called when you want to show an ad.

canShow()
return a boolean
It verify if ads are preloaded locally, if Internet is available and if the ads are still valid


Show calls are limited to one each 10 seconds.

Import the following :

.. code-block:: java
    :linenos:

    if(placement1.canShow()) {
            placement1.show();
        }


################################################################
3. Create and display an interstitial ad without precaching
################################################################
adToServe(): the ad is loaded and directly shown after the function call

This process is really useful if you want to display ads on splashscreen / launch of the app.

Import the following :

.. code-block:: java
    :linenos:
    
    placementDefault.adToServe();




################################################################
4. Request an Ad Impression
################################################################


.. note::
    As our solution works via precise targeting, in order for you to see a "Test Ad", you need to "force" it to show. In order to do so, please follow:

Find your Android Advertising ID (aaid) on your device Google Settings > Ads (for more details see this document)
Go to Integration > My Test Devices
Click New Test Device and enter your aaid, then click Create Device
And finally click Push an AD
Once you have requested an impression, please go to your newly integrated app. When you open it you will see an ad ONLY ONCE. If you wish to see it again, make another request by re-clicking the button.




