#Table of Content
- [1. System Requirements](#user-content-1-system-requirements)
- [2. SDK Initialization](#user-content-2-sdk-initialization)
	- [a. Import Files](#user-content-a-import-files)
	- [b. Link the Static Library](#user-content-b-link-the-static-library)
	- [c. Initialize the SDK](#user-content-c-initialize-the-sdk)
	- [d. Advanced Settings](#user-content-d-advanced-settings)
- [3. Feature Activation](#user-content-3-feature-activation)
	- [a. Analytics Activation](#user-content-a-analytics-activation)
		- [i. Automatic Events](#user-content-i-automatic-events)
		- [ii. Standard Events](#user-content-ii-standard-events)
		- [iii. Custom Events](#user-content-iii-custom-events)
		- [iv. Best Practices](#user-content-iv-best-practices)
	- [b. Push Notification Activation](#user-content-b-push-notification-activation)
		- [i. Initialization](#user-content-i-initialization)
		- [ii. Setup Apple Push Notification Services](#user-content-ii-setup-apple-push-notification-services)
		- [iii. Segment your Audience](#user-content-iii-segment-your-audience)
	- [c. Attribution Tracking Activation](#user-content-c-attribution-tracking-activation)
		- [i. Track RadiumOne Campaigns](#user-content-i-track-radiumone-campaigns)
		- [ii. Track 3rd party Campaigns](#user-content-ii-track-3rd-party-campaigns)
		
// TODO Update TOC and links above AFTER review completion


#1. System Requirements
The R1 Connect SDK supports all mobile and tablet devices running Android 2.2 and above. The downloadable directory (see below "[a. Import Files](#a-import-files)") contains the library and headers of the R1 Connect SDK for Android. 

// TODO Update the download link above.

The library supports the following architectures:

*     	arm7
*	arm7s
*	arm64
*	i386
*	x86_64

// TODO Yuriy - Verify the supporting architectures.


#2. SDK Initialization

## a. Import the library to your project

Please start by downloading the demo project:
git clone https://github.com/radiumone/r1-connect-demo-Android

Then copy SdkLib/LibR1Connect.jar into your project. All mobile and tablet devices running Android 2.2. and above are supported.

You will need to have created the app you will be using in R1 Connect.


## b. Initialize the SDK in all Activities and Application Classes
The following steps will explain how to integrate with R1 Connect to enable event tracking.

<img src="https://raw.github.com/radiumone/r1-connect-demo-Android/master/readme-images/image1.png” width="440" />

After copying the lib files, import the emitter in all your application activities:

```java
import com.radiumone.emitter.R1Emitter;
```

And override onStart and onStop methods:

```java
@Override
protected void onStart() {
// Auto-generated method stub super.onStart();
R1Emitter.getInstance().onStart(this);
}
￼
@Override
protected void onStop() {
// Auto-generated method stub super.onStop();
R1Emitter.getInstance().onStop(this);
}
```

Then create a class that extends the Application class (or use an existing one), and initialize the SDK in the onCreate method:

```java
package com.example.yourpackagename; 
import com.radiumone.emitter.R1Emitter; 
import android.app.Application;

public class TestApplication extends Application{
@Override
public void onCreate() {
super.onCreate();
R1Emitter.getInstance().connect(this); 
}
}
```

## c. Configure the SDK

Next, to configure how the library will be used in your project you will need to create a file called r1connect.properties in the folder called “assets”.

<img src="https://raw.github.com/radiumone/r1-connect-demo-Android/master/readme-images/image2.png” width="440" />

```java
app_id=<YOUR APPLICATION_ID> 
client_key=<YOUR CLIENT KEY>

# default value for push when application starts. Default true
enable_push=true

#allow location tracking by gps.
enable_gps=true

#location update timeout. Default 600 sec
location_update_time=600

#location update distance. Default 100 meters
location_update_distance=100

#allow location tracking in background. Default false
location_in_background=false
```

As you can see in the example above, it will contain the following:

• app_id – You will need to enter the App ID you received when creating your app on R1 Connect (it’s found under Dev Tools -> Keys & Secrets)

• 	client_key – You will need to enter the App Key you received when creating your app on R1 Connect (it ‘s found under Dev Tools -> Keys & Secrets)

• 	disable_sdk_location - when set to “true” it disables the use of sdk tracking location. It is useful if you want to use your own tracking location. You can pass a location object like so:
R1Emitter.getInstance().trackLocation(location);

• 	enable_ gps – when set to “true” it enables the use of device GPS to get location (only if GPS is enabled in device settings), when “false” only the network is used

• 	location_update_time – this is the timeout between location updates

• 	location_update_distance – you can set the change in distance for location updates

• 	location_in_background – you can set whether or not the locatio
n is allowed to be sent while the app is in the background

## d. Update your manifest
Finally, in your manifest, add proper permissions:

```java
<uses-permission android:name="android.permission.INTERNET" /> 
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" /> 
<uses-permission android:name="android.permission.READ_PHONE_STATE" /> 
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" /> 
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

And make sure that your Application class is properly declared:

```java
<application
android:name="com.example.yourpackagename.TestApplication" 
android:allowBackup="true"
…
<service android:name="com.radiumone.emitter.location.LocationService"/>
</application>
```

## e. Parameters
The following is a list of configuration parameters for the R1 Connect SDK, most of these contain values that are sent to the tracking server to help identify your app on our platform and to provide analytics on sessions and location.

**appName**

The application name associated with emitter. By default, this property is populated with the package name of the application. If you wish to override this property, you must do so before making any tracking calls.

```java
R1Emitter.getInstance().setApplicationName("customApplicationName");
```

**appId**

The application identifier associated with this emitter. By default, this property is null. If you wish to set this property, you must do so before making any tracking calls. Note that this is not your app's bundle id (e.g. com.example.appname).

```java
R1Emitter.getInstance().setApplicationUserId("12345");
```

**appVersion**

The application version associated with this emitter. By default, this property is populated with the android:versionName= string from the application AndroidManifest.xml. If you wish to override this property, you must do so before making any tracking calls.

```java
R1Emitter.getInstance().setAppVersion("1.0");
```

**appScreen**

The current application screen set for this emitter.

```java
R1Emitter.getInstance().emitAppScreen("appScreen", "contentDescription", "documentLocationUrl", "documentHostName", "documentPath", parameters);
// where parameters is a HashMap. Example:
private HashMap<String, Object> parameters = new HashMap<String, Object>();
parameters.put("key","value");
```

**sessionStart**

// TODO Yuriy - Are we making sessionStart not user-configurable?  If so, we should remove this module.

If true, indicates the start of a new session. Note that when a emitter is first instantiated, this is initialized to true. To prevent this default behavior, set sessionTimeout to negative value. By itself, setting this does not send any data. If this is true, when the next emitter call is made, a parameter will be added to the resulting emitter information indicating that it is the start of a session, and this flag will be cleared.

```java
R1Emitter.getInstance().setSessionStarted(true);
// Your code here
R1Emitter.getInstance().setSessionStarted(false);
```

**sessionTimeout**

Indicates how long, in seconds, the application must transition to the inactive or background state before the tracker automatically indicates the start of a new session. When this happens and the app becomes active again it will set sessionStart to true. For example, if this is set to 30 seconds, and the user receives a phone call that lasts for 45 seconds while using the app, upon returning to the app, the sessionStart parameter will be set to true. If the phone call instead lasted 10 seconds, sessionStart will not be modified. By default, this is 30 seconds.

```java
R1Emitter.getInstance().setSessionTimeout(30);
```

**applicationUserID**

Optional current user identifier.

```java
R1Emitter.getInstance().setApplicationUserId("12345");
```









#3. Feature Activation
##a. Analytics Activation
### i. Automatic Events

The R1 Connect SDK will automatically capture some generic events, but in order to get the most meaningful data on how users interact with your app the SDK. These events are triggered when the state of the application is changed and, therefore, they do not require any additional code to be written in the app in order to work out of the box:

**Launch** - emitted when the app starts

**First Launch** - emitted when the app starts for the first time

**First Launch After Update** - emitted when the app starts for the first time after a version upgrade

**Suspend** - emitted when the app is put into the background state

// TODO Yuriy - Is Suspend event available on Android?

**Resume** - emitted when the app returns from the background state

// TODO Yuriy - Is Resume event available on Android?

**Application Opened** - This event is very useful for push notifications and can measure when your app is opened after a message is sent.

**Session Start** - As the name implies the Session Start event is used to start a new session.

**Session End** - The Session End event is used to end a session and passes with it a Session Length attribute that calculates the session length in seconds.


### ii. Standard Events

Standard Events give you an easy way to cover all the main user flows (login, register, share, purchase...) in a standardized format for optimized reporting on the portal. They provide some great foundation for your analytics strategy. Once you set them up in your code, they unlock great insights, especially on user lifetime value.

*Note: The last argument in all of the following emitter callbacks, otherInfo, is a dictionary of “key”,”value” pairs or nil, which enables you to customize these events as much as you want.*

**Login**

Tracks a user login within the app

```java
R1Emitter.getInstance().emitLogin( sha1("userId"), "userName", parameters);
```

**Registration**

Records a user registration within the app

```java
UserItem userItem = new UserItem();
userItem.userId = sha1("userId");
userItem.userName = "userName";
userItem.city = "City";
userItem.state = "State"
R1Emitter.getInstance().emitRegistration(userItem, parameters);
```

**Facebook connect**

Allows access to Facebook services

```java
HashMap<String, Object> parameters = new HashMap<String, Object>();
parameters.put("custom_key","value");
R1Emitter.getInstance().emitFBConnect(permissions, parameters);
```

where permissions is a List of R1SocialPermissions:

```java
ArrayList<R1SocialPermission> socialPermissions = new ArrayList<R1SocialPermission>();
socialPermissions.add( new R1SocialPermission("permission", true));
```

**Twitter connect**

Allows access to Twitter services

```java
HashMap<String, Object> parameters = new HashMap<String, Object>();
parameters.put("custom_key","value");
R1Emitter.getInstance().emitTConnect( sha1( "userId" ),
“username”, permissions, parameters);
```

where permissions is a List of R1SocialPermissions:

```java
ArrayList<R1SocialPermission> socialPermissions = new ArrayList<R1SocialPermission>();
socialPermissions.add( new R1SocialPermission("permission", true));
```

**User Info**

This event enables you to send user profiles to the backend.

```java
R1EmitterUserInfo *userInfo = [R1EmitterUserInfo userInfoWithUserID:@"userId"
                           userName:@"userName"
                              email:@"user@email.com"
                          firstName:@"first name"
                           lastName:@"last name"
                      streetAddress:@"streetAddress"
                              phone:@"phone"
                               city:@"city"
                              state:@"state"
                                zip:@"zip"];

[[R1Emitter sharedInstance] emitUserInfo:@"userId"
                               otherInfo:@{"custom_key":"value"}];
```

// TODO Yuriy - Update **User Info** sample code above

**Upgrade**

Tracks an application version upgrade

```java
[[R1Emitter sharedInstance] emitUpgradeWithOtherInfo:@{"custom_key":"value"}];
```

// TODO Yuriy - Update **Upgrade** sample code above

**Trial Upgrade**

Tracks an application upgrade from a trial version to a full version

```java
R1Emitter.getInstance().emitTrialUpgrade(parameters);
```

**Screen View**

Basically, a page view, it provides info about that screen

```java
R1Emitter.getInstance().emitAppScreen("title","description","http://    www.example.com/path”,”example.com”,”path”,parameters);
```

**Transaction**

```java
EmitItem purchaseItem = new EmitItem();
purchaseItem.storeId = "storeId";
purchaseItem.storeName = "name";
purchaseItem.transactionId = "AE3237DAA"
purchaseItem.cartId = "ABBCCD"
purchaseItem.orderId = "ABCDEF";
purchaseItem.totalSale = 3.2f;
purchaseItem.currency = "EUR";
purchaseItem.shippingCosts = 1.8f;
purchaseItem.transactionTax = 2.5f;
R1Emitter.getInstance().emitTransaction(emitItem, parameters);
```

**TransactionItem**

```java
R1EmitterLineItem lineItem = new R1EmitterLineItem();
lineItem.productId = "productId";
lineItem.productName = "productName";
lineItem.quantity = 5;
lineItem.unitOfMeasure = "parts";
lineItem.msrPrice = 1.3f;
lineItem.pricePaid = 3.4f;
lineItem.currency = "EUR;
lineItem.itemCategory = "items";
R1Emitter.getInstance().emitTransactionItem("transactionItemId", lineItem,  parameters);
```

**Create Cart**

```java
R1Emitter.getInstance().emitCartCreate("cartId", parameters);
```

**Delete Cart**

```java
R1Emitter.getInstance().emitCartDelete("cartId", parameters);
```

**Add To Cart**

```java
R1EmitterLineItem lineItem = new R1EmitterLineItem();
lineItem.productId = "productId";
lineItem.productName = "productName";
lineItem.quantity = 5;
lineItem.unitOfMeasure = "parts";
lineItem.msrPrice = 1.3f;
lineItem.pricePaid = 3.4f;
lineItem.currency = "EUR;
lineItem.itemCategory = "items";
R1Emitter.getInstance().emitAddToCart("cartId", lineItem, parameters);
```

**Delete From Cart**

```java
R1EmitterLineItem lineItem = new R1EmitterLineItem();
lineItem.productId = "productId";
lineItem.productName = "productName";
lineItem.quantity = 5;
lineItem.unitOfMeasure = "parts";
lineItem.msrPrice = 1.3f;
lineItem.pricePaid = 3.4f;
lineItem.currency = "EUR;
lineItem.itemCategory = "items";
R1Emitter.getInstance().emitRemoveFromCart("cartId", lineItem, parameters);
```


###iii. Custom Events



With custom events you have the ability to create and track specific events that are more closely aligned with your app. If planned and structured correctly, custom events can be strong indicators of user intent and behavior. Some examples include pressing the “like” button, playing a song, changing the screen mode from portrait to landscape, and zooming in or out of an image. These are all actions by the user that could be tracked with events.

To include tracking of custom events for the mobile app, the following callbacks need to be included in the application code:

```java
// Emits a custom event without parameters
R1Emitter.getInstance().emitEvent("Your custom event name");
// Emits a custom event with parameters
private HashMap<String, Object> parameters = new HashMap<String, Object>();
parameters.put("key","value");
R1Emitter.getInstance().emitEvent("Your custom event name", parameters);
```




###iv. Best Practices
####Event Naming Convention
One common mistake is to parametrize event names (with user data for example). Event names should be hard-coded values that you use to segement data on a specific category of event. 

Example: "ProfileViewing"

Avoid: "Profile Viewing - Lady Gaga's profile"

As you may have thousands of user profiles in your database, it is preferable to keep the event name high level ("ProfileViewing") so you can run interesting anaytics on it. For example, it will be much easier to answer this question: "how many profiles does a user visit every day on average?". 

####Parameter Variance

Another common mistake is to add parameters to the event that have too many possible values. To follow up on the previous example, one may decide to add the number of profile followers as an event parameter:

```java
[[R1Emitter sharedInstance] emitEvent:@"ProfileViewing"
			withParameters:@{"profileFollowers":profileFollowers}];
```

// TODO Update the sample code above
			  			   
Again, the problem here is that each profile may have any number of followers. This will result in having your data much too fragmented to extract any valuable information.

Instead, a good strategy would be to define relevant buckets to replace high variance parameters. For example, in this case, it might be more relevant to separate traffic on the profiles with a lot of followers from traffic on frofiles with very few followers. You could define 3 categories: 

- "VERY_INFLUENTIAL" for profiles > 100,000 
- "INFLUENTIAL" for profile > 10,000 and <= 100,000
- "NON_INFLUENTIAL" for profile <= 10,000

Then a proper event would be 

```java
[[R1Emitter sharedInstance] emitEvent:@"ProfileViewing"
			withParameters:@{"profileFollowersBucket":@"VERY_INFLUENTIAL"}];
```

// TODO Update the sample code above
			  			   
This will enable you to create much more insightful reports.

##b. Push Notification Activation

###i. Initialization

To make sure push notifications work correctly, please follow these steps:

Configure the r1connect.properties file in your project:

• sender_id – You will need to enter the project number you received when creating the Google API project.

• enable_push – This defaults to “true” and it will enable push notifications or disable push notifications after you start your application. 


Create a class that inherits from the class Application (or you can use an existing one in the project)

To enable an action such as opening the app when a notification is clicked, create a class that inherits from BroadcastReceiver and add the necessary logic to it. If you are okay with the default, which closes the notification upon pressing it, then no further coding is required.

// TODO Yuriy - Insert sample code from the screenshot: https://raw.github.com/radiumone/r1-connect-demo-Android/master/readme-images/image3.png

The class referred to in the first item is used in the following way:

```java
R1Emitter.getInstance().setNotificationIconResourceId(this, R.drawable.ic_launcher);
//The above line is necessary for creating an icon in the notification bar when the device receives the notification
R1Emitter.getInstance().setIntentReceiver(this, TestPushReceiver.class);
//This line tells the library that the class created in step 2 will be processing the push notification
//In TestPushReceiver (see step 2) we want to open ShowNotificationActivity when notification is clicked
R1Emitter.getInstance().connect(this); //To make sure the library works correctly it is necessary this line in onCreate() method
```

// TODO Yuriy - Insert sample code from the screenshot: 
https://raw.github.com/radiumone/r1-connect-demo-Android/master/readme-images/image4.png

If you want to create your own notifications you have to create a class that implements R1NotificationBuilder interface and write your notification builder like in the example below:

// TODO Yuriy - Insert sample code from the screenshot: 
https://raw.github.com/radiumone/r1-connect-demo-Android/master/readme-images/image7.png

After that add this line just before R1Emitter.getInstance().connect(this) in your application class:
```java
R1Emitter.getInstance().setNotificationBuilder( new CustomNotificationBuilder());
```

To make sure the library works correctly it is also necessary to include the following in onStart and onStop methods in all your application Activities:

// TODO Yuriy - Insert sample code from the screenshot: 
https://raw.github.com/radiumone/r1-connect-demo-Android/master/readme-images/image5.png

// TODO Yuriy - Insert sample code from the screenshot: 
https://raw.github.com/radiumone/r1-connect-demo-Android/master/readme-images/image6.png

In the manifest you need to create the following:

```java
<!-- android:name of application tag must be full application name that was created in first step.-->
<application
    android:name="com.example.r1connecttestapplication.TestApplication"
    android:allowBackup="true"
    android:icon="@drawable/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/AppTheme" >
    <!—the next lines are your project activities-->
    //Then there are necessary fields for the library to work correctly (cont…):
    <receiver android:name="com.radiumone.emitter.gcm.R1GCMPushReceiver" android:exported="true"
              android:permission="com.google.android.c2dm.permission.SEND" >
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <!-- name must be your applicationPackage -->
            <category android:name="com.radiumone.sdk" />
        </intent-filter>
    </receiver>
    <receiver android:name="com.radiumone.emitter.push.R1PushBroadcastReceiver"
              android:exported="false">
        <intent-filter>
            <action android:name="com.radiumone.r1push.OPENED_INTERNAL"/>
        </intent-filter>
    </receiver>
    <service android:name="com.radiumone.emitter.push.R1ConnectService"/>
    <service android:name="com.radiumone.emitter.location.LocationService"/>
    //Using the class created in Step 2 (cont…):
    <receiver android:name=".testpush.TestPushReceiver"
              android:exported="false">
        <intent-filter>
            <action android:name="com.radiumone.r1push.OPENED"/>
        </intent-filter>
    </receiver>
</application>
//Permissions that are necessary for the library to work:
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
//Permission to get location when using the network
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
//Permission to get location when using GPS
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.WAKE_LOCK"></uses-permission>
<uses-permission android:name="android.permission.GET_ACCOUNTS" />
<uses-permission android:name="android.permission.VIBRATE"/>
<!—(your_application_package).permission.C2D_MESSAGE -->
<permission android:name="com.radiumone.sdk.permission.C2D_MESSAGE" android:protectionLevel="signature"/>
<uses-permission android:name="com.radiumone.sdk.permission.C2D_MESSAGE"></uses-permission>
```


###ii. Setup Google Cloud Messaging

####Prerequisites for Google Cloud Messaging  Setup
This doc assumes you have already set up Google Play Services in your application project. Google Play Services is needed to use Google Cloud Messaging (GCM), the notification gateway RadiumOne Connect will use for your Android app. Also if you have not done so already, you will need to add the app you will be using to the RadiumOne Connect portal in order to add the Google API Key to it once you have generated the API Key following the steps below.

####Configuring your App for GCM
######Obtain an API key from Google
In order to use RadiumOne Connect with your application you will need an API key from Google. We will summarize those steps here, but for more info on this process please visit “GCM Getting Started” here.
1.	Create a Google API project in Google APIs Console (take note of your project number which is the value after #project: it will be used later as your GCM sender ID)

// TODO Insert a screenshot image

2.	Enable the GCM Service

// TODO Insert a screenshot image

3.	Create a new Server key in the Google APIs Console page under API Access

// TODO Insert two screenshot images

4.	Copy the key, it is used for GCM Connection Servers and for RadiumOne Connect setup

######Setting up the API Key on R1 Connect

1.	Please make sure you are signed into your account on RadiumOne Connect and go to the application you want to add this API key to.

2.	Next, in the side menu go to Dev Tools > Push Services > Google Cloud Messaging.

// TODO Insert a screenshot image

3.	Add your API Key to the API Key field and click Save.

// TODO Insert a screenshot image

If it saved correctly you will see a green badge with a white checkmark in it. Now your app is setup with GCM.




###iii. Segment your Audience    

You can specify Tags for *R1 Connect SDK* to send *Push Notifications* for certain groups of users.

The maximum length of a Tag is 128 characters.

*R1 Connect SDK* saves Tags. You do not have to add Tags every time the application is launched.

***Add a new Tag***

```java
R1Push.getInstance(context).addTag("tag");
```

***Add multiple Tags***
	
```java
[[R1Push sharedInstance].tags addTags:@[ @"NEW TAG 1", @"NEW TAG 2" ]];
```

// TODO Yuriy - Replace the code above for Add multiple tags

***Remove existing Tag***

```java
R1Push.getInstance(context).removeTag("tag");
```

***Remove multiple Tags***

```java
[[R1Push sharedInstance].tags removeTags:@[ @"EXIST TAG 1", @"EXIST TAG 2" ]];
```

// TODO Yuriy - Replace the code above for Remove multiple tags

***Replace all existing Tags***

```java
[R1Push sharedInstance].tags.tags = @[ @"NEW TAG 1", @"NEW TAG 2" ];
```
or
```java
[[R1Push sharedInstance].tags setTags:@[ @"NEW TAG 1", @"NEW TAG 2" ]];
```

// TODO Yuriy - Replace the code above for Replace all existing tags

***Get all Tags***
	
```java
String[] allTags = R1Push.getInstance(context).getTags(context);
```



##c. Attribution Tracking Activation
###i. Track RadiumOne Campaigns
Please contact your Account Manager to setup R1 ad campaign as well as tracking campaigns.  If you don't have one, please contact us  [here](http://radiumone.com/contact-mobile-team.html) and one of our Account Managers will assist you.

Once your Account Manager has set up tracking, you will start receiving attribution tracking report automatically!

###ii. Track 3rd party Campaigns
1. Please contact your Account Manager to setup tracking URLs for your 3rd party campaigns.  If you don't have one, please contact us [here](http://radiumone.com/contact-mobile-team.html) and one of our Account Managers will assist you.
2. Send the list of all your media suppliers (anyone you run a mobile advertising campaign with).
3. For each media supplier, your account manager will send you 2 tracking URLs (one impression tracking URL, 1 click tracking URL).
4. Send each pair of URLs to the relevant Media Supplier so they can set these tracking URLs on the creatives
5. You're all set and will start having access to Attribution Tracking Reports


