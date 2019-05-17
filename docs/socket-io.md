---
id: socket-io
title: Socket.IO
sidebar_label: Real Time Engine
---

## Description
---
This websocket implementation used both **socket.io-client** and **ngrx/store**.

**socket.io** documentation: <https://socket.io/get-started/chat>

**ngrx/store** documentation: <https://ngrx.io/guide/store>

## Configuration
---
Our file structure:

**📁 src**

**I--📁 providers**

**I-----------I--📁 websocket**

**I----------------------I--📄 events.ts**

**I----------------------I--📄 index.ts**

**I--📁 store**

**I--------I--📁websocket**

**I-------------------I--📁reducers**

**I----------------------------I--📄resetStates.ts**

**I-------------------I--📄index.ts**


Before adding a socket connection, we should first add the following:

1. **Websocket**
- Events
2. **ngrx/store**
- Reducers
- Actions
First, in the `📄events.ts`, our code will be:
```js
export default function() {
  return [
    // add room name here
  ]
}
```
We will export all the event/room names needed to call the data from the websocket
Note: Inside the array, only include string names

Example:
```js
export default function() {
  return [
    'newMessage'
  ]
}

```
Next will be our reducers and actions inside the `📁reducers` folder
Example:
1. Create a file inside the `📁reducers` folder named `📄newMessage.ts`
Note: Use the event name for the file name to easily recognize the reducer
2. You can now add the code for reducer:
```js
import { Action } from '@ngrx/store';
const NEW_MESSAGE = '[Websocket] New Message';
type Type = NewMessage

export class NewMessage implements Action{
  readonly type = NewMessage
  constructor(public payload: any){}
}

export function newMessageReducer(state: any, action: Type){
  switch (action.type) {
      case NEW_MESSAGE:
      return action.payload;
  }
}
```

3. Keep in mind that every socket room needs a reducer file.

Our new file structure will be like this:

**📁 src**

**I--📁 providers**

**I-----------I--📁 websocket**

**I----------------------I--📄 events.ts**

**I----------------------I--📄 index.ts**

**I--📁 store**

**I--------I--📁websocket**

**I-------------------I--📁reducers**

**I----------------------------I--📄resetStates.ts**

**I----------------------------I--📄newMessage.ts** `<= newly added reducer`

**I----------------------------I--📄index.ts**


We will register our reducers in `📄index.ts` file inside `📁reducers` folder:

**I--📁 store**

**I--------I--📁websocket**

**I-------------------I--📁reducers**

**I----------------------------I--📄resetStates.ts**

**I----------------------------I--📄index.ts** `<= we will register it here`

and our code will be:
```js
// class and reducer imports here~
import { newMessage, newMesssageReducer } from './reducers/currentOrderLocation';


export const reducers = {
  // add reducers here~
  newMessage: newMessageReducer,
}

export const actions = {
  // add action class here~
  newMessage: newMessage,
}

// For reseting all states
import { ResetStates, ResetStatesReducer } from './reducers/resetStates'
export const resetStatesReducer = [ResetStatesReducer]
export const resetStates = ResetStates
```

**Note:** The reset states, will reset all data from the store to `undefined`

We can achieve this by simply calling `this.store.dispatch(new resetStates())`

Finally, we can now make our socket connection in-

**I--📁 providers**

**I--------I--📁 websocket**

**I--------------------I--📄 index.ts** `<= We will add the socket connection in this file`

our connection code will look like this:
```js
connect(token, callback) {
    const delay = this.close() ? 1 : 1000
    setTimeout(() => {
      this.socket = io(GlobalsProvider.websocketUrl, {
        reconnect: true,
        transports: ['websocket'],
        query: {
          token: token
        }
      })

      const actionList = actions
      for (let event of Events()) {
        this.socket.on(event, data => {
          this.store.dispatch(new actionList[event](data));
        })
      }
    }, delay)
}
```

We will also add three(3) functions needed for socket communication:
```js
  on(name) {
    return this.store.select(name)
  }

  emit(event, payload) {
    try { this.socket.emit(event, payload) }
    catch (error) { }
  }

  close() {
    try { this.socket.close() }
    catch (error) { return true }
  }
```

The `on()` function will be our substitute for the `io.on()` function in the <https://socket.io/>
documentation. In this process we will get the data from the ngrx/store using `this.store.select(name)`.
It accepts the event name and will return an observable that will also updated everytime the socket received the data.

It will be updated like the traditional <https://socket.io/> subscription because of this code:
```js
this.socket.on(event, data => {
         this.store.dispatch(new actionList[event](data));
  })

```

We already subscribed ALL the socket connections that we have and already available since we run the app.

The `emit()` function and `io.emit()` is the same, the only difference is the error handling in the defined function

The `close()` function is used after the logout or any event that we dont need the socket anymore
Note: Calling this function will close the current socket connection and will never reconnected again unless
the `connect()` function is called again.


## Usage
---
We will first add the WebsocketProvider in the `📄app.module.ts` file

Then, will call the `connect()` function in two conditions:

1. The app should have the token in the storage and

2. We need to call it from the start when the app is already loaded

Example:
```js
async socketConnect() {
  await this.storage.get('token')
  this.socket.connect(token, () => {
    console.log('socket connected!');
  })
}
```

The `connect()` function will establish connection between the server and the client.

It will also create a storage for every socket event/room that will process here:

```js
 for (let event of Events()) {
        this.socket.on(event, data => {
          this.store.dispatch(new actionList[event](data));
        })
  }
```
and after we dispatch, we can now subscribe to our socket connections in any page.

We need to add the provider (WebsocketProvider) in our pages so that we can achieve something like this:

```js
this.ws.on('newMessage').subscribe(newMessage => {
    // do something with newMessage data
})
```

**Note:** If you subscribe from a pushed page, please wrapped it in subscription to unsubscribe on page pop/dismiss

You can check the subscription documentation here: <http://reactivex.io/rxjs/class/es6/Subscription.js~Subscription.html>

Example:
```js
import { ISubscription } from "rxjs/Subscription";

export class`````` implements OnDestroy {

private subscription = []

  constructor(private subscription: ISubscription) {

    this.subscription.push(this.ws.on('newMessage').subscribe(newMessage => {
      // do something with newMessage data
    }))
  
  }
  
  ngOnDestroy() {
    for(let s of subscription) {
      s.unsubscribe()
    }
  }

}
```
Unable to unsubscribe creates multiple subscription and will run the code inside the subscribe block
multiple times.

If you want to know more about ngrx/store (reducers, actions, process, etc) you can learn here:
<https://medium.com/frontend-fun/angular-ngrx-a-clean-and-clear-introduction-4ed61c89c1fc>



