# my-dev-fixes
My curated list of dev hacks, notes and fixes.

#### Building android apps

```
cd android
gradlew clean
# clean shit
cd ..
react-native run-android
# build that shit
```

#### Reactotron not connecting

```
adb reverse --list
# list forwarded ports
adb reverse tcp:9090 tcp:9090
# forward default port for Reactotron
```
