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

## Git fast commit & push

```
git add -A && git commit -m "update" && git push -u origin master
```