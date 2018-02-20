## react-native-af-video-player

[![npm version](http://img.shields.io/npm/v/react-native-af-video-player.svg?style=flat-square)](https://npmjs.org/package/react-native-af-video-player "View this project on npm")
[![npm downloads](http://img.shields.io/npm/dm/react-native-af-video-player.svg?style=flat-square)](https://npmjs.org/package/react-native-af-video-player "View this project on npm")
[![npm licence](http://img.shields.io/npm/l/react-native-af-video-player.svg?style=flat-square)](https://npmjs.org/package/react-native-af-video-player "View this project on npm")
[![Platform](https://img.shields.io/badge/platform-ios%20%7C%20android-989898.svg?style=flat-square)](https://npmjs.org/package/react-native-af-video-player "View this project on npm")
[![npm](https://img.shields.io/npm/dt/react-native-af-video-player.svg?style=flat-square)](https://npmjs.org/package/react-native-af-video-player "View this project on npm")

A customisable React Native video player for Android and IOS

![Demo](https://github.com/abbasfreestyle/react-native-af-video-player/blob/master/demo.gif)

## Features

* Fullscreen support for Android and iOS!
* Works with react-navigation
* Optional action button for custom use
* Add your own logo and/or placeholder

## Install

```shell
npm i -s react-native-af-video-player
```

Then link

```shell
react-native link react-native-video
react-native link react-native-keep-awake
react-native link react-native-vector-icons
react-native link react-native-orientation
react-native link react-native-linear-gradient
```

## Simple Usage

```jsx
import React from 'react'
import { AppRegistry, StyleSheet, View } from 'react-native'
import Video from 'react-native-af-video-player'

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center'
  }
})

const url = 'https://your-url.com/video.mp4'

class VideoExample extends React.Component {

  render() {
    return (
      <View style={styles.container}>
        <Video url={url} />
      </View>
    )
  }
}

AppRegistry.registerComponent('VideoExample', () => VideoExample)
```

## Props

Prop                  | Type     | Required | Default                   | Description
--------------------- | -------- | -------- | ------------------------- | -----------
url                   | string   | Yes      |                           | A URL string is required.
autoPlay              | bool     | No       | false                     | Autoplays the video as soon as it's loaded
loop                  | bool     | No       | false                     | Allows the video to continuously loop
title                 | string   | No       | ''                        | Adds a title of your video at the top of the player
placeholder           | string   | No       | undefined                 | Adds an image placeholder while it's loading and stopped at the beginning
logo                  | string   | No       | undefined                 | Adds an image logo at the top left corner of the video
theme                 | string   | No       | 'white'                   | Adds an optional theme colour to the players controls
resizeMode            | string   | No       | 'contain'                 | Fills the whole screen at aspect ratio. contain, cover etc
rotateToFullScreen    | bool     | No       | false                     | Tapping the fullscreen button will rotate the screen. Also rotating the screen will automatically switch to fullscreen mode
fullScreenOnly        | bool     | No       | false                     | This will play only in fullscreen mode
inlineOnly            | bool     | No       | false                     | This hides the fullscreen button and only plays the video in inline mode
playInBackground      | bool     | No       | false                     | Audio continues to play when app enters background.
playWhenInactive      | bool     | No       | false                     | [iOS] Video continues to play when control or notification center are shown.
rate                  | number   | No       | 1                         | Adjusts the speed of the video. 0 = stopped, 1.0 = normal
volume                | number   | No       | 1                         | Adjusts the volume of the video. 0 = mute, 1.0 = full volume
onMorePress           | function | No       | undefined                 | Adds an action button at the top right of the player. Use this callback function for your own use. e.g share link
onFullScreen          | function | No       |                           | Returns the fullscreen status whenever it toggles. Useful for situations like react navigation.
onTimedMetadata       | function | No       | undefined                 | Callback when the stream receives metadata
contentAbove          | node     | No       | null                      | This adds content above the video inside a ScrollView
contentBelow          | node     | No       | null                      | This adds content below the video inside a ScrollView
scrollBounce          | bool     | No       | false                     | Enables the bounce effect for the ScrollView
lockPortraitOnFsExit  | bool     | No       | false                     | Keep Portrait mode locked after Exiting from Fullscreen mode

### Referencing

To toggle play/pause manually, you can do it like so:

```jsx

  class MyComponent extends Component {

    play() {
      this.video.play()
    }

    pause() {
      this.video.pause()
    }

    render() {
      return (
        <View>
          <Video
            url={url}
            ref={(ref) => { this.video = ref }}
          />
          <Button onPress={() => this.play()}>Play</Button>
          <Button onPress={() => this.pause()}>Pause</Button>
        </View>
      )
    }
  }
```

## Issues

### Container

Avoid adding alignItems: 'center' to the container, it can cause fullscreen mode to disappear :D

### React Navigation

If you’re using react-navigation you need to manually hide the headers / tab bars to take advantage of fullscreen videos.

### Example

```jsx
import React, { Component } from 'react'
import { StyleSheet, View, ScrollView, Alert, Text } from 'react-native'

import Video from 'react-native-af-video-player'

const styles = StyleSheet.create({
  container: {
    flex: 1
  }
})

class ReactNavigationExample extends Component {

  static navigationOptions = ({ navigation }) => {
    const { state } = navigation
    // Setup the header and tabBarVisible status
    const header = state.params && (state.params.fullscreen ? undefined : null)
    const tabBarVisible = state.params ? state.params.fullscreen : true
    return {
      // If you're using stack navigators, you can hide the header bar like so
      header,
      // If you're using the tab navigators, you can hide the tab bar like so
      tabBarVisible,
    }
  }

  onFullScreen(status) {
    // Set the params to pass in the fullscreen status to navigationOptions
    this.props.navigation.setParams({
      fullscreen: !status
    })
  }

  onMorePress() {
    Alert.alert(
      'Boom',
      'This is an action call!',
      [{ text: 'Aw yeah!' }]
    )
  }

  render() {

    const url = 'https://your-url.com/video.mp4'
    const logo = 'https://your-url.com/logo.png'
    const placeholder = 'https://your-url.com/placeholder.png'
    const title = 'My video title'

    return (
      <View style={styles.container}>
        <Video
          autoPlay
          url={url}
          title={title}
          logo={logo}
          placeholder={placeholder}
          onMorePress={() => this.onMorePress()}
          onFullScreen={status => this.onFullScreen(status)}
          fullScreenOnly
        />
        <ScrollView>
          <Text>Some content here...</Text>
        </ScrollView>
      </View>
    )
  }
}

export default ReactNavigationExample

```

### http vs https

For your sanity you should use https especially if you’re planning to use this for iOS. Using http will not work due to App Transport Security Settings will result in AppStore rejection.

### Fullscreen videos inside a ScrollView

If you want a video inside a ScrollView. Simply use the props contentAbove and/or contentBelow like so:
The reason for this is because we need to hide all of it's content due to ScrollView styling challenges when enabling fullscreen mode. We wouldn't want you deal with that headache, instead let this component handle it :)

### Example

```jsx

  const styles = StyleSheet.create({
    container: {
      flex: 1
    }
  })

  class VideoInScrollView extends Component {
    constructor(){
      super()
      this.state = {
        fullScreen: false
      }
    }

    onFullScreen(fullScreen) {
      this.setState({ fullScreen })
    }

    render() {
      const { fullScreen } = this.state
      return (
        <View style={styles.container}>
          <Video
            autoPlay
            url={url}
            title={title}
            logo={logo}
            placeholder={logo}
            rotateToFullScreen
            onFullScreen={status => this.onFullScreen(status)}
            contentAbove={
              <Text>Some content above</Text>
            }
            contentBelow={
              <Text>Some content below</Text>
            }
          />
        </View>
      )
    }
  }
```

## To Do

* Improve scrubber controls for iOS
* Customise specific components for better theming
* ~~Provide fullscreen support within a ScrollView~~

---

**MIT Licensed**
