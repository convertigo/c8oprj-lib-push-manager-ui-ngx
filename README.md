# lib_PushManager_Ngx
This is the Firebase Cloud Messaging (FCM) Push notifications library for the Convertigo Low Code Platform. This library brings the server part and the client part for Firebase Push Notifications support for Convertigo mobile builder apps. 

## Setting up the Firebase Service

1. Connect to the firebase console : [https://console.firebase.google.com/](https://console.firebase.google.com/)
2. Sign in with your google account and add a project by clicking on the "+" (add project)
3. Give your project a name, hit "Continue", Disable or enable google Analytics and hit "Continue"
4. Wait for the resources to be provisioned, Hit "Continue", you will be on the firebase overview page
5. Click on the Gear icon next to "Project Overview" -> "Project settings" to configure your Firebase  project
6. Add your iOS and Android app by clicking on the iOS or Android icons. When you add your apps be sure to configure a valid bundle id (iOS) or Android package name (Android) and use the same ids for both platforms, for example __"com.mycompany.myapp"__. The id configured here will be used later on to configure the __MobileApplication__ object in Convertigo project.
7. Download the config files, this will be the __GoogleService-info.plist__ (iOS) or the __google-services.json__ (Android). These files will have to be included in your app and described in the next section.
8. You can ignore next steps for Convertigo Mobile Builder applications. for Convertigo SDK Applications, follow the steps to include the FCM SDK in your app.

## Getting your server key

Firebase services automatically generated a Server key. You will need it to Configure the Convertigo Push manager Library. To get your server key:

1. Click "settings" in top header of the projet, then click on the "Cloud Messaging" tab.
2. Copy the "Server Key" and paste it to the Convertigo global Symbol named **lib_PushManager_Ngx.serverKey** in Convertigo Administration console.

## Configure your iOS APNS notifications (Only if you want to do iOS notifications)

FCM supports APNS notification for iOS. You will have to configure your APNS Auth key. This is done in the "Cloud Messaging' tab under the "iOS app configuration" section (this section will be shown only if you added an iOS app to your FCM project). To get your APNS Auth key, you need an Apple developer account and connect to :  

[https://developer.apple.com/account/resources/authkeys/add](https://developer.apple.com/account/resources/authkeys/add)

Add an APNS key, give it a name and be sure to know your team id (shown in upper right corner of the apple developer portal)

Once the key is generated, download the .P8 file, and upload it in the FCM "iOS app configuration" section with its name and TEAM ID.

## Client Part configuration

The library provides a __FCMPushNotifications__ Convertigo Shared action you must add in your client application. To do so :

1. In your Mobile Builder application add a [Project Reference](https://www.convertigo.com/documentation/latest/reference-manual/convertigo-objects/common/references/schema-references/project-reference/) (Right click on the Convertigo project root and follow New->Reference->Project reference). Choose any template as it will be edited in the next step.
2. On the project reference property click on the __"Project name and remote URL"__ property '...' to edit the property
3. Paste in the "Project Remote URL" field this reference :

		lib_PushManager_Ngx=https://github.com/convertigo/c8oprj-lib-push-manager-ngx.git
 
	  
	The library will be downloaded from github and installed in your Convertigo workspace.
4. Now invoke the shared action in your client app to register and start receiving push notifications.

You will also have to include the __google-service.json__ and __GoogleService-info.plist__ files you downloaded in the previous steps to your app to link it to google FCM services :

1. Copy the __google-service.json__ and __GoogleService-info.plist__ files in your projects __DisplayObjects/mobile/assets__ directory.
2. Edit the __DisplayObjects/platforms/Android/config.xml__ file and add the 

    	<resource-file src="assets/google-services.json" target="app/google-services.json" />

	in the ```<platform>``` section. 


4. Edit the __DisplayObjects/platforms/iOS/config.xml__ file and add the 

    	<resource-file src="assets/GoogleService-info.plist" target="app/GoogleService-info.plist" />

	in the ```<platform>``` section. 

The FCM Push notifications Cordova plugin requires Cordova cli-9.0.0 version so you must update this in your __config.xml__:


1.  Edit the __DisplayObjects/platforms/Android/config.xml__ file and modify the phonegap cli version to 9.0.0:

	    <preference name="phonegap-version" value="cli-9.0.0" />      <!-- all: current version of PhoneGap CLI -->

2.  Edit the __DisplayObjects/platforms/iOS/config.xml__ file and modify the phonegap cli version to 9.0.0:

	    <preference name="phonegap-version" value="cli-9.0.0" />      <!-- all: current version of PhoneGap CLI -->

The last step is to configure in your [MobileApplication](https://www.convertigo.com/documentation/latest/reference-manual/convertigo-objects/mobile-application/mobile-application/) the correct __Application ID__ for your app. This must be exactly the ID you configured in the google FCM Console at step #6  


## Client part usage

the __FCMPushNotifications__ shared action has a __Topics__ shared variable you have to set to register FCM notifications on a list of topics. By default, the topic list is empty but you have to configure an array of FCM topics you want to be notified on.

You can receive FCM events by using a [Subscribe Handler](https://www.convertigo.com/documentation/latest/reference-manual/convertigo-objects/mobile-application/components/control-components/subscribe-handler/) on the "FCM" event topic property. You will receive FCM events with this data structure :

		{
			type: "data" or "token" (If token the data field is the token),
			data: {
				wasTapped: false if received in background, true otherwise,
				property1: value1,
				property2: value2,
				...
				propertyN: valueN,
				body: "The push notification payload"
			}
		}					   
       
## Server part configuration

For the server part you will need to configure a Convertigo Global symbol for the FCM server key. Set the : 

		lib_PushManager_Ngx.serverKey

To the value of the key you copied in the section __Getting your server key__ 

## Server part usage

When you want to send a notification on a topic, just use the __SendNotifications__ sequence and use the following variables :

* topic   : The topic you want to notify on. All devices registered with this topic will receive the notification
* title   : The title of the notification. Will appear in the notification tray
* payload : The message body for notification
* sound	  : Sound to be played upon notification. Supports "default" or the filename of a sound resource bundled in the app. Sound files must reside in __/res/raw/__ (Android) or String specifying sound files in the main bundle of the client app or in the Library/Sounds folder of the app's data container. See the iOS Developer Library for more information (iOS). 
