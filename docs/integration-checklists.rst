Integration Checklists
===========================================================

✔ **Add Your App**
    Log in to the Ogury platform and add your app. Don't forget to copy your app_key for the next steps.

✔ **Set Up Ad Units (optionnal)**
    Define your ad units and placements on the Ad Units page. If you choose optin video ad unit, don't forget to set the item name and reward amount.

✔ **Add the SDK to Your Project**
    Add the Ogury SDK to your project through Manual Download

✔ **Update AndroidManifest.xml**
    Add the necessary permissions and activities to your AndroidManifest

✔ **Set the Listeners**
    Register to the Listeners of the ad units you set up earlier on the Ogury platform

✔ **Set UserID (Optin video only)**
    If using server-to-server callbacks to reward your users (for Optin Video), you must set the UserID. In addition, the userID parameter must be set before you make the init request.

✔ **Init the SDK**
    Initialize the SDK using the app key generated on the Ogury platform. We recommend referring to the ad units you set up earlier in the ad unit parameter like this Presage.init(this, appKey, YOUR_AD_UNITS);

 ✔ **Integrate Ad Units**
    Integrate one of our ad units like Interstitials or Optin Video in order to finish your monetization integration