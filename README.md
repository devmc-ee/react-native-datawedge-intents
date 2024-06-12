This is a cleaned and fixed version of [https://github.com/darryncampbell/react-native-datawedge-intents](https://github.com/darryncampbell/react-native-datawedge-intents)

# React-Native-DataWedge-Intents
React Native Android module to interface with Zebra's DataWedge Intent API

This module is useful when developing React Native applications for Zebra mobile computers, making use of the Barcode Scanner

### Installation

```bash
yarn add https://github.com/DevMcEE/react-native-datawedge-intents.git#master
```

## Example usage

There are two samples available for this module:

**Please see [DataWedgeReactNative](https://github.com/darryncampbell/DataWedgeReactNative) for a more fully featured and up to date application that makes use of this module**, file [App.js](https://github.com/darryncampbell/DataWedgeReactNative/blob/master/App.js).  This application requires a minimum version of 0.1.0 of this module.

```javascript
import DataWedgeIntents from 'react-native-datawedge-intents'
...
//  Register a receiver for the barcode scans with the appropriate action
DataWedgeIntents.registerBroadcastReceiver({
  filterActions: [
      'com.zebra.reactnativedemo.ACTION',
      'com.symbol.datawedge.api.RESULT_ACTION'
  ],
  filterCategories: [
      'android.intent.category.DEFAULT'
  ]
});
...
//  Declare a handler for broadcast intents
this.broadcastReceiverHandler = (intent) =>
{
  this.broadcastReceiver(intent);
}
...
//  Initiate a scan (you could also press the trigger key)
this.sendCommand("com.symbol.datawedge.api.SOFT_SCAN_TRIGGER", 'TOGGLE_SCANNING');
...
sendCommand(extraName, extraValue) {
  console.log("Sending Command: " + extraName + ", " + JSON.stringify(extraValue));
  var broadcastExtras = {};
  broadcastExtras[extraName] = extraValue;
  broadcastExtras["SEND_RESULT"] = this.sendCommandResult;
  DataWedgeIntents.sendBroadcastWithExtras({
    action: "com.symbol.datawedge.api.ACTION",
    extras: broadcastExtras});
}
```

## DataWedge

This module **requires the DataWedge service running** on the target device to be **correctly configured** to broadcast Android intents on each barcode scan with the appropriate action.  This can be achieved either manually or via an API, see the sample application readme files for a more thorough explanation.

### Output Plugin

Please also ensure you disable the keyboard output plugin to avoid undesired effects on your application: [thread](https://developer.zebra.com/message/95397).

For more information about DataWedge and how to configure it please visit Zebra [tech docs](http://techdocs.zebra.com/).  The DataWedge API that this module calls is detailed [here](http://techdocs.zebra.com/datawedge/latest/guide/api/)


