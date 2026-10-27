# rn-release-bundle-resets-metro-cache

![Build](https://github.com/YevheniiKotyrlo/rn-release-bundle-resets-metro-cache/workflows/Pre%20Merge%20Checks/badge.svg)

React Native 0.88.1's template app, where an Android release bundle empties the machine-wide Metro cache: the Gradle plugin's `BundleHermesCTask` always passes `--reset-cache`, and Metro's default cache root is `os.tmpdir()/metro-cache`.

## Reproduce

1. `cd ReproducerApp && yarn`
2. Leave an entry in Metro's cache root, as another bundler would: `mkdir -p "$TMPDIR/metro-cache/00" && echo x > "$TMPDIR/metro-cache/00/entry-from-another-bundler"` (on Windows, `%TEMP%`).
3. `cd android && ./gradlew :app:createBundleReleaseJsAndAssets`
4. `ls "$TMPDIR/metro-cache/00/entry-from-another-bundler"`: the entry is gone. The task ran `bundle --platform android --dev false --reset-cache --entry-file index.js`, and `metro-cache` holds only this bundle's entries.

# How to use this Reproducer

This project has been created with `npx @react-native-community/cli init` and is a vanilla React Native app.

> [!IMPORTANT]  
> Make sure you have completed the [React Native - Environment Setup](https://reactnative.dev/docs/set-up-your-environment) so that you have a working environment locally.

## Step 1: Start the Metro Server

First, you will need to start **Metro**, the JavaScript _bundler_ that ships _with_ React Native.

To start Metro, run the following command from the _root_ of your React Native project:

```bash
# using npm
npm start

# OR using Yarn
yarn start
```

## Step 2: Start your Application

Let Metro Bundler run in its _own_ terminal. Open a _new_ terminal from the _root_ of your React Native project. Run the following command to start your _Android_ or _iOS_ app:

### For Android

```bash
# using npm
npm run android

# OR using Yarn
yarn android
```

### For iOS

First, make sure you install dependencies with:

```bash
cd ios && bundle install && bundle exec pod install
```

Then you can run the iOS app with:

```bash
# using npm
npm run ios

# OR using Yarn
yarn ios
```

If everything is set up _correctly_, you should see your new app running in your _Android Emulator_ or _iOS Simulator_ shortly provided you have set up your emulator/simulator correctly.

This is one way to run your app — you can also run it directly from within Android Studio and Xcode respectively.

## Step 3: Modifying your App

Now that you have successfully run the app, let's modify it.

1. Open `App.tsx` in your text editor of choice and edit some lines.
2. For **Android**: Press the <kbd>R</kbd> key twice or select **"Reload"** from the **Developer Menu** (<kbd>Ctrl</kbd> + <kbd>M</kbd> (on Window and Linux) or <kbd>Cmd ⌘</kbd> + <kbd>M</kbd> (on macOS)) to see your changes!

   For **iOS**: Hit <kbd>Cmd ⌘</kbd> + <kbd>R</kbd> in your iOS Simulator to reload the app and see your changes!
