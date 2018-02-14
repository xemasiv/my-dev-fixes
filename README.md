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

## ~
git add -A && git commit -m "update" && git push -u origin master