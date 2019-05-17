---
id: global-import-export
title: Global Import And Exports Of Modules
sidebar_label: Global Import And Exports Of Modules
---

## Introduction
---
- We are doing this practice to prevent lag, delays, cleaner code and better user experience on the application.
- To prevent declaring same import packages.

**OBJECTIVE:** 

- To prevent adding multiple decalaration on app.module.ts and per ts page declaration. 
- To prevent too much content of *app.module.ts* which results of laggy on first load of the application. Only add it if is really necessary needed

## Files
---
In the root of the project repository, you will find **`📁 exports`** folder. In there there are 4 files namely :

`📄 angular.exporter.ts`

`📄 component.exporter.ts`

`📄 native.exporter.ts`

`📄 provider.exporter.ts`

`📄 directive.exporter.ts`

## List of Exported Modules Per File
---
1. **Angular** `📄 angular.exporter.ts`

Note: If you want to add another class module from node_modules, just simply add it. Only @angular/ modules can be added here.

2. **Components** `📄 components.exporter.ts`

Note: If you want to add another class module from node_modules, just simply add it.

3. **Directive** `📄 directive.exporter.ts`

Note: If you want to add another class module from node_modules, just simply add it. Some of these plugins are third-party plugins.

4. **Native** `📄 native.exporter.tsnative.exporter.ts`

Note: Only @ionic-native modules can be exported in here.

5. **Provider** `📄 provider.exporter.ts`

Note: If you want to add another class provider from providers folder, just simply add it.

## Basic Example Usage (📄 native.exporter.ts)
---
We will be using the *camera* plugin as an example.

1. Install your native plugin using *npm install*
2. In your `📄 native.exporter.ts` add your native plugin

```js
export { Camera } from '@ionic-native/camera';
```

3. In your page, import your native plugin in your *exports/native.exporter.ts* path
```js
import { Camera } from '../../exports/native.exporter';
```

4. Lastly, add it on your constructor
```js
constructor( public camera: Camera ) 
```