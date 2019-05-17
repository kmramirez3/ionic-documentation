---
id: plugins
title: Ionic Plugins
sidebar_label: Ionic Plugins
---

## Emoji
---
EmojiOne plugin gives users a set of libraries to convert native emojis from devices to EmojiOne emoji.

### Installation

`$npm install emojione`

### Basic Usage

Reference Site: <https://github.com/emojione/emojione>

#### Declaration

`import * as emoji from 'emojione';`

#### Types of Usage

- `.toShort(str)` - converts native unicode emoji input(from mobile device) to their corresponding shortnames.

- `.shortnameToImage(str)` - converts shortnames directly to EmojiOne images.

- `.toImage(str)` - converts input containing both native unicode emoji and shortnames into EmojiOne images.

#### Demo

![GIF of Demo Emoji](https://kmramirez3.github.io/ionic-documentation/img/mygif.gif)


## Gallery Modal
---

### Installation
Type in your terminal inside your **Ionic Project**

`npm install ionic-gallery-modal --save`

and then in your **app/** folder directory create a file called **hammer.module.ts**

paste in this block of code below in that file

```js
import { HammerGestureConfig } from '@angular/platform-browser';
// import { HAMMER_GESTURE_CONFIG } from '@angular/platform-browser';

declare var Hammer: any;

export class HammerConfig extends HammerGestureConfig {
    overrides: {
        pan: {
            direction: number;
        };
        press: {
            time: number;
        };
    };

    buildHammer(element: HTMLElement) {
      let mc: any = new Hammer(element, {
        touchAction: 'pan-y'
      });
      return mc;
    }
}
```

after that import it from your **app.module** file
```js
import * as ionicGalleryModal from 'ionic-gallery-modal';
import { HammerConfig } from './hammer.module';
```

import the file that we created called **HammerConfig** and `import * as ionicGalleryModal` from the plugin that we installed

then in inside your **imports array** in your **app.module** insert the `ionicGalleryModal.GalleryModalModule`

```js
imports: [
...,
ionicGalleryModal.GalleryModalModule,
...
]
```

and in your **providers array** put the **HammerConfig** that we created
```js
providers: [
    { provide: HAMMER_GESTURE_CONFIG, useClass: HammerConfig },
    ...
  ]
```

### Basic Usage

#### Declaration
```js
import { GalleryModal } from 'ionic-gallery-modal';
```

#### Types of Usage
```js
viewGallery(img) { // View the image by tapping one index on an array using gallery modal
    const modal = this.modalCtrl.create(GalleryModal, {
      photos: this.arrayOfImages, // Should be an array of images
      initialSlide: img.index, // Index of image in the array you want to view
      closeIcon: 'arrow-back'
    }, {
        cssClass: 'no-index'
      })
    modal.present()
  }
```
It's just like creating a normal modal in Ionic but you passed in the **GalleryModal** in the **import declaration** instead of passing a *page name* in the first parameter and the **GalleryModal** will handle it for you with swipe functionality to view an array of photos like a gallery. The second parameter is the options. `photos` property expects an array of images with this format below:
```json
[
 {
   url: 'http://i.pravatar.cc/300'
  }
]
```
The `url` property inside the array of objects should be require in order to render the images passed in.

For more information visit: <https://www.npmjs.com/package/ionic-gallery-modal>


## Image Viewer
---

### Basic Usage

#### Declaration
```js
import { ImageViewerController } from 'ionic-img-viewer';

constructor(public imageViewerCtrl: ImageViewerController) {

}
```

#### Types of Usage

- **Image Viewer WITHOUT config options**

In html you need to declare a directive called `#myImage` or you can name it whatever you want. This directive will be passed in a function to present the image.
```html
<img src="http://i.pravatar.cc/300" #myImage (click)="presentImage(myImage)" />
```

In your **.ts file** create a variable and passed in the `imageViewerCtrl.create(Image)` base on your **declaration** to create the instance of the image viewer and call the `imageViewer.present()` in order to view the image.
```js
presentImage(myImage) { // View the image using image viewer
    const imageViewer = this.imageViewerCtrl.create(myImage);
    imageViewer.present();
  }
```

- **Image viewer WITH config options**

Using the same HTML, this time we can add a second parameter as a config options. In this example we added:

- `enableBackdropDismiss` - set it to **true** so whenever you click outside of the **image** it will close the viewer.
- `onCloseCallback` - expects a function as a callback whenever the image has been closed.

For more config options vist: <https://www.npmjs.com/package/ionic-img-viewer>
```js
presentImage(myImage) { // View the image using image viewer
    const config = {
      enableBackdropDismiss: true,
      onCloseCallback: this.someFunc()
    }
    const imageViewer = this.imageViewerCtrl.create(myImage, config);
    imageViewer.present();
  }
    
someFunc() {
    console.log('Callback happened')
  }
```