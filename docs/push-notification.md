---
id: push-notification
title: Push Notification - OneSignal
sidebar_label: Push Notification
---

This application uses **OneSignal** - a high volume and reliable push notification service for websites and mobile applications. It support all major native and mobile platforms by providing dedicated SDKs for each platform, a RESTful server API, and an online dashboard for marketers to design and send push notifications. <https://documentation.onesignal.com/docs>

## OneSignal Setup
---

Documentation available here: 
<https://docs.google.com/document/d/1IpwuFwIXSMX5BCT82n2P-WZB2L1J4UXeyLsokHE2TEY/edit>
<https://medium.com/appseed-io/how-to-integrate-onesignal-push-notifications-into-an-ionic-3-application-eb2fdc3e6176>


### Generate a Google Server API Key (SERVER KEY)
The application we are going to build, uses **Firebase** in order to generate a Google Server API Key. This key allows OneSignal to use Google’s web push services for your notifications. 

Visit <https://console.firebase.google.com/> and create a new project.

**Sample Credentials:** Project ID: onesignal-test-7763c (email: vrferrer.halcyondigital)

![Image of Adding Project](https://kmramirez3.github.io/ionic-documentation/img/1.png)

![Image of Adding Project](https://kmramirez3.github.io/ionic-documentation/img/2.png)

#### Get your Firebase Cloud Messaging token and Sender ID

- Click the **gear icon** at the top left of the page and select **Project Settings**

![Image of Project Setting](https://kmramirez3.github.io/ionic-documentation/img/3.png)

- Select the **Cloud Messaging** tab.
- Make a note of the two values listed:

You’ll need your **Servery Key**, also known as the Google Server API key.

You’ll need your **Sender ID**, also known as the Google Project Number, later as well.

![Image of Settings](https://kmramirez3.github.io/ionic-documentation/img/4.png)

In Sample Image below, the credentials are:

**SERVER KEY:** AAAAZrAi7r4:APA91bEjeLzjlqsDaym0Y_kVas-2WBlqrO0hfUQLajbg2xUF2SDBqFDBknE2aU_ZYruN8GYRur23cidmYkCDMUEiq9rSimRqHy9_UHPvY5uoBgSB7ZaojA16E6qOLxnyHrzc_Wg8rx (email: vrferrer.halcyondigital)

**SENDER ID:** 441041743550 (email: vrferrer.halcyondigital)

### Configure OneSignal Application

We will use OneSignal to send push notifications. Visit OneSignal and create an account if you haven’t already.

#### Configure OneSignal Android Platform Settings.

- After login to One Signal, click on Add new app.
- Type a name for your application in the appeared pop up. Here, we use “AppseedPushNotifications” as an App Name.

![Image of OneSignal New App](https://kmramirez3.github.io/ionic-documentation/img/5.png)

- Select the platform that you want to configure with OneSignal push notifications and click NEXT. For this tutorial, select Google Android (GCM).

![Image of OneSignal New App](https://kmramirez3.github.io/ionic-documentation/img/6.png)

- Next, enter your Google Server API Key and Google Project Number and click SAVE. These are the details you got from the Firebase console in the previous step.

![Image of OneSignal New App](https://kmramirez3.github.io/ionic-documentation/img/7.png)

- Then, select the target SDK for your app. In our case, we are making an Ionic app so select, PhoneGap, Cordova, Ionic, Intel XDK and click NEXT.

![Image of OneSignal New App](https://kmramirez3.github.io/ionic-documentation/img/8.png)

- As a final step, take a note of the OneSignal App ID, as we will use it in our application later.

![Image of OneSignal New App](https://kmramirez3.github.io/ionic-documentation/img/9.png)
*(You can’t click done unless there is one subscriber user)*

## OneSignal Implementation
---

After setup, you need to update keys and ids located on **providers/globals.ts**
```js
restApiKey= 'OTAyNDg0NWMtODEwYy00YmFkLWFkNjMtY2I0OGVlYzczZDU3'
oneSignalAppId = '2b9a43f4-8003-4ff4-802b-1ca4b3deef50'
senderId = '441041743550'
```

## Send Push Notification
---

There are two ways to send push notification on OneSignal:

### POST Request
```js
import { GlobalsProvider } from '../../../providers/globals';
constructor( public globals: GlobalsProvider ) { }

sendPushNotif() {
  this.http.post('https://onesignal.com/api/v1/notifications',
  {
    app_id: this.globals.oneSignalAppId,
    contents: { en: 'Test Push Notif' },
    included_segments: ['All'] //this notif will be sent to all the subscribed users
  },
  {
    headers: new HttpHeaders({
      'Authorization': `Basic ${this.globals.restApiKey}`,
      'Content-Type': 'application/json'
    })
  }).toPromise().then(data => {
    console.log(data)
  }).catch(err => {
    console.log(err)
  })
}
```

### OneSignal Dashboard

After running the application visit OneSignal <https://onesignal.com/> . Select your application. Here you can see the number of users who have been registered for receiving push notifications.

Go ahead and send a message. To do this, navigate to OneSignal dashboard and click **MESSAGES**.

Click **NEW PUSH**.

Type the preferred title and message and click **CONFIRM** in the bottom of the screen.

Finally, from the appeared pop up click **SEND MESSAGE**.

Moments later, a push notification will be received in your device.


## Back-end Integration
---

Both back end and front end can send push notification by accessing the OneSignal API <https://documentation.onesignal.com/reference#create-notification>

Give the OneSignal App ID and Rest API Key to your backend developer because it is needed on the body and headers of the POST request





