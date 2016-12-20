# Sample apps using Layer Web SDK

This repository contains sample chat applications that demonstrate different ways of building a Web Application with Layer.  Sample apps are built using Backbone, Angular and React; each app is built once with the Layer WebSDK, and once with the Layer WebSDK + Layer UI for Web (Layer UI Framework).

## The Web SDK

These sample apps are built using the [Layer WebSDK](https://docs.layer.com/sdk/web/introduction).  These samples support two message types:
`text/plain` which is any normal message you send, and `text/quote` which is any Message you send that starts with `> `.

### React

The [React App](./websdk-samples/react) uses:

* Redux to implement a flux architecture
* Layer-react to add Layer Query data to the state
* NPM to include the Layer WebSDK

### Angular 1.x

The [Angular App](./websdk-samples/angular) uses:

* CDN to include the Layer WebSDK
* Standard Angular 1.x templates and controllers to work with Layer Query data

### Backbone

The [Backbone App](./websdk-samples/backbone) uses:

* CDN to include the Layer WebSDK
* Standard Backbone Views to work with Layer Query data


## Layer UI for Web

[Layer UI for Web](http://static.layer.com/layer-ui-web-beta/docs/) is an Early Beta release of a library of Webcomponent widgets for rendering Layer data and events.
To simplify use of these widgets  from various frameworks, the library  ships with Adapters to provision its widgets in a manner friendlier to other frameworks.  Examples and initialization code for these is shown below.

Note that all calls to `LayerUI.init()` take a recommended `appId` as input; these samples don't follow this recommended practice because the `appId` is not know at load time, and must be entered by the user of the sample app.  Instead `appId` is provided directly to each Web Component.

Out of the box, this library provides the sample apps with support for:

* Images
* Videos
* Links to youtube videos
* Links to images
* Emojis

### React with Layer UI

The [React App](./ui-web-samples/react) uses a single `src/layer-ui-adapters.js` file, which is imported by any Component that needs React Views created from the Webcomponent widgets.  This file can be included from anywhere/everywhere that you need to insure you have an initialized UI library
and initialized React Components.  This is a useful pattern for many apps:

```javascript
import * as Layer from 'layer-websdk';
LayerUI.init({
  appId: 'layer:///apps/staging/my-app-id',
  layer: Layer
});

const LayerUIWidgets = LayerUI.adapters.react(React, ReactDom);
module.exports = LayerUIWidgets;
```

`LayerUIWidgets` is now a library of React components wrapping the Layer Webcomponents, and is available to all of your Components.

### Angular 1.x with Layer UI

The [Angular App](./ui-web-samples/angular) uses the Layer UI Angular adapter to define the Angular Directives needed to recognize and work with Layer's Webcomponents.  Thus with the following intialization code:

```javascript
window.layerUI.init({
  appId: 'layer:///apps/staging/my-app-id',
  layer: window.layer
});

// Define the "layerUIControllers" controller
window.layerUI.adapters.angular(angular);


var app = angular.module('ChatApp', [
      "layerUIControllers", ...
]);
```

We have now initialized the directives that let us have partials such as:

```html
<layer-conversation
    ng-delete-message-enabled="true"
    ng-app-id="appCtrlState.client.appId"
    ng-conversation-id="chatCtrlState.currentConversation.id"
    ng-compose-buttons="composeButtons"></layer-conversation>
```

You can use any documented property of the webcomponents, but its recommended to prefix it with `ng-` so that scope can be evaluated prior to passing the value into the webcomponent.


### Backbone with Layer UI

The [Backbone App](./ui-web-samples/backbone) uses the Layer UI Backbone adapter to create a library of Backbone Views:

```javascript
window.layerUI.init({
  appId: 'layer:///apps/staging/my-app-id',
  layer: window.layer
});
var LayerUIWidgets = window.layerUI.adapters.backbone(Backbone);
var NotifierView = LayerUIWidgets.Notifier;
var ConversationsListView = LayerUIWidgets.ConversationList;
var ConversationView = LayerUIWidgets.Conversation;
```

After initializing the UI Framework, generate the Backbone views so that they can be instantiated and used.


## Authentication

For demonstration purposes Layer provides a sample authentication endpoint which works for Layer **Staging Applications only**. It exposes a global `window.layerSample` utility and injects an initial HTML login dialog.

Layer Staging Application IDs can be found in your [Developer Dashboard](https://developer.layer.com/projects/keys).

> In real application this should be replaced with your own authentication mechanism and manage your own user identities.


## Sample Users

Sample Users should have been setup for you automatically.  If your app does NOT have users setup for you already, you will see an alert when running the application, and you can use the following commands to setup your app with the correct Identities.  You can find YOUR_APP_ID in the `Keys` section of the Developer Dashboard; you can find YOUR_TOKEN in the `Integration` section of the Developer Dashbaord.

```
curl  -X POST \
      -H 'Accept: application/vnd.layer+json; version=1.1' \
      -H 'Authorization: Bearer YOUR_TOKEN' \
      -H 'Content-Type: application/json' \
      -d '{"display_name": "User 0", "avatar_url": "https://s3.amazonaws.com/static.layer.com/sdk/sampleavatars/0.png"}' \
      https://api.layer.com/apps/YOUR_APP_ID/users/0/identity

curl  -X POST \
      -H 'Accept: application/vnd.layer+json; version=1.1' \
      -H 'Authorization: Bearer YOUR_TOKEN' \
      -H 'Content-Type: application/json' \
      -d '{"display_name": "User 1", "avatar_url": "https://s3.amazonaws.com/static.layer.com/sdk/sampleavatars/1.png"}' \
      https://api.layer.com/apps/YOUR_APP_ID/users/1/identity

curl  -X POST \
      -H 'Accept: application/vnd.layer+json; version=1.1' \
      -H 'Authorization: Bearer YOUR_TOKEN' \
      -H 'Content-Type: application/json' \
      -d '{"display_name": "User 2", "avatar_url": "https://s3.amazonaws.com/static.layer.com/sdk/sampleavatars/2.png"}' \
      https://api.layer.com/apps/YOUR_APP_ID/users/2/identity

curl  -X POST \
      -H 'Accept: application/vnd.layer+json; version=1.1' \
      -H 'Authorization: Bearer YOUR_TOKEN' \
      -H 'Content-Type: application/json' \
      -d '{"display_name": "User 3", "avatar_url": "https://s3.amazonaws.com/static.layer.com/sdk/sampleavatars/3.png"}' \
      https://api.layer.com/apps/YOUR_APP_ID/users/3/identity

curl  -X POST \
      -H 'Accept: application/vnd.layer+json; version=1.1' \
      -H 'Authorization: Bearer YOUR_TOKEN' \
      -H 'Content-Type: application/json' \
      -d '{"display_name": "User 4", "avatar_url": "https://s3.amazonaws.com/static.layer.com/sdk/sampleavatars/4.png"}' \
      https://api.layer.com/apps/YOUR_APP_ID/users/4/identity

curl  -X POST \
      -H 'Accept: application/vnd.layer+json; version=1.1' \
      -H 'Authorization: Bearer YOUR_TOKEN' \
      -H 'Content-Type: application/json' \
      -d '{"display_name": "User 5", "avatar_url": "https://s3.amazonaws.com/static.layer.com/sdk/sampleavatars/5.png"}' \
      https://api.layer.com/apps/YOUR_APP_ID/users/5/identity

curl  -X POST \
      -H 'Accept: application/vnd.layer+json; version=1.1' \
      -H 'Authorization: Bearer YOUR_TOKEN' \
      -H 'Content-Type: application/json' \
      -d '{"distinct": false, "participants": ["0","1","2","3","4","5"]}' \
      https://api.layer.com/apps/YOUR_APP_ID/conversations
