![screenshot](https://raw.githubusercontent.com/ellipticaldoor/vue-webpack-cordova/master/screenshot.jpg)

# feathers-cordova-oauth-example

> The original Vue Webpack template prepared for running cordova apps on ios and android

If you want to know how this was built [this blog post](https://ellipticaldoor.com/2017-10-22-making-work-the-vue-js-template-with-cordova/) explains how this works.

## Build Setup

``` bash
# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for mobile
npm run build-mobile

# run the ios / android app
cordova run ios
cordova run android

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## Dependencies
``` shell
npm install -g cordova
npm install
```

The ios dependencies (if you need to build an ios app) [only mac]
``` shell
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
npm install -g ios-sim ios-deploy

brew install cocoapods
pod setup
```

And the android dependencies. Here I'm using [cask](https://caskroom.github.io/) as package manager to install them, if you are not in mac use your favorite one.
``` shell
brew tap caskroom/cask
brew cask install caskroom/versions/java8

touch ~/.android/repositories.cfg

brew cask install android-sdk
brew install gradle
```

Add the mobile platforms that you need
``` shell
cordova platform add ios
# or / and
cordova platform add android
```

Check that everything is okay
``` shell
cordova requirements
```
