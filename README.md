# Custom video_360 

A fork of the Flutter Video 360 package with added functionality for:
- Loading 360 videos from assets.
- Dynamically switching video sources from assets at runtime.

**Note:** This functionality is only implemented for Android.

# Notice

Flutter Version <= 2.10.5 used this plugin version 0.0.6<br>
Flutter Version >= 3.0.0 used this plugin version 0.0.8

## Getting Started

The Android uses the open source [AndroidX Media3](https://github.com/androidx/media)

> AndroidX Media3 : 1.1.1 (via Google ExoPlayer Version: 2.19.1)

The iOS uses the open source [Swifty360Player](https://github.com/abdullahselek/Swifty360Player)

> Swifty360Player Version: 0.2.5

## Installation

Add pubspec.yaml dependencies.

```dart
dependencies:
  video_360: ^0.0.9
```

Android Requirements

> Minimum SDK Target : 19

iOS Requirements

> Minimum iOS Target : 11.0<br>
> Swift Version : 5.x

## How to use

importing the libray:

```dart
import 'package:video_360/video_360.dart';
```

Add Video360View:

```dart
Video360View(
    onVideo360ViewCreated: _onVideo360ViewCreated,
    url: YOUR_360_VIDEO_URL,    
    assetPath: YOUR_ASSET_PATH // added assetPath attribute
    isRepeat: true, // defalut : false
    onPlayInfo: (Video360PlayInfo info) {
        // Play info Callback
    },
)
```

Video360Controller Method

> play() : video play<br>
> stop() : video stop<br>
> reset() : video reset<br>
> jumpTo() : video jump, parameter is millisecond<br>
> seekTo() : video seek, parameter is plus, minus millisecond <br>
> **changeAssetSource(String assetPath)** : added method for changing video source from assets

sample code:

```dart
import 'package:flutter/material.dart';
import 'package:video_360/video_360.dart';

void main() {
  WidgetsFlutterBinding.ensureInitialized();
  runApp(MaterialApp(home: MyApp()));
}

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {

  Video360Controller? controller;

  String durationText = '';
  String totalText = '';

  @override
  void initState() {
    super.initState();
  }

  @override
  Widget build(BuildContext context) {

    var statusBar = MediaQuery.of(context).padding.top;

    var width = MediaQuery.of(context).size.width;
    var height = MediaQuery.of(context).size.height;

    return Scaffold(
      appBar: AppBar(
        title: const Text('Video 360 Plugin example app'),
      ),
      body: Stack(
        children: [
          Center(
            child: Container(
              width: width,
              height: height,
              child: Video360View(
                onVideo360ViewCreated: _onVideo360ViewCreated,
                url: 'https://bitmovin-a.akamaihd.net/content/playhouse-vr/m3u8s/105560.m3u8',
                onPlayInfo: (Video360PlayInfo info) {
                  setState(() {
                    durationText = info.duration.toString();
                    totalText = info.total.toString();
                  });
                },
              ),
            ),
          ),
          Column(
            children: [
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  MaterialButton(
                    onPressed: () {
                      controller?.play();
                    },
                    color: Colors.grey[100],
                    child: Text('Play'),
                  ),
                  MaterialButton(
                    onPressed: () {
                      controller?.stop();
                    },
                    color: Colors.grey[100],
                    child: Text('Stop'),
                  ),
                  MaterialButton(
                    onPressed: () {
                      controller?.reset();
                    },
                    color: Colors.grey[100],
                    child: Text('Reset'),
                  ),
                  MaterialButton(
                    onPressed: () {
                      controller?.jumpTo(80000);
                    },
                    color: Colors.grey[100],
                    child: Text('1:20'),
                  ),
                ],
              ),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  MaterialButton(
                    onPressed: () {
                      controller?.seekTo(-2000);
                    },
                    color: Colors.grey[100],
                    child: Text('<<'),
                  ),
                  MaterialButton(
                    onPressed: () {
                      controller?.seekTo(2000);
                    },
                    color: Colors.grey[100],
                    child: Text('>>'),
                  ),
                  Flexible(
                    child: MaterialButton(
                      onPressed: () {
                      },
                      color: Colors.grey[100],
                      child: Text(durationText + ' / ' + totalText),
                    ),
                  ),
                ],
              )
            ],
          )
        ],
      ),
    );
  }

  _onVideo360ViewCreated(Video360Controller? controller) {
    this.controller = controller;
    this.controller.play();
  }
}
```
