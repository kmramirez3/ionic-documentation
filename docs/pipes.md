---
id: pipes
title: Ionic Pipes
sidebar_label: Ionic Pipes
---

## Emoji
---

### Basic Usage

EmojiOne Reference site : <https://github.com/emojione/emojione>

#### Instructions

generate emoji pipe `ionic g pipe emoji`

declare EmojiOne plugin `import * as emoji from 'emojione';`

Choose among the 3 types of conversion

###### emoji.ts
```
import { Pipe, PipeTransform } from '@angular/core';
import * as emoji from 'emojione';

@Pipe({
  name: 'emoji',
})
export class EmojiPipe implements PipeTransform {
  /**
   * Takes a value and convert it to emoji.
   */
  transform(value: string) {
    return emoji.toImage(value) //converts input containing both native unicode emoji and shortnames into EmojiOne images.
 // return emoji.toShort(value) //converts native unicode emoji input(from mobile device) to their corresponding shortnames.
 // return emoji.shortnameToImage(value) //converts shortnames directly to EmojiOne images.
  }
}
```
###### emoji-test.html
```
<ion-content padding>

  <table>
    <tr>
      <th>Emoji</th>
      <th>Short Code</th>
    </tr>
    <tr *ngFor="let emoji of stringEmojis">
      <td [innerHTML]="emoji.name | emoji"></td>
      <td>{{emoji.shortCode}}</td>
    </tr>
  </table>

  <p [innerHTML]="inputtedValue | emoji" text-center></p>

  <ion-item>
    <ion-label color="primary" floating>Input</ion-label>
    <ion-input [(ngModel)]="inputtedValue"></ion-input>
  </ion-item>

</ion-content>
```
**Note:** `[innerHTML]` was used because pipe transforms shortcode to img tag

#### Demo

![GIF of Demo Emoji](https://kmramirez3.github.io/ionic-documentation/img/mygif.gif)

## Order By
---

### Description

This pipe is based on this repo: <https://github.com/FuelInteractive/fuel-ui/tree/master/src/pipes/OrderBy>

### Usage

- `orderBy` => for single type array (String or Integer)

Example:
```
<span *ngFor="let n of numberArray | orderBy">{{n}}<span>
```

- `orderBy`: - => for single type array in descending order

positive(+) sign == ascending(default)

negative(-) sign == descending

Example:
```
<span *ngFor="let fruit of fruitArray | orderBy : '-'">{{fruit}}<span>
```

- `orderBy`: key => for a multidimentional array (array of objects) on single column

Example:
```
<span *ngFor="let todo of todos | orderBy : 'status'">{{todo.name}} - {{todo.status}}<span>
<span *ngFor="let todo of todos | orderBy : '-status'">{{todo.name}} - {{todo.status}}<span>
```

- `orderBy`: ['key1', 'key2'] => for Multidimensional Array Sort on multiple columns

Example:
```
<span *ngFor="let todo of todos | orderBy : ['status', '-title']">{{todo.name}} - {{todo.status}}<span>
```


