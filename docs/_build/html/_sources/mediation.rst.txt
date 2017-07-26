.. _mediations:

#################
Mediations
#################

*************************
Mopub
*************************


===========================================================
1. Mopub Interstitials Adapters
===========================================================

**Quick Start for Mopub Interstitials**

| 1. Create a subclass of CustomEventInterstitial in the com.mopub.mobileads package of your application.
|
| 2. Override the loadInterstitial(Context, CustomEventInterstitialListener, Map<String, Object>, Map<String, String>) method and implement code that requests an interstitial.
|
| 3. Override the showInterstitial() method and implement code that displays your interstitial.
|
| 4. When your ad loads successfully, you must notify MoPub of the event and display your ad by calling the onInterstitialLoaded() method on the CustomEventInterstitialListener object (passed in loadAd).
|
| 5. Similarly, you must notify the CustomEventInterstitialListener object when your ad fails to load by calling the onInterstitialFailed(MoPubErrorCode) method on the listener object.
|
| (Optional) Notify the listener object of other ad lifecycle events via the corresponding methods on CustomEventInterstitialListener.
| `public void onInterstitialShown();`
| `public void onInterstitialDismissed();`
| `public void onInterstitialClicked();`
| `public void onLeaveApplication();`
|
| (Optional) Override the onInvalidate() method of CustomEventInterstitial if your custom event requires any sort of cleanup.
|

6. Finally, on the MoPub web interface, create a network with the "Custom Native Network" type. Place the fully-qualified class name of your custom event (e.g. com.mopub.mobileads.YourCustomEventInterstitial) in the "Custom Class" column.

Once you've completed these steps, the MoPub SDK will be able to cause your `CustomEventInterstitial` subclass to be instantiated at the proper time while your application is running. You do not need to instantiate any of these subclasses in your application code. **Note:** the MoPub SDK will cause a new `CustomEventInterstitial` object to be instantiated on every ad call, so you can safely make changes to the custom event object's internal state between calls.
8

===========================================================
2. Mopub Optin Video Adapters
===========================================================

**Quick Start for Mopub Rewarded Video**

| 1. Create a subclass of `CustomEventRewardedVideo` in the com.mopub.mobileads package of your application.
|
| 2. Use `checkAndInitializeSdk(Context, Map<String, Object>, Map<String, String>) method` to initialize the network SDK 
|
| 3. Override the `loadWithSdkInitialized(Context, Map<String, Object>, Map<String, String>)` method and implement code that requests a rewarded interstitial. 
|
| 4. Override `hasVideoAvailable()` method to return whether there is a video available from the network SDK 
|
| 5. Override the `showVideo()` method and implement code that displays your rewarded interstitial. 
|
| 6. Notify the listener object of all the ad lifecycle events via the corresponding methods on `MoPubRewardedVideoManager`.
|   `public void onRewardedVideoLoadSuccess` 
|   `public void onRewardedVideoLoadFailure` 
|   `public void onRewardedVideoStarted` 
|   `public void onRewardedVideoPlaybackError` 
|   `public void onRewardedVideoClicked` 
|   `public void onRewardedVideoClosed` 
|   `public void onRewardedVideoCompleted` 
| 
| 7. (Optional) Override the `onInvalidate()` method of `CustomEventRewardedVideo` if your custom event requires any sort of cleanup. 
| 
| 8. (Optional) Implement `MediationSettings` marker interface if you want to enable to publishers to pass any network specific additional parameters during the initialization.
|
|

*************************
Admob
*************************

===========================================================
1. Admob Interstitials Adapters
===========================================================

**Quick Start for Admob Interstitials**

To implement an interstitial custom event, you first create an interstitial custom event within AdMob Mediation, much like a banner custom event. Then implement a CustomEventInterstitial to provide notification. This example uses the sample ad network as before.


1. Define a custom event :
You can define an interstitial custom event through the AdMob Interface. Make sure the value you give for the Class Name has a full path. Parameter should contain any information needed to make an ad request to the network you're implementing in the custom event.

2. Request an interstitial :
Define a class that implements CustomEventInterstitial; we'll call this class SampleCustomEventInterstitial. When the custom event is chosen from the mediation flow, mediation calls the requestInterstitialAd() method on the class name you provided in the settings. You can use the parameters provided in this method to make an interstitial request to your desired network. The following example shows how to request an interstitial from the sample ad network via a custom event:

Here's an example SampleCustomEventInterstitial.java implementation:

.. code-block:: java
    :linenos:

    public class SampleCustomEventInterstitial implements CustomEventInterstitial {

        /** Represents a {@link SampleInterstitial}. */
        private SampleInterstitial sampleInterstitial;

        @Override
        public void requestInterstitialAd(Context context,
                CustomEventInterstitialListener listener,
                String serverParameter,
                MediationAdRequest mediationAdRequest,
                Bundle customEventExtras) {
            /**
            * In this method, you should:
            * 1. Create your interstitial ad.
            * 2. Set your ad network's listener.
            * 3. Make an ad request.
            */

            sampleInterstitial = new SampleInterstitial(context);

            // Here we're assuming the serverParameter is the ad unit for the sample ad network.
            sampleInterstitial.setAdUnit(serverParameter);

            // Implement a SampleAdListener and forward callbacks to AdMob Mediation.
            sampleInterstitial.setAdListener(new SampleCustomInterstitialEventForwarder(listener));

            // Make an ad request.
            sampleInterstitial.fetchAd(createSampleRequest(mediationAdRequest));
        }

        /**
        * Helper method to create a {@link SampleAdRequest}.
        * @param mediationAdRequest The mediation request with targeting information.
        * @return The created {@link SampleAdRequest}.
        */
        private SampleAdRequest createSampleRequest(MediationAdRequest mediationAdRequest) {
            SampleAdRequest request = new SampleAdRequest();
            request.setTestMode(mediationAdRequest.isTesting());
            request.setKeywords(mediationAdRequest.getKeywords());
            return request;
        }

        @Override
        public void showInterstitial() {
            // Show your interstitial ad.
            sampleInterstitial.show();
        }
    }
    
The interstitial custom event interface requires you to implement the showInterstitial() method. Mediation invokes this method when you tell the Google Mobile Ads SDK to show the interstitial ad.

3. Notify AdMob Mediation :
Just as with the banner custom event example, implement your network's ad listener to send messages back to AdMob Mediation.

This example SampleCustomInterstitialEventForwarder class implements the SampleAdListener interface to forward the callbacks from the sample ad network:

.. code-block:: java
    :linenos:

    public class SampleCustomInterstitialEventForwarder extends SampleAdListener {
        private CustomEventInterstitialListener mInterstitialListener;

        /**
        * Creates a new {@code SampleInterstitialEventForwarder}.
        * @param listener An AdMob Mediation {@link CustomEventInterstitialListener} that should
        *                 receive forwarded events.
        */
        public SampleCustomInterstitialEventForwarder(CustomEventInterstitialListener listener) {
            this.mInterstitialListener = listener;
        }

        @Override
        public void onAdFetchSucceeded() {
            mInterstitialListener.onAdLoaded();
        }

        @Override
        public void onAdFetchFailed(SampleErrorCode errorCode) {
            switch (errorCode) {
                case UNKNOWN:
                    mInterstitialListener.onAdFailedToLoad(AdRequest.ERROR_CODE_INTERNAL_ERROR);
                    break;
                case BAD_REQUEST:
                    mInterstitialListener.onAdFailedToLoad(AdRequest.ERROR_CODE_INVALID_REQUEST);
                    break;
                case NETWORK_ERROR:
                    mInterstitialListener.onAdFailedToLoad(AdRequest.ERROR_CODE_NETWORK_ERROR);
                    break;
                case NO_INVENTORY:
                    mInterstitialListener.onAdFailedToLoad(AdRequest.ERROR_CODE_NO_FILL);
                    break;
            }
        }

        @Override
        public void onAdFullScreen() {
            mInterstitialListener.onAdOpened();
            // Only call onAdLeftApplication if your ad network actually exits the developer's app.
            mInterstitialListener.onAdLeftApplication();
        }

        @Override
        public void onAdClosed() {
            mInterstitialListener.onAdClosed();
        }
    }

This completes the custom events implementation for interstitial ads. The full example is available on GitHub. You can use it with an ad network that is already supported or modify it to display custom interstitial ads.

===========================================================
2. Admob Optin Video Adapters
===========================================================

**Quick Start for Admob Rewarded Video**

.. note::
    This guide is intended for those looking to use the AdMob Mediation platform to display rewarded video ads from a third party ad network.
    
    AdMob supported ad networks and custom events :
    Ad networks that are directly supported by the AdMob mediation platform will implement standard mediation adapters. Ad networks that are not directly supported by AdMob can still be be used to request and display rewarded videos by implementing custom event adapters. The differences between standard mediation adapters and custom event adapters are outlined below.

1. Defining server parameters :
Ad networks mediated to through the AdMob mediation platform need one or more identifiers in order to identify a publisher. These identifiers are represented as server parameters and are defined when configuring a third party ad network for mediation in the AdMob interface. If you are building a custom event adapter, follow the creating a custom event step below. If you are building an adapter your ad network and corresponding server parameters will be configured as part of the ad netowrk onboarding process with AdMob.

2. Creating a custom event :
To define a custom event, you must first create it in the AdMob interface. You can find instructions for creating a custom event in this help center guide. Once the custom event is defined, it points to a class within your app that implements MediationRewardedVideoAdAdapter to serve a rewarded video. The custom event also lists a server parameter that is passed to your rewarded video adapter.

The entries in the example screen shown above represent the following:

Class Name	The fully qualified name of the class that implements the custom event.
Label	A unique name for the event.
Parameter	An optional argument passed to your custom event.
Implementing a mediation adapter

This guide demonstrates the steps required to implement an adapter. To show what adapter code may look like in practice, we have created a Sample Ad Network SDK and will be building the adapter for this SDK.

3. Initialize the adapter :
On an app's initial rewarded video ad request, the Google Mobile Ads SDK invokes the initialize() method of your adapter.
Your adapter is responsible for implementing this method to initialize the third party ad network. This method provides you a Bundle of serverParameters that may be required to complete the initialization. There is a slight difference in how these parameters can be accessed if you're developing a mediation adapter or a custom event adapter.
For a mediation adapter, the keys for server parameters are preconfigured, and you can extract the server parameters individually. For custom events, there is a single parameter that can be accessed via the MediationRewardedVideoAdAdapter.CUSTOM_EVENT_SERVER_PARAMETER_FIELD key:

MEDIATION ADAPTERCUSTOM EVENTS

.. code-block:: java
    :linenos:

    String parameter1 = serverParameters.getString("SERVER_PARAMETER_KEY1");
    String parameter2 = serverParameters.getString("SERVER_PARAMETER_KEY2");

The adapter should retain a reference to the MediationRewardedVideoAdListener instance so it can forward ad events to the Google Mobile Ads SDK. If initialization succeeds, the adapter should call MediationRewardedVideoAdListener.onInitializationSucceeded(). If adapter initialization fails, the adapter should invoke MediationRewardedVideoAdListener.onInitializationFailed(). In the event that initialization fails, the Google Mobile Ads SDK reattempts to initialize the adapter each time a rewarded video ad request is made. A sample implementation of the initialize() method is shown below:

.. code-block:: java
    :linenos:

    @Override
    public void initialize(Context context,
        MediationAdRequest mediationAdRequest,
        String unused,
        MediationRewardedVideoAdListener listener,
        Bundle serverParameters,
        Bundle mediationExtras) {

        // In this method you should initialize your SDK.

        // The sample SDK requires activity context to initialize, so check that the context
        // provided by the app is an activity context before initializing.
        if (!(context instanceof Activity)) {
            // Context not an Activity context, log the reason for failure and fail the
            // initialization.
            Log.d(TAG, "Sample SDK requires an Activity context to initialize");
            listener.onInitializationFailed(
                    SampleAdapter.this, AdRequest.ERROR_CODE_INVALID_REQUEST);
            return;
        }

        // Get the Ad Unit ID for the Sample SDK from serverParameters bundle.
        String adUnit = serverParameters.getString(
                MediationRewardedVideoAdAdapter.CUSTOM_EVENT_SERVER_PARAMETER_FIELD);

        if (TextUtils.isEmpty(adUnit)) {
            listener.onAdFailedToLoad(this, AdRequest.ERROR_CODE_INVALID_REQUEST);
            return;
        }

        // Create a rewarded video event forwarder to forward the events from the Sample SDK to
        // the Google Mobile Ads SDK.
        mRewardedVideoEventForwarder =
                new SampleMediationRewardedVideoEventForwarder(listener, SampleAdapter.this);

        // Initialize the Sample SDK.
        SampleRewardedVideo.initialize((Activity) context, adUnit, mRewardedVideoEventForwarder);
    }

Don't assume that the context parameter is of type Activity. Depending on publisher implementation, Google Mobile Ads Mediation may forward an application context to your adapter. If your adapter can't handle an application context, it is recommended to invoke onInitializationFailed() with error code AdRequest.ERROR_CODE_INVALID_REQUEST.

.. code-block:: java
    :linenos:

    public class SampleMediationRewardedVideoEventForwarder extends SampleRewardedVideoAdListener {

        ...

        @Override
        public void onRewardedVideoInitialized() {
            super.onRewardedVideoInitialized();
            mIsInitialized = true;
            mMediationRewardedVideoAdListener.onInitializationSucceeded(mSampleAdapter);
        }

        @Override
        public void onRewardedVideoInitializationFailed(SampleErrorCode error) {
            super.onRewardedVideoInitializationFailed(error);
            mIsInitialized = false;
            mMediationRewardedVideoAdListener.onInitializationFailed(
                    mSampleAdapter, getAdMobErrorCode(error));
        }
    }

4. Return initialization state :
The isInitialized() method of your adapter should return false until the adapter's first call to MediationRewardedVideoAdListener.onInitializationSucceeded(). At that point, this method should always return true, as shown below:

.. code-block:: java
    :linenos:

    @Override
    public boolean isInitialized() {
        return mRewardedVideoEventForwarder != null && mRewardedVideoEventForwarder.isInitialized();
    }

5. Load a rewarded video :
Once the adapter invokes the MediationRewardedVideoAdListener.onInitializationSucceeded() method, the Google Mobile Ads SDK can call the loadAd() method of your adapter. This method should request a rewarded video and call MediationRewardedVideoAdListener.onAdLoaded() once it has a rewarded video to show. If no videos are available, the adapter should call MediationRewardedVideoAdListener.onAdFailedToLoad().

For rewarded videos, many ad networks employ an API where there is no notion of ad requests. Once the ad network has been initialized, you can check if there is inventory for the current user and an ad is ready. If this criteria is met, you can show the ad to the user. A sample implementation of the loadAd() for such an ad network is shown below.

.. code-block:: java
    :linenos:

    @Override
    public void loadAd(MediationAdRequest mediationAdRequest,
        Bundle serverParameters,
        Bundle mediationExtras) {
        if (SampleRewardedVideo.isAdAvailable()) {
            // Ad already available, use the forwarder to send a success callback to AdMob.
            mRewardedVideoEventForwarder.onAdLoaded();
        } else {
            // No ad available, use the forwarder to send a failure callback.
            mRewardedVideoEventForwarder.onAdFailedToLoad();
        }
    }

On the first ad request, loadAd() may be called on the adapter immediately after it's invocation of the MediationRewardedVideoAdListener.onInitializationSucceeded() method. If an ad network is still loading ads at this time, immediately reporting an ad failing to load can result in a large proportion of no fills on that initial rewarded video ad request. In this case, we recommend that adapters track when the loading of ads is in progress and delay responding to the Google Mobile Ads SDK until ads have either successfully loaded or failed to load. The Google Mobile Ads SDK has a timeout in place in case the adapter doesn't respond fast enough.

6. Show the rewarded video :
The Google Mobile Ads SDK may call the showVideo() method of your adapter any time after the adapter notifies the Google Mobile Ads SDK of a successful ad load. The showVideo() method will only ever get called once per successful loadAd callback. Upon invocation of this method, the adapter should play the rewarded video.

.. code-block:: java
    :linenos:

    @Override
    public void showVideo() {
        // Show the rewarded video ad.
        if (SampleRewardedVideo.isAdAvailable()) {
            // Rewarded video ad available, show ad.
            SampleRewardedVideo.showAd();
        } else {
            // Show ad will only be called if the adapter sends back an ad loaded callback in
            // response to a loadAd request. If for any reason the adapter is not ready to show
            // an ad after sending an ad loaded callback, log a warning.
            Log.w(TAG, "No ads to show.");
        }
    }

7. Define a reward :
When the adapter is ready to reward the user, it must create an object that conforms to the RewardItem interface. A sample implementation of RewardItem is shown below:

.. code-block:: java
    :linenos:

    /**
    * A {@link RewardItem} that maps the sample reward type and reward amount.
    */
    public class SampleRewardItem implements RewardItem {
        private String mRewardType;
        private int mRewardAmount;

        /**
        * Creates a {@link SampleRewardItem}.
        *
        * @param rewardType   the sample reward type.
        * @param rewardAmount the sample reward amount.
        */
        public SampleRewardItem(String rewardType, int rewardAmount) {
            this.mRewardType = rewardType;
            this.mRewardAmount = rewardAmount;
        }

        @Override
        public String getType() {
            return mRewardType;
        }

        @Override
        public int getAmount() {
            return mRewardAmount;
        }
    }

To notify the Google Mobile Ads SDK that a user should be rewarded, the adapter is required to invoke the MediationRewardedVideoAdListener.onRewarded() method with an instance of RewardItem as an argument.

8. Forward ad events to Google Mobile Ads SDK :
Your adapter is required to notify the Google Mobile Ads SDK when any of the following ad events occur:

MediationRewardedVideoAdListener method	Call when
onAdOpened()	A full screen overlay covers the app content
onVideoStarted()	The video ad starts
onAdClicked()	The video ad is clicked
onAdLeftApplication()	The user leaves the application due to the video ad (for example, to go to the browser)
onRewarded()	The video ad rewards the user
onAdClosed()	The video ad is closed
An example implementation of ad forwarding to the Google Mobile Ads SDK is shown below:

.. code-block:: java
    :linenos:

    /**
    * A {@link SampleRewardedVideoAdListener} that forwards events to AdMob mediation's {@link }.
    */
    public class SampleMediationRewardedVideoEventForwarder extends SampleRewardedVideoAdListener {
        private MediationRewardedVideoAdListener mMediationRewardedVideoAdListener;
        private SampleAdapter mSampleAdapter;
        private boolean mIsInitialized;

        /**
        * Creates a new {@link SampleMediationRewardedVideoEventForwarder}.
        *
        * @param listener      An AdMob Mediation {@link MediationRewardedVideoAdListener} that should
        *                      receive forwarded events.
        * @param sampleAdapter A {@link SampleAdapter} mediation adapter.
        */
        public SampleMediationRewardedVideoEventForwarder(MediationRewardedVideoAdListener listener,
        SampleAdapter sampleAdapter) {
            this.mMediationRewardedVideoAdListener = listener;
            this.mSampleAdapter = sampleAdapter;
        }

        /**
        * @return whether or not the Sample SDK is initialized.
        */
        public boolean isInitialized() {
            return mIsInitialized;
        }

        @Override
        public void onRewardedVideoInitialized() {
            super.onRewardedVideoInitialized();
            mIsInitialized = true;
            mMediationRewardedVideoAdListener.onInitializationSucceeded(mSampleAdapter);
        }

        @Override
        public void onRewardedVideoInitializationFailed(SampleErrorCode error) {
            super.onRewardedVideoInitializationFailed(error);
            mIsInitialized = false;
            mMediationRewardedVideoAdListener.onInitializationFailed(
            mSampleAdapter, getAdMobErrorCode(error));
        }

        @Override
        public void onAdRewarded(final String rewardType, final int amount) {
            super.onAdRewarded(rewardType, amount);

            /*
            * AdMob requires a reward item with a reward type and amount to be sent when sending the
            * rewarded callback. If your SDK does not have a reward amount you need to do the following:
            *
            * 1. AdMob provides an ability to override the reward value in the front end. Document
            * this asking the publisher to override the reward value on AdMob's front end.
            * 2. Send a reward item with default values for the type (an empty string "") and reward
            * amount (1).
            */
            mMediationRewardedVideoAdListener.onRewarded(
                    mSampleAdapter, new SampleRewardItem(rewardType, amount));
        }

        @Override
        public void onAdClicked() {
            super.onAdClicked();
            mMediationRewardedVideoAdListener.onAdClicked(mSampleAdapter);
        }

        @Override
        public void onAdFullScreen() {
            super.onAdFullScreen();
            mMediationRewardedVideoAdListener.onAdOpened(mSampleAdapter);
            // Only send video started here if your SDK starts video immediately after the ad has been
            // opened/is fullscreen.
            mMediationRewardedVideoAdListener.onVideoStarted(mSampleAdapter);
        }

        @Override
        public void onAdClosed() {
            super.onAdClosed();
            mMediationRewardedVideoAdListener.onAdClosed(mSampleAdapter);
        }

        /**
        * Forwards the ad loaded event to AdMob SDK. The Sample SDK does not have an ad loaded
        * callback, the adapter calls this method if an ad is available when loadAd is called.
        */
        protected void onAdLoaded() {
            mMediationRewardedVideoAdListener.onAdLoaded(mSampleAdapter);
        }

        /**
        * Forwards the ad failed event to AdMob SDK. The Sample SDK does not have an ad failed to
        * load callback, the adapter calls this method to forward the failure callback.
        */
        protected void onAdFailedToLoad() {
            mMediationRewardedVideoAdListener.onAdFailedToLoad(
            mSampleAdapter, AdRequest.ERROR_CODE_NO_FILL);
        }

        /**
        * Converts {@link SampleErrorCode} into an AdMob SDK's {@link AdRequest} error code.
        *
        * @param errorCode a Sample SDK error code.
        * @return an AdMob SDK readable error code.
        */
        private int getAdMobErrorCode(SampleErrorCode errorCode) {
            switch (errorCode) {
                case BAD_REQUEST:
                return AdRequest.ERROR_CODE_INVALID_REQUEST;
            case NETWORK_ERROR:
                return AdRequest.ERROR_CODE_NETWORK_ERROR;
            case NO_INVENTORY:
                return AdRequest.ERROR_CODE_NO_FILL;
            case UNKNOWN:
            default:
                return AdRequest.ERROR_CODE_INTERNAL_ERROR;
            }
        }
    }

Third party ad networks likely have ad events listeners for their ad network. Ad events from the third party SDK should be mapped to corresponding ad events for the MediationRewardedVideoAdListener provided in the initialize() method of the adapter and invoked at the appropriate time.

9. Notification of context changes :
Your adapter can optionally implement the OnContextChangedListener to be notified of a change to the current context. The onContextChanged method is invoked when there is an update to the RewardedVideoAd object's current context. The updated activity context can be accessed as shown below:

.. code-block:: java
    :linenos:

    @Override
    public void onContextChanged(Context context) {
        if (context instanceof Activity) {
            SampleRewardedVideo.setCurrentActivity((Activity) context);
        }
    }
