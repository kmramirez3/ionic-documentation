---
id: global-bad-coding
title: Good and Bad Coding
sidebar_label: Good and Bad Coding
---

## Attribute / Property Proper Binding
---
```html
<!-- Bad -->
<img src=”{{imagePath}}”>


<!-- Bad -->
<img [src]=imagePath>


<!-- Good -->
<img [src]=”imagePath”  [disabled]=””>
```

## Line Optimization
---
```js
// Bad 
    if (this.value == 1) {  
       this.dosomething()
  } else {
          	 this.doDiffThing()
       }
       
    // Good
    if (this.value == 1) this.dosomething()
    else this.doDiffThing()
    
    
    
    // Bad 
    .then((response) => {
        console.log(response)
 })

    // Good
    .then((response) => console.log(response))
```

## Conditions
---
```html
<!--  Bad  -->
    <div *ngIF=”task.done ==  false” >

 <!-- Good  -->
    <div *ngIF=”!task.done” >


 <!-- Bad  -->
    <div *ngIF=”task.done ==  true” >

 <!-- Good  -->
    <div *ngIF=”task.done” >
```