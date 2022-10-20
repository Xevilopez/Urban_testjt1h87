# Video Recorder

This library allows you to record MP4 videos of HTML `canvas` elements, including those with WebGL contexts. It aims to support a wide range of real-world devices at interactive frame-rates.

## Starting Development

You can use this library by downloading a standalone zip containing the necessary files, by linking to our CDN, or by installing from NPM for use in a webpack project.

#### Standalone Download

Download the bundle from this link:
https://libs.zappar.com/zappar-videorecorder/1.0.1/zappar-videorecorder.zip

Unzip into your web project and reference from your HTML like this:
```html
<script src="zappar-videorecorder.js"></script>
```

#### CDN

Reference the zappar-videorecorder.js library from your HTML like this:
```html
<script src="https://libs.zappar.com/zappar-videorecorder/1.0.1/zappar-videorecorder.js"></script>
```

#### NPM Webpack Package

Run the following NPM command inside your project directory:
```bash
$ npm install --save @zappar/video-recorder
```

Then import the library into your JavaScript or TypeScript files:
```ts
import * as ZapparVideoRecorder from "@zappar/video-recorder";
```

## Usage

Use the `ZapparVideoRecorder.createCanvasVideoRecorder()` function to initialize a new video recorder, passing in your canvas, along with any desired options. The function returns a promise that will contain an object that you can use to control recording:

```ts
ZapparVideoRecorder.createCanvasVideoRecorder(canvas, {
  // Options
}).then(recorder => {

  // Use recorder to control recording

});
```

The available options are:
 - `autoFrameUpdate`: by default the recorder will automatically capture frames from the canvas every browser frame (once `recorder.start()` has been called). Pass `false` here to prevent this behavior. If you do, you will have to call `recorder.frameUpdate()` yourself to capture and encode frames from the canvas into the video.
 - `maxFrameRate`: default 30, use this to limit the maximum number of frames-per-second that will be captured and encoded into the video. Note that the actual frame-rate of the video may be lower than this value depending on the performance of the device.
 - `speed`: default 10, controls the trade-off between encoding time and quality of video between `0` (slowest encoding) and `10` (fastest encoding);
 - `quality`: default 25, controls the trade-off between video quality and file size between `10` (best video quality) and `51` (lowest video quality).
 - audio: default `true`, whether or not to record audio from the device microphone as part of the video.

The `recorder` object can be used to start and stop recording of the supplied canvas:
```ts
recorder.start();
// ...
recorder.stop();
```

The `recorder` object also provides a number of events that you can bind handler functions to.

The `onStart` event is triggered when video recording begins. Note that this can be some time after your call `recorder.start()` since a microphone permission dialog may be shown to the user before actual recording starts.
```ts
recorder.onStart.bind(() => {
  // Video recording has started
  // You may like to use this to show a 'stop recording' button
  // or to plan an on-screen animation during recording
});
```

The `onComplete` event is emitted when recording is finished. Note that this can be some time after your call `recorder.stop()` since it may take a few moments for video encoding to complete and the resulting video file to be available. The event passes a single argument to your event handler function - an object that you can use to access the final video file.
```ts
recorder.onComplete.bind(result => {
  // Use result to access the final video file
});
```

The `result` object has properties `arrayBuffer` and `blob` that contain the final MP4 video file. Alternatively the `asDataURL()` function returns a promise that resolves to a data URL containing the MP4 video file.
```ts
// Use the raw ArrayBuffer:
result.arrayBuffer

// Or a blob containing the same data:
result.blob

// Or get a data URL:
result.asDataURL().then(dataurl => {
  // Use dataurl
});
```

## Patent Licensing

This project includes encoders for AVC/H264 and AAC-LC. There are patent licensing implications to the use and deployment of this software - please seek your own legal advice and obtain licenses as appropriate.

## License

This project is built from multiple components with various licenses. They are included here:

 - Copyright (c) 2020 Trevor Sundberg, MIT License
 - fdk-aac: Copyright (c) 1995 - 2018 Fraunhofer-Gesellschaft zur Förderung der angewandten Forschung e.V. All rights reserved., Software License for The Fraunhofer FDK AAC Codec Library for Android
 - libmp4v2: libmp4v2 Contributors, Mozilla Public License version 1.1
