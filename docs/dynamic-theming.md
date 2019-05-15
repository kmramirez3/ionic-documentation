---
id: dynamic-theming
title: Dynamic Theming
sidebar_label: Dynamic Theming
---

## Create Themes Files
---

*Under src/themes folder*
```
// Filename: theme.#{color}.scss or theme.dark.scss

.#{color}-theme { //theme.dark.scss, theme.red.scss, ....
    
    //set variables. these variables will serve as value of every property
    $primary: #000; //set the primary color;
    $font-color: #fff; //set base color
    
    //set styles, note: make css value dynamic, for able to adapt to another theme. Add variables/ styles if needed.
    ion-content {
    background-color: $primary;
    color: $font-color;
  }
 
  .toolbar-title {
    color: $font-color;
  }
 
  .header .toolbar-background {
    background-color: $primary;
  }
}  
```
*Import scss file on variables.scss*
```
@import "theme.light";
@import "theme.dark";
```

## Create Settings Provider
---

*open your src/providers/settings/settings.ts and insert*
```
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs/Rx';
 
@Injectable()
export class SettingsProvider {
 
    private theme: BehaviorSubject<String>;
 
    constructor() {
        this.theme = new BehaviorSubject('light-theme');
    }
 
    setActiveTheme(val) {
        this.theme.next(val);
    }
 
    getActiveTheme() {
        return this.theme.asObservable();
    }
}
```

## Page That Triggers the Theme
---
*page.ts*
```
//import settings
import { SettingsProvider } from './../../providers/settings';

//declare a variable
selectedTheme: String;

//on the constructor
constructor(public navCtrl: NavController, private settings: SettingsProvider) {
    this.settings.getActiveTheme().subscribe(val => this.selectedTheme = val);
}

//create function toggleAppTheme()
toggleAppTheme(theme) {
    this.settings.setActiveTheme(`${theme}-theme`);
}

//whole code
import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';
import { SettingsProvider } from './../../providers/settings';
/**
 * Generated class for the ThemingPage page.
 *
 * See https://ionicframework.com/docs/components/#navigation for more info on
 * Ionic pages and navigation.
 */

@IonicPage()
@Component({
  selector: 'page-theming',
  templateUrl: 'theming.html',
})
export class ThemingPage {
  selectedTheme: String;

  constructor(public navCtrl: NavController, public navParams: NavParams, private settings: SettingsProvider) {
    this.settings.getActiveTheme().subscribe(val => this.selectedTheme = val);
  }

  ionViewDidLoad() {
    console.log('ionViewDidLoad ThemingPage');
  }

  toggleAppTheme(theme) {
    // alert(`${theme}-theme`);  
    this.settings.setActiveTheme(`${theme}-theme`);
  }

}

  
<button ion-button color="dark" (click)="toggleAppTheme('dark')">Dark</button>
<button ion-button color="light" (click)="toggleAppTheme('light')">Light</button>
<button ion-button color="blue" (click)="toggleAppTheme('blue')">Blue</button>
<button ion-button color="red" (click)="toggleAppTheme('red')">Red</button>
<button ion-button color="green" (click)="toggleAppTheme('green')">Green</button>
<button ion-button color="pink" (click)="toggleAppTheme('pink')">Pink</button>
<button ion-button color="violet" (click)="toggleAppTheme('violet')">Violet</button>
```

## Last Step
---

```
import { SettingsProvider } from './../providers/settings/settings';
import { Component } from '@angular/core';
import { Platform } from 'ionic-angular';
import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';
 
import { HomePage } from '../pages/home/home';
@Component({
  templateUrl: 'app.html'
})
export class MyApp {
  rootPage:any = HomePage;
  selectedTheme: String;
  
  constructor(platform: Platform, statusBar: StatusBar, splashScreen: SplashScreen, private settings: SettingsProvider) {
    this.settings.getActiveTheme().subscribe(val => this.selectedTheme = val);
 
    platform.ready().then(() => {
      statusBar.styleDefault();
      splashScreen.hide();
    });
  }
}
<ion-nav [root]="rootPage" [class]="selectedTheme"></ion-nav>
```

## Reference
---
<https://devdactic.com/dynamic-theming-ionic/>




