PMS
===

Post Message Service which helps to deal with native js [window.postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window.postMessage).

---

Methods
=======

**Constructor**

`sets` (Object) - settings object  

Creates new Post Message Service instance.

    var pms = new PMS(sets);

Constructor gets settings that should be like this:

    {
        targetWindow: window.parent // target window object (e.g. iframe.contentWindow, window.parent)
        connectionID: 'myService1' // optional string unique ID of the current connection to filter messages from target window
        targetOrigin: '*' // optional string representing the origin of the targetWindow where event must be dispatched
    }

---

**.set(sets)**

`sets` (Object) - settings object same as in constructor  

This method resets the service, but *without* removing binded callbacks.

    pms.set(sets);

---

**.send(type[, data])**

`type` (string) - event name to be dispatched in the target window  
`data` (Object) - data to be sent to the target window  

Sends an event to the target window with provided data. Data must be an object.

    pms.send('hello', { to: 'John' });

---

**.sendRAW(message)**

`message` (*) - message of any type which can be JSON.stringified  

Sends a native (RAW) postMessage to the target window. You should listen to a 'message' event to receive RAW messages

    pms.sendRAW({ eventName: 'hello', eventData: 'John' });
    pms.sendRAW('Hi Jack');

---

**.on(type, callback[, context])**

`type` (string) - event name to listen to  
`callback` (Function) - callback to be called  
`context` (Object) - context for calling the callback  

Binds callback to the event. You can provide several `type`s divided by spaces or commas.

    pms.on('hello', function(e){
        console.log('Hello ', e.data.to);
    });

    pms.on('hello, hi, goodMorning', this._sayHello, this);

---

**.once(type, callback[, context])**

`type` (string) - event name to listen to  
`callback` (Function) - callback to be called  
`context` (Object) - context for calling the callback  

Binds callback to the event only *once*. You can provide several `type`s divided by spaces or commas.

    pms.once('hello', function(e){
        console.log('Hello ', e.data.to);
    });

    pms.once('hello, hi, goodMorning', this._sayHello, this);

---

**.trigger(type[, data[, rawEvent]])**

`type` (string) - event name to be dispatched  
`data` (Object) - event data  
`rawEvent` (*) - RAW event  

Triggers an event in *this* window. You can provide several `type`s divided by spaces or commas.

    pms.trigger('saidHello', { to: 'John' });
    pms.trigger('hello, hi, goodMorning', { to: 'John' });

---

**.off([type[, callback]])**

`type` (string) - event name to stop listen to
`callback` (Function) - callback to be removed from listening stack

 Removes event listeners. You can provide several types divided by spaces or commas. If `type` isn't provided *all* callbacks will be removed.

    pms.off('hello');
    pms.off('hello, hi', this._sayHello);
    pms.off();

---
