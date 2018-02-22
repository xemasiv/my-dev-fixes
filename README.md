# my-dev-fixes
My curated list of dev hacks, notes and fixes.

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

## Git fast commit & push

```
git add -A && git commit -m "update" && git push -u origin master
```
