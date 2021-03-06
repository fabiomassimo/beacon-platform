# Android Beacon Service Demo App
A simple app that demonstrates usage of the [Proximity Beacon API](https://developers.google.com/beacons/proximity/).

## Requirements
The app was developed with [Android Studio](http://developer.android.com/sdk/) and targets the Android Lollipop 5.1 (API 22) platform.

## Registering the app with your Google Cloud Console project

The Proximity Beacon API is associated with your Google Cloud Console account and uses the standard GCC authorization mechanisms. Follow the instructions at the [Authorizing with Google for REST APIs](https://developers.google.com/android/guides/http-auth) guide to create a project and register the app with your account.

- At step 4 where it says "Enable the API you'd like to use by setting the Status to ON", search for `Google Proximity Beacon API` and set its status to enabled
- The package name is `com.google.sample.beaconservice`
- Note that there's currently a typo in how to get the SHA1 hash. The command you need to run is `keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore -list -v`. Note the space before the `-keystore` switch.
- Once you've created the client ID you're done.

## Dependencies
Enumerated in the app's [build.gradle](BeaconServiceDemoApp/app/build.gradle), we require:

- [Android 5.1](http://developer.android.com/about/versions/lollipop.html)
- [Google Play Services](https://developers.google.com/android/guides/overview) (for account management and authorization utilities)
- [Volley](https://developer.android.com/training/volley/index.html) (Async HTTP library)
- [Joda-Time](http://www.joda.org/joda-time/)

If you use Android Studio these dependencies are managed automatically for you. (When you first import the project you'll be asked to sync the relevant modules from the SDK manager.)

## Building and Running
Import the BeaconServiceDemoApp folder into Android Studio as an existing project and select `Run > Run app`.

When launched click the `scan` button to discover any nearby [Eddystone](https://github.com/google/eddystone) devices broadcasting an Eddystone-UID frame. (If you don't have an Eddystone device around you can use the [TxEddystone-UID app](https://github.com/google/eddystone/tree/master/eddystone-uid/tools/txeddystone-uid) to turn a compatible phone into one.) Detected beacons will be listed with their IDs, ordered from strongest signal to weakest.

The app will then use the [beacons.get](https://developers.google.com/beacons/proximity/reference/rest/v1beta1/beacons/get) method to fetch the status of each beacon, updating the beacons icon as the results are returned. The icons correspond to the beacon's status:

- ![check-circle](BeaconServiceDemoApp/app/src/main/res/drawable-hdpi/ic_action_check_circle.png) Registered to you and currently active (green) or inactive (orange)
- ![highlight-off](BeaconServiceDemoApp/app/src/main/res/drawable-hdpi/ic_action_highlight_off.png) Registered to you and decommissioned
- ![lock-open](BeaconServiceDemoApp/app/src/main/res/drawable-hdpi/ic_action_lock_open.png) Unregistered
- ![lock](BeaconServiceDemoApp/app/src/main/res/drawable-hdpi/ic_action_lock.png) Registered to someone else
- ![help](BeaconServiceDemoApp/app/src/main/res/drawable-hdpi/ic_action_help.png) Unknown status

Clicking on an entry takes you to the management screen for that beacon. There you'll be able to register the beacon, update the location, stability, description, activation status, and create and delete simple attachment data. Fields that are editable are marked with the ![mode-edit](BeaconServiceDemoApp/app/src/main/res/drawable-mdpi/ic_action_mode_edit.png) icon.
