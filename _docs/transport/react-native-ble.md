---
title: React native Bluetooth (draft)
subtitle:
tags: []
category:
author:
toc: true
layout: doc_tr
---



## Install the package

`yarn add @ledgerhq/react-native-hw-transport-ble`

## Pre-requisite

-   [**Install and link library `react-native-ble-plx` + configure your app for Bluetooth permissions**](https://github.com/Polidea/react-native-ble-plx) (Open the link for documentation)
-   [for Android] **a recent version of JavaScriptCore** instead of React Native's default one. The simplest way is to set up [jsc-android](https://www.npmjs.com/package/jsc-android). In future, [we expect](https://github.com/facebook/react-native/issues/19737) React Native to upgrade JSC for Android.
-   **global.Buffer** available. Typically `global.Buffer = require("buffer").Buffer;` that can be placed in a `polyfill.js` and imported with `import "./polyfill";` at first line of the main JavaScript entry file.

## Troubleshooting

### Bad BLE link between the Phone and the device

It can sometimes happen that the phone have a broken link, it is in this case recommended to users to go to their phone settings and manually Forget the Nano X. There is at the moment no other ways: there is no programmatic way to unpair a bonded device but you can however have a button that opens that phone settings Bluetooth section.

## Minimal getting started

`@ledgerhq/react-native-hw-transport-ble` works like any of our `@ledgerhq/hw-transport` libraries.

The difference here is that the listen() is costy and you likely don't want to always scan for devices, you would better save a selected device as "known" to suggest it to user later.

> Important: you will also have to deal with specifics of Bluetooth BLE, for instance, you need to request the LOCATION permission on Android!

Here is a gist of the most important parts required.

### Check for Bluetooth state

```js
import TransportBLE from "@ledgerhq/react-native-hw-transport-ble";

const subscription = TransportBLE.observeState({
  next: e => syncWithUI(e.available),
  complete: () => {},
  error: () => {}
});
```

**when Bluetooth is available we can continue to...**

### Scan for devices

```js
const subscription = TransportBLE.listen({
  complete: () => {
    this.setState({ refreshing: false });
  },
  next: e => {
    if (e.type === "add") {
      const device = e.descriptor;
      addDeviceToTheUI(device);
    }
  },
  error: error => {
    this.setState({ error, refreshing: false });
  }
});

// NB we also recommend the use of RxJS, you can just wrap it like so:
// Observable.create(TransportBLE.listen).subscribe({ ... })
// In the future of ledgerjs, we might directly returns Observable.
```

This is the basic idea, obviously you need to call this code in your logic and implement `addDeviceToTheUI(device)`.

In that `device` object, you can typically use two fields:

-   `device.id`: a unique identifier of the device that can later be given to `TransportBLE.open(id)`
-   `device.name`: the name of the device (e.g. `NanoX A4F5`)

**With this device we can now...**

### Connect to known device

```js
const transport = await TransportBLE.open(device);
// OR
const transport = await TransportBLE.open(deviceId); // deviceId can come from persisted state. It's unique per device
```

**and now we can just use the transport like any other Ledger transport!**

## Implementation

### Parameters

-   `device` **Device**
-   `writeCharacteristic` **Characteristic**
-   `notifyObservable` **Observable&lt;[Buffer](https://nodejs.org/api/buffer.html)>**
-   `deviceModel` **DeviceModel**

### Examples

```javascript
import BluetoothTransport from "@ledgerhq/react-native-hw-transport-ble";
```

**exchange**

communicate with a BLE transport

**Parameters**

-   `apdu` **[Buffer](https://nodejs.org/api/buffer.html)**

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;[Buffer](https://nodejs.org/api/buffer.html)>**

### isSupported

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)>**

### setLogLevel

**Parameters**

-   `level` **any**

### observeState

TODO could add this concept in all transports
observe event with { available: bool, string } // available is generic, type is specific
an event is emit once and then listened

**Parameters**

-   `observer` **any**

### listen

Scan for bluetooth Ledger devices

**Parameters**

-   `observer` **any**

### open

Open a BLE transport

**Parameters**

-   `deviceOrId` **any**

### disconnect

Globally disconnect a BLE device by its ID

**Parameters**

-   `id` **any**
