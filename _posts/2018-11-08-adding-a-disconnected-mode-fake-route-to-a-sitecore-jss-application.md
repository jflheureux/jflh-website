---
title: Adding a Disconnected Mode Fake Route to a Sitecore JSS Application
tags: [Sitecore, JSS]
sharing:
  linkedin: Ever needed to fake a Sitecore route while developing a JSS application? Search no more and read my latest blog post.
---

When working with Sitecore JavaScript Services (JSS) in disconnected mode, there is a fake layout service that returns data from the route files. Sometimes you need to fake another endpoint or route that exist in Sitecore, but not in disconnected mode.

<!-- more -->

That was my case when I implemented Coveo for Sitecore in the JSS sample React application. I wanted to create a fake Coveo REST API endpoint only in disconnected mode and use the real endpoint already available in Sitecore for the deployed JSS application. My fake endpoint only needed to serve the same static response over and over. I did not need anything dynamic.

## Disconnected Mode Proxy Server

The first stop is the disconnected mode proxy script (`\scripts\disconnected-mode-proxy.js`). After inspection of the `@sitecore-jss/sitecore-jss-dev-tools` library, I learned that the proxy options object passed to the `createDefaultDisconnectedServer` function support one more callback not implemented by default: `afterMiddlewareRegistered(app)`.

`afterMiddlewareRegistered` receives an `app` object which is the [Express](https://expressjs.com/) app instance of the disconnected mode server. Thus, you can setup the Express app the way you want in this callback. To add a fake endpoint serving a static file, all you have to do is something like this:

```javascript
const coveoRestApiSearchResponse = require('../coveo/rest/v2/searchResponse.json');

const proxyOptions = {
  // Other options,
  afterMiddlewareRegistered: (app) => {
    app.post('/coveo/rest/v2', (req, res) => res.send(coveoRestApiSearchResponse));
  },
};
```

This example adds a `/coveo/rest/v2` fake route that serves the content of the `/coveo/rest/v2/searchResponse.json` project file.

## React Proxy Setup

Unfortunately, the above code is not enough as the React router will still try to resolve the route inside your application. You need to setup the React proxy to re-route calls to the disconnected mode proxy server. This is done differently in Tech Preview 4 (TP4) and the Generally Available (GA) version still to be released at the time of writing these lines.

### Technical Preview 4 (TP4)

The change must be done in the `package.json` file at the root of your project. Locate the `proxy` object and add a property for your fake route.

```json
{
  "proxy": {
    "/coveo/rest/v2": {
      "target": "http://localhost:3042"
    }
  }
}
```

### Version 10.0 (GA)

As the version 10.0 of JSS uses `create-react-app` and `react-scripts` version 2, the change must be made in the `/src/setupProxy.js` file. Simply add your fake route to the module exports.

```javascript
module.exports = (app) => {
  // Existing proxy rules
  app.use(proxy('/coveo/rest/v2', { target: 'http://localhost:3042/' }));
};
```

The next time you start the application in disconnected mode using `jss start`, all calls to your fake route will return the same fake response.