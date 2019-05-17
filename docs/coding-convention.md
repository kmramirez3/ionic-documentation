---
id: coding-convention
title: Coding Convention
sidebar_label: Coding Convention
---

## Import / Export Constructor Proper Declaration
---

### Constructor

Always remove the auto generated **console.log** of ionic

#### For one line
```js
constructor( public navCtrl: NavController ) {}
```

#### For two or more lines
```js
  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public imageViewerCtrl: ImageViewerController,
    public modalCtrl: ModalController
  ) { }
```

### Import / Export

#### For one line
```js
 import { IonicPage } from 'ionic-angular';
```

#### For two or more lines
```js
constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public imageViewerCtrl: ImageViewerController,
    public modalCtrl: ModalController
  ) { }
```

## Line Spacing
---

#### 2 lines away for every function
```js
    viewGallery(img) {
        const modal = this.modalCtrl.create(GalleryModal, {
            photos: this.arrayOfImages,
            initialSlide: img.index,
            closeIcon: 'arrow-back'
        }, {
            cssClass: 'no-index'
            })
        modal.present()
    }
//         Two lines away 
//      between two functions  
    presentImage(myImage) {
        const imageViewer = this.imageViewerCtrl.create(myImage);
        imageViewer.present();
    }

```

## Classes
---

Do use **upper camel case** when naming classes.

- Why? Follows conventional thinking for class names.
- Why? Classes can be instantiated and construct an instance. By convention, upper camel case indicates a constructable asset.

```js
/* bad */

export class exceptionService {
  constructor() { }
}

/* good */

export class ExceptionService {
  constructor() { }
}
```

## Constants
---

Do declare variables with const if their values should not change during the application lifetime.

- Why? Conveys to readers that the value is invariant.

```js
export const nationalHero = 'Jose Rizal';
export const heroesUrl = 'api/heroes';   
```


