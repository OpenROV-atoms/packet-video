# packet-video
This is a webcomponent that allows streaming live real-time .mp4 video files packet by packet using Media Source Extensions which injects the video packets directly in to the browsers graphic video decoding subsystem for optimal playback.  Great for streaming video over socket.io or websockets.

This component has been tuned for low-latency video delivery (no buffering).

The component works and has passing unit tests.

The incoming video has to be a `segmented mp4` file.

The current system only supports a single video stream at the moment.  If you want to send audio, you will need to send it separately.

To install:
```
bower install https://github.com/OpenROV-atoms/packet-video.git
```

Example usage:
```
<!doctype html>
<html>
  <head>
    <script src="../bower_components/webcomponentsjs/webcomponents-lite.js"></script>
    <link rel="import" href="../bower_components/packet-video.html">
  </head>
  <body>
    <packet-video id='video'>
      <h2>Text to overlay the video</h2>
    </packet-video>
...
  <script>
    var v = document.getElementById("video");
    var initialFrame = //some means to get the initial frame of the segmented mp4
    v.init(initialFrame);
    socket.on('data',function(data){
        v.append(data);  //both the data and the initFrame need to be UInt8Array
    });
```

Running the unit tests:
The tests have a dependency on Google's web component tester (https://github.com/Polymer/web-component-tester) which should be installed via npm in to your local environment.

To run the tests:
```
wct
```

The included wct.conf.json file has a list of local browsers it attempts to test with, adjust it as needed.




Issues affecting live H264 streaming (Updated 2016 Mar 29)
=====================================

currentTime as reported by HTML5 `<video>` is not frame-accurate
https://code.google.com/p/chromium/issues/detail?id=555376

MSE - remove() to escape QuotaExceededError is leading to decode error later
https://code.google.com/p/chromium/issues/detail?id=529990

videoTag.currentTime should not exceed videoTag.buffered.end(N) for the currently playing range
https://code.google.com/p/chromium/issues/detail?id=414828
