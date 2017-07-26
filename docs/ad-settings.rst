Ad Settings
===========================================================

.. _ad-placements:

======================================
Ad Placements
======================================

************
The Basics
************
A placement is a specific area in your application where you might want to display an Ogury ad to your users. 
Typical Placements include Interstitials and Optin Video. All Ogury ads are displayed via Placements in the application. 

Placement configuration are optionnal if you only want to display Interstitials ads so you don't need to set an ad unit id within interstitial placement object. But for Optin Video, placement configuration is required in order for this ad unit to work properly and you will need a proper ad unit id coming from Ogury admin for the optin video object in code.

Placements can be contextual, triggered by the user’s natural progression through the application. 
For example, when your application starts, it’s a good idea to call an AppOpen placement, as this is often a good place to display content to your users. 
Placements can also be user-initiated: a "Watch Videos to get Reward!" button that triggers a WatchVideoForReward Placement.

Once you create a Placement, you can change some options for that Placement with the Ogury Dashboard without having to alter your application’s code.

****************************
Creating a placement
****************************


***********
Next steps
***********

Add, edit, and remove managed placements.

======================================
Capping / Limit
======================================

======================================
Ads Options
======================================

======================================
Server to Server Callbacks
======================================
