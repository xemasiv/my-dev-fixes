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

## React Native Yoganode Error

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

## Git fast commit & push

```
git add -A && git commit -m "update" && git push -u origin master
```
