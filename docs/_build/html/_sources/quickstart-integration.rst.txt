
.. _quickstart-integration:

***************************************
Quick start Integration
***************************************

.. note::
    We support Android Operating Systems Version 4.0 (API level 14) and up.

.. seealso::
    If you are using other plugins like Unity, Adobe Air or Cordova, you can go to the :ref:`plugins` page to follow specific plugins integration.


:ref:`step1`
:ref:`step2`
:ref:`step3`
:ref:`step4`



.. _step1:

===========================================
Step 1 - Add the Ogury SDK to your project
===========================================

**Integrate the SDK through manual download**

1- Download the SDK on the :ref:`download-links` page, unzip it and drop the presage.jar into the libs folder in your project.


2- Make sure you add the following to your build.gradle file under the dependencies section:

.. code-block:: xml
    :linenos:

    compile fileTree(dir: 'libs', include: ['*.jar'])

.. _step2:


============================================
Step 2 - Update your AndroidManifest.xml
============================================

Put this code between your <application> tags.

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

Don't forget Internet Permissions !

.. code-block:: xml
    :linenos:

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

.. _step3:

=======================================
Step 3 - Initialize the SDK
=======================================


Add the following import

.. code-block:: java
    :linenos:
    
    import io.presage.Presage;

Inside your activity, in your onCreate method, add:

.. code-block:: java
    :linenos:

    Presage.getInstance().setContext(this.getBaseContext());
    Presage.getInstance().start();

.. _step4:

===========================================
Step 4 - Proguard configuration (Optional)
===========================================

.. note::
    If you use Proguard, you need to add these lines in your configuration

.. code-block:: xml
    :linenos:

    # ---- PRESAGE - start

    -dontnote io.presage.**
    -dontwarn shared_presage.**
    -dontwarn org.codehaus.**

    -keepattributes Signature

    -keep class shared_presage.** { *; }
    -keep class io.presage.** { *; }
    -keepclassmembers class io.presage.** {
    *;
    }

    -keepattributes *Annotation*
    -keepattributes JavascriptInterface
    -keepclassmembers class * {
        @android.webkit.JavascriptInterface <methods>;
    }

    # ---- OKHTTP
    -dontnote okhttp3.**
    -dontnote okio.**
    -dontwarn okhttp3.**
    -dontwarn okio.**

    -dontwarn javax.annotation.Nullable
    -dontwarn javax.annotation.ParametersAreNonnullByDefault

    -dontnote sun.misc.Unsafe
    -dontnote android.net.http.*

    -dontnote org.apache.commons.codec.**
    -dontnote org.apache.http.**

    -dontwarn org.apache.commons.collections.BeanMap
    -dontwarn java.beans.**

    # ---- GOOGLE
    -dontnote com.google.gson.**
    -dontnote com.google.android.gms.ads.**
    -dontnote com.google.android.**
    -dontnote com.google.ads.**

    -keepclassmembers class * implements java.io.Serializable {
        static final long serialVersionUID;
        private static final java.io.ObjectStreamField[] serialPersistentFields;
        private void writeObject(java.io.ObjectOutputStream);
        private void readObject(java.io.ObjectInputStream);
        java.lang.Object writeReplace();
        java.lang.Object readResolve();
    }

    # ---- PRESAGE - end


Done! You just integrated the Ogury SDK in your app. 
You are now ready to start working with Ogury’s Ad Units and Mediation Tools.