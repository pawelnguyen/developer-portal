---
title: Web Bluetooth (draft)
subtitle:
tags: []
category:
author:
toc: true
layout: doc_tr
---



## Install the package

`yarn add @ledgerhq/hw-transport-web-ble`

## Implementation

### Parameters

-   `device` **Device**
-   `writeCharacteristic` **Characteristic**
-   `notifyObservable` **Observable&lt;any>**
-   `deviceModel` **DeviceModel**

### Examples

```js
import BluetoothTransport from "@ledgerhq/hw-transport-web-ble";
```

### exchange

Exchange with the device using APDU protocol.

**Parameters**

-   `apdu` **[Buffer](https://nodejs.org/api/buffer.html)**

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;[Buffer](https://nodejs.org/api/buffer.html)>** a promise of apdu response

### observeAvailability

observe event with { available: bool, type: string }
(available is generic, type is specific)
an event is emit once and then each time it changes

**Parameters**

-   `observer` **any**

### listen

Scan for Ledger Bluetooth devices.
On this web implementation, it only emits ONE device, the one that was selected in the UI (if any).

**Parameters**

-   `observer` **any**

### open

open a bluetooth device.

**Parameters**

-   `deviceOrId` **(Device | [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))**

### disconnect

globally disconnect a bluetooth device by its id.

**Parameters**

-   `id` **any**
