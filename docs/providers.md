---
id: providers
title: Ionic Providers
sidebar_label: Ionic Providers
---

## Alert
---

### Functions

#### open
```js
open(title: string, message: string, buttonsArray: Array<String> = ['OK'], addParams: AlertOptions = {})
```
- **title** : string - title of the alert

- **message** : string - message of the alert

- **buttonsArray** : Array = ['OK'] - buttons of the alert. Default value is ['Ok']

- **addParams** : AlertOptions = {} - additional params for specials cases like enableBackdropDismiss. Default value is {}

All ionic alert options can be added on this params. 

See <https://ionicframework.com/docs/api/components/alert/AlertController/> for more info.


### Basic Usage

### Declaration
```js
import { AlertMessageProvider } from '../../exports/provider.exporter';

constructor(private alertMessage: AlertMessageProvider) {

}
```

### Types of Usage
- Default Alert
```js
this.alertMessage.open('Title', 'Message');
```

![Image of Default Alert](https://kmramirez3.github.io/ionic-documentation/img/alert-basic.png)

- With Cancel Button
```js
this.alertMessage.open('Title', 'Message', ['Cancel', 'Ok']).then(btn => {

        if (btn === 'Ok') {
                console.log('Ok button clicked')
                //Additional logic can be added here
        }

        if (btn === 'Cancel') {
                console.log('Cancel button clicked')
                //Additional logic can be added here
        }

})
```
![Image of Default Alert](https://kmramirez3.github.io/ionic-documentation/img/alert-with-cancel.png)

- With Additional Parameters
```js
//the user cannot dismiss the alert by clicking the backdrop
this.alertMessage.open('Title', 'Message', ['Cancel', 'Ok'], { enableBackdropDismiss: false }).then(btn => {

        if (btn === 'Ok') {
                console.log('Ok button clicked')
                //Additional logic can be added here
        }

        if (btn === 'Cancel') {
                console.log('Cancel button clicked')
                //Additional logic can be added here
        }

})
```

## FormData
---

### Documentation 

The FormData provides a way to easily construct a set of key/value pairs representing form fields and their values, which can then be easily sent using the `XMLHttpRequest.send()` method.
It uses the same format a form would use if the encoding type were set to *multipart/form-data*.

Formdata provider is in 📁providers > 📄formdata.ts

In that folder, you have two functions namely:

1. `generateBlob()`

- This function lets you convert your `base64` image/video format to `blob` object. A Blob object represents a file-like object of immutable, raw data. Blob objects are used when passing image data into the API.
2. `generate()`

- This function converts all your array of data to `formdata`. The backend usually requires data passed must be in formdata format.


## Global Declarations

### Header

Declaring global variables is good to use for constants to keep consistency. You can only declare it once and can be accessible to all modules. It keeps maintenance easier and the program easier to read.

### Basic Usage

Generate a provider

`ionic g provider globals`

declare variables you will be using a lot to different modules
```js
@Injectable()
export class GlobalsProvider {

  baseUrl: string = 'http://yourBaseUrl/';
  globalVariable: string = 'This is a global message'
  imagePathUrl: string = 'https://placeimg.com/640/480/any'

}
```
import the provider to access your globally declared variables
```js
import { GlobalsProvider } from '../../../exports/provider.exporter';

@IonicPage()
@Component({
  selector: 'page-globals-provider-test',
  templateUrl: 'globals-provider-test.html',
  providers: [
    GlobalsProvider
  ]
})
export class GlobalsProviderTestPage {

  global: string = this.globalsProvider.globalVariable
  globaImgUrl: string = this.globalsProvider.imagePathUrl

}
```
HTML Template
```html
<ion-header>

  <ion-navbar>
    <ion-title>Globals</ion-title>
  </ion-navbar>

</ion-header>

<ion-content padding>

  <p>Gloabl Variable: <span style="font-style: italic;">"{{global}}"</span></p>
  <br>
  <img [src]="globaImgUrl">
  <p text-center>This is global image url.</p>

</ion-content>
```

![Image of Global Declaration](https://kmramirez3.github.io/ionic-documentation/img/global.png)

## Pluralizer

This provider is use for the angular I18nPluralPipe (reference site: <https://angular.io/api/common/I18nPluralPipe>) that returns an object with a set of conditions base on the length of an array or number.

### Basic Usage

generate provider `ionic g provider pluralizer`

###### pluralizer.ts
```js
import { Injectable } from '../exports/angular.exporter';

@Injectable()
export class PluralizerProvider {

  setPluralForm(ifZero, ifOne, ifTwoMore) {
    return { '=0': ifZero, '=1': ifOne, 'other': ifTwoMore }
  }

}
```

#### Provider Implementation

###### pluralizer-test.ts
```js
import { PluralizerProvider } from '../../../exports/provider.exporter';

@IonicPage()
@Component({
  selector: 'page-pluralize-provider-test',
  templateUrl: 'pluralize-provider-test.html',
  providers: [ PluralizerProvider]
})
export class PluralizeProviderTestPage {

  messages: any = ['Message 1', 'Message 2', 'Message 3', 'Message 4'];

  constructor(private pluralizerProvider: PluralizerProvider) { }

  pluralizerForMessages() { // set conditional messages based on array length/numbers
    return this.pluralizerProvider.setPluralForm('# Messages', '# Message', '# Messages')
  }
```
###### pluralizer-test.html
```html
`<p text-center>{{ messages.length | i18nPlural: pluralizerForMessages() }}</p>`
```

## Toast
---

```js
presentToast(payload: ToastOptions, callback: VoidFunction = this.defaultCallback) {  }
```

| Property 	| Type                   	| Description                                                                                                                     	|
|----------	|------------------------	|---------------------------------------------------------------------------------------------------------------------------------	|
| payload  	| Object  `ToastOptions` 	| Options of the toast, available list are here  <https://ionicframework.com/docs/api/components/toast/ToastController/#advanced> 	|
| callback 	| function               	| Triggers on toast dismissal                                                                                                     	|


### Basic Usage

#### Declaration
```js
import { ToastMessageProvider } from '../../exports/provider.exporter';

constructor(private toastMessage: ToastMessageProvider) {

}
```
#### Types of Usage
- Default Toast
The expected required parameters in the first is always an object. It has an intellisense for the required properties in the object base from `ToastOptions`.
```js
this.toastMessage.presentToast({ message: 'TADAHHH!!!', duration: 3000 });
```

- Toast with Callback
```js
showToast() { 
    this.toastMsg.presentToast({ message: 'TADAHHH!!!' }, () => {
     console.log('Toast has been dismissed')
    })
  }
```