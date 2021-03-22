# lib_PushManager_ui_ngx
Uses the Firebase Cloud Messaging (FCM) Push notifications library for the Convertigo Low Code Platform. This library brings the client part for Firebase Push Notifications support for Convertigo mobile builder apps. 

## Client Part configuration

The library provides a __FCMPushNotifications__ Convertigo Shared action you must add in your client application. To do so :

1. In your Mobile Builder application add a [Project Reference](https://www.convertigo.com/documentation/latest/reference-manual/convertigo-objects/common/references/schema-references/project-reference/) (Right click on the Convertigo project root and follow New->Reference->Project reference). Choose any template as it will be edited in the next step.
2. On the project reference property click on the __"Project name and remote URL"__ property '...' to edit the property
3. Paste in the "Project Remote URL" field this reference :

		lib_PushManager_ui_ngx=https://github.com/convertigo/c8oprj-lib-push-manager-ui-ngx.git:branch=8.0.0
 
	  
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
       