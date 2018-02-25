# my-dev-fixes
My curated list of dev hacks, notes and fixes.

## Unable to load script from assets 'index.android.bundle'. Make sure your bundle is packaged correctly or you're running a packager server.

`react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res`



## Error retrieving parent for item: No resource found that matches the given name blah blah Material.Widget.Button.Borderless.Colored

Your newly installed & linked library has sdk version conflicts with your project.

FIX IT.

## Error: EPERM: operation not permitted, unlink

* Execute `npm install --save` with `--no-bin-links`
* Or use npm 5.3 `npm install -g npm@5.3`
* Or use npm 5.0.3 `npm install -g npm@5.0.3`
* Or use npm 5.7.1 `npm install -g npm@5.7.1`
* Or use `yarn` LOL WORKED FOR ME
  * https://yarnpkg.com/lang/en/docs/migrating-from-npm/
  
## Android device not found

`adb reverse tcp:8081 tcp:8081 && adb reverse tcp:8097 tcp:8097 && adb reverse --list`

## Speeding up gradle builds

* https://hackernoon.com/how-to-speed-up-your-slow-gradle-builds-5d9a9545f91a
* https://medium.com/@wasyl/make-your-gradle-builds-fast-again-ea323ce6a435

## Building android apps

```
# clean shit:
cd android
gradlew clean

# build that shit
cd ..
react-native run-android
```

## Reactotron not connecting

```
# ensure config is correct
Reactotron
  .configure({
    host: 'localhost', // can be any IP address
	port: '9090' // default
    })
  // ...
  .connect()
  
# list forwarded ports:
adb reverse --list

# forward default port for Reactotron:
adb reverse tcp:9090 tcp:9090
```
## failed to execute 'importscripts' on 'workerglobalscope'
## React Native Yoganode Error

Both are code syntax errors.

## Time-sync error

1. Ensure your PC & Phone have same timezone settings
2. On Windows -> Date & Time -> Internet Time tab -> Change Settings -> Check Synchronize -> Click Update Now
3. On Device -> Settings -> Date & Time -> Toggle-on Auto updating -> Toggle it off
4. Restart / re-run your app.



## Windows NDIS based Internet Sharing Device Error on Tethered Devices

* Error codes 31 & 10

1. Restart PC
2. Restart Tethered Device

## Axios simple graphql client

```
const axios = require('axios');
axios.defaults.baseURL = 'http://localhost/';
axios.defaults.url = '/graphql';
axios.defaults.method = 'POST';
axios.defaults.headers.post['Content-Type'] = 'application/json';

axios
  .request({
    data: {
      query: `{
        hello
      }`
    }
  })
  .then((response)=>{
    console.log(response.data);
  })
  .catch((error)=>{
    console.log(error.response.data);
  });
```

## Binding EventEvemitter3 to an object

`npm install eventemitter3 --save`

```
const EventEmitter = require('eventemitter3');

Class SampleClass{
  constructor () {
    const emitter = new EventEmitter();
    this.emit = emitter.emit.bind(emitter);
    this.on = emitter.on.bind(emitter);
  }
  Test () {
    this.emit('ready');
  }
}

const Instance = new SampleClass();
Instance.on('ready', ()=>{
  console.log('ready!');
});
Instance.Test();
```

## Why GraphQL

* https://medium.com/@nzmebooks/taming-rest-apis-using-graphql-419cc9c0ec8
* https://medium.freecodecamp.org/rest-apis-are-rest-in-peace-apis-long-live-graphql-d412e559d8e4
* https://dev-blog.apollodata.com/graphql-vs-rest-5d425123e34b
* https://blog.codeship.com/documenting-graphql/

## How to GraphQL

* http://graphql.org/graphql-js/
* http://graphql.org/learn/schema/
* https://www.apollographql.com/docs/apollo-server/example.html
* https://www.apollographql.com/docs/graphql-tools/

## Google Cloud

* Regions & Zones - https://cloud.google.com/compute/docs/regions-zones/
* Reserving IP Address - https://cloud.google.com/compute/docs/ip-addresses/reserve-static-external-ip-address
* Datastore Entities - https://cloud.google.com/datastore/docs/concepts/entities#retrieving_an_entity
* Datastore Queries - https://cloud.google.com/datastore/docs/concepts/queries

## BitBucket

* Generating SSH Keys - https://confluence.atlassian.com/x/X4FmKw

## NodeJS

* NodeJS Ubuntu Installation - https://nodejs.org/en/download/package-manager/
* Express SSL - https://stackoverflow.com/a/11745114/9357445

---

## Free SSL & React Native Apps

Problem:
* Your react-native app can't reach your http server, since https is required
* Your https server can't be reached, because sites / endpoints using self-signed certificates are disallowed
* Long story short, XHR / axios / Webview can't reach your server

Solution:

1. Generate a CSR

* You can use Namecheap's tool - https://decoder.link/csr_generator
* Make sure you keep the private key, you'll use it later

2. Go get a Free SSL catered by Comodo.

* https://ssl.comodo.com/free-ssl-certificate.php
* The certificate bundle you'll get won't be a self-signed one, a commodo support representative confirmed this with me.
* Use your CSR, then validate your ownership
* After couple of minutes you'll receive your cert bundle in your email, with 4 files
  * `api_realtycoast_io.crt` (named after the domain I used, which is api.realtycoast.io)
  * `AddTrustExternalCARoot.crt`
  * `COMODORSAAddTrustCA.crt`
  * `COMODORSADomainValidationSecureServerCA.crt`
  
3. Do a CA/End-Entity Certificate matching

* Go to https://decoder.link/ca_matcher
* Paste the content of your `api_realtycoast_io.crt` equivalent as the END ENTITY CERTIFICATE
* Paste the content of your `COMODORSADomainValidationSecureServerCA.crt` as the CA CERTIFICATE
* Validate that shit, should be successful - if so, we proceed

4. Configure your Express SSL configuration

* Create a `/ssl` folder
* Put all those certificate files there
* Create a `private.key` file, PASTE YOUR PRIVATE KEY from STEP 1 there
* Now we use your site's certificate, your private key, and your CA certificate

```
const fs = require('fs');
const http = require('http');
const https = require('https');
const express = require('express');
const bodyParser = require('body-parser');

const SSL = {
  credentials: {
    key: fs.readFileSync('ssl/private.key', 'utf8'),
    cert: fs.readFileSync('ssl/api_realtycoast_io.crt', 'utf8'),
    ca: [
      fs.readFileSync('ssl/COMODORSADomainValidationSecureServerCA.crt', 'utf8')
    ]
  }
};

const app = express();
app.use(bodyParser.json());
app.get('/', (req, res) => res.send('Hello World!'));
http.createServer(app).listen(80);
https.createServer(SSL.credentials, app).listen(443);
```

* Git add, commit and push that shit
* Run your nodejs server again

5. Verify that shit

* Visit your `https://yoursite.com/` that should be working well
* Visit `https://www.sslshopper.com/ssl-checker.html`
* Check your server endpoint's url there, you should be seeing ALL CHECKS, AND NO X's

6. Rebuild your react-native app, and re-run it - should be workign well now

* Test code for webview

```
import { WebView, View } from 'react-native';

// in your component render(), below

return (
<View style={{flex:1}}>
	<WebView
	source={{uri: 'https://api.realtycoast.io/auth'}}
	javaScriptEnabled={true}
	domStorageEnabled={true}
	startInLoadingState={true}
	style={{ flex:1, height: 500, width: 350 }}
	/>
	</View>
);
```

* Test code for axios

```
	// replace your baseURL and url, below
	
          <Button onPress={
            ()=>{
              const x = axios.create({
                baseURL: 'https://api.realtycoast.io/',
                timeout: 10000,
                headers: {
                  'Accept': 'application/json',
                  'Content-Type': 'application/json',
                }
              });
            x.request({
              url: '/user/123'
            })
              .then(function (response) {
                console.log(response);
              })
              .catch(function (error) {
                console.log(error);
              });
            }
          }>
            <Text>Axios Test</Text>
          </Button>
```

---

## Git fast commit & push

```
git add -A && git commit -m "update" && git push -u origin master
```
