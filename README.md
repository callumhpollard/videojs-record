Video.js Record
===============

A Video.js plugin for recording audio/video/image files.

![Screenshot](examples/img/screenshot.png?raw=true "Screenshot")

[![npm version](https://img.shields.io/npm/v/videojs-record.svg?style=flat)](https://www.npmjs.com/package/videojs-record)
[![npm](https://img.shields.io/npm/dm/videojs-record.svg)]()
[![License](https://img.shields.io/npm/l/videojs-record.svg)](LICENSE)
[![Build Status](https://travis-ci.org/collab-project/videojs-record.svg?branch=master)](https://travis-ci.org/collab-project/videojs-record)

Installation
------------

Use [bower](http://bower.io) (`bower install videojs-record`) or
[npm](https://www.npmjs.org) (`npm install videojs-record`) to install
the plugin or download and include the dependencies manually.

Dependencies
------------

The plugin has the following mandatory dependency:

- [Video.js](https://github.com/videojs/video.js) - HTML5 media player that provides the user interface.

When recording audio and/or video you also need:

- [RecordRTC.js](https://github.com/muaz-khan/RecordRTC) - Adds support for audio/video/GIF recording.

And when recording audio-only, the following dependencies are also required:

- [wavesurfer.js](https://github.com/katspaugh/wavesurfer.js) - Provides a navigable waveform for audio files. Comes with a [microphone plugin](http://www.wavesurfer-js.org/example/microphone) used for realtime visualization of the microphone audio signal.
- [videojs-wavesurfer](https://github.com/collab-project/videojs-wavesurfer) - Transforms Video.js into an audio-player.

Usage
-----

Start by including the video.js stylesheet and library:

```html
<link href="http://vjs.zencdn.net/4.12.12/video-js.css" rel="stylesheet">
<script src="http://vjs.zencdn.net/4.12.12/video.js"></script>
```

If you're going to record audio and/or video you need to include RecordRTC as well:

```html
<script src="http://cdn.webrtc-experiment.com/RecordRTC.js"></script>
```

The videojs-record plugin automatically registers itself when the script
is included on the page:

```html
<script src="videojs.record.js"></script>
```

Add the extra stylesheet for the plugin that includes a
[custom font](src/css/font) with additional icons:

```html
<link href="videojs.record.css" rel="stylesheet">
```

### Audio/video/image

When recording either audio/video, video-only, animated GIF or a single image,
include a `video` element:

```html
<video id="myVideo" class="video-js vjs-default-skin"></video>
```

Check out the full audio/video
([demo](http://collab-project.github.io/videojs-record/examples/audio-video.html) / [source](https://github.com/collab-project/videojs-record/blob/master/examples/audio-video.html)),
image ([demo](http://collab-project.github.io/videojs-record/examples/image-only.html) / [source](https://github.com/collab-project/videojs-record/blob/master/examples/image-only.html)),
animated GIF ([demo](http://collab-project.github.io/videojs-record/examples/animated-gif.html) / [source](https://github.com/collab-project/videojs-record/blob/master/examples/animated-gif.html)) or the
video-only ([demo](http://collab-project.github.io/videojs-record/examples/video-only.html) or [source](https://github.com/collab-project/videojs-record/blob/master/examples/video-only.html)) examples.

Note that recording both audio and video into a single WebM file is currently
only supported in Mozilla Firefox >= 29. In the Chrome browser two separate
Blob objects are created: one for audio and one for video.

### Audio-only

![Audio-only screenshot](examples/img/audio-only.png?raw=true "Audio-only screenshot")

When recording audio-only, also include the wavesurfer.js library and
the videojs-wavesurfer and microphone plugins. Make sure to place this before
the `videojs.record.js` script.

```html
<script src="http://wavesurfer-js.org/dist/wavesurfer.min.js"></script>
<script src="http://wavesurfer-js.org/dist/plugin/wavesurfer.microphone.min.js"></script>
<script src="videojs.wavesurfer.js"></script>
```

And define an `audio` element:

```html
<audio id="myAudio" class="video-js vjs-default-skin"></audio>
```

Check out the full audio-only example ([demo](http://collab-project.github.io/videojs-record/examples/audio-only.html) / [source](https://github.com/collab-project/videojs-record/blob/master/examples/audio-only.html)).

Options
-------

Configure the player using the video.js
[options](https://github.com/videojs/video.js/blob/master/docs/guides/options.md),
and enable the plugin by adding a `record` configuration to `plugins`. For
example:

```javascript
var player = videojs("myVideo",
{
    controls: true,
    loop: false,
    width: 320,
    height: 240,
    plugins: {
        record: {
            image: false,
            audio: false,
            video: true,
            maxLength: 5
        }
    }
});
```

The available options for this plugin are:

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `image` | boolean | `false` | Create single snapshot image. |
| `audio` | boolean | `false` | Include audio in the recorded clip. |
| `video` | boolean | `false` | Include video in the recorded clip. |
| `animation` | boolean | `false` | Animated GIF. |
| `maxLength` | float | `10` | Maximum length of the recorded clip. |
| `audioBufferSize` | float | `4096` | The size of the audio buffer (in sample-frames per second). Legal values: 0, 256, 512, 1024, 2048, 4096, 8192 and 16384. |
| `audioSampleRate` | float | `22050` | The audio sample rate (in sample-frames per second) at which the `AudioContext` handles audio. Legal values are in the range of 22050 to 96000. |
| `animationFrameRate` | float | `200` | Frame rate for animated GIF (in frames per second). |
| `animationQuality` | float | `10` | Sets quality of color quantization (conversion of images to the maximum 256 colors allowed by the GIF specification). Lower values (minimum = 1) produce better colors, but slow processing significantly. The default produces good color mapping at reasonable speeds. Values greater than 20 do not yield significant improvements in speed. |
| `debug` | boolean | `false` | Enables console log messages in RecordRTC. |

Methods
-------

Methods for this plugin:

| Method | Description |
| --- | --- |
| `isRecording` | Returns a boolean indicating whether recording is active or not. |
| `getRecordType` | Get recorder type as string. Either `image_only`, `animation`, `audio_only`, `video_only` or `audio_video`. |
| `destroy` | Destroys the recorder instance and children (including the video.js player). |

Events
------

Plugin events that are available on the video.js player instance:

| Event | Description |
| --- | --- |
| `deviceReady` | The webcam or microphone is ready to use. |
| `deviceError` | User doesn't allow the browser to access the webcam and/or microphone. Check `player.deviceErrorCode` for the specific [error code](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getUserMedia#errorCallback). |
| `startRecord` | User pressed the record or camera button to start recording. |
| `stopRecord` | User pressed the stop button to stop recording. |
| `finishRecord` | The recorded stream or image is available. Check the `player.recordedData` object for the recorded data. |

Get recorded data
-----------------

Listen for the `finishRecord` event and obtain the recorded data from the
`player.recordedData` object for further processing:

```javascript
// user completed recording and stream is available
player.on('finishRecord', function()
{
    // the recordedData object contains the stream data that
    // can be downloaded by the user, stored on server etc.
    console.log('finished recording: ', player.recordedData);
});
```

Note that in the Chrome browser `player.recordedData` returns an object with
`audio` and `video` properties when recording both audio/video. In Firefox
it returns a single WebM Blob object containing both audio and video.

Customizing controls
--------------------

To disable and hide specific controls, use the video.js `children`
option:

```javascript
children: {
    controlBar: {
        children: {
            // hide fullscreen control
            fullscreenToggle: false
        }
    }
},
```

Localization
------------

This plugin supports multiple languages. Each language has it's own file
(found in the `lang` directory) that contains the translated text.

Using a different language, for example Dutch (`nl`), is done by including
the Video.js language file and the plugin's language file:

```html
<script src="videojs-record/dist/lang/nl.js"></script>
<script src="http://vjs.zencdn.net/4.12.12/lang/nl.js"></script>
```

And setting the Video.js player's `language` option:

```javascript
language: "nl"
```

Adding support
for an additional language is done by adding a new file with a filename that
consists of the countrycode and the `.json` extension, eg. `fr.json`. The
[build script](#development) compiles the JSON into a usable language file,
eg. `fr.js`. Check the Video.js wiki for a
[list of supported countrycodes](https://github.com/videojs/video.js/blob/master/docs/guides/languages.md#language-codes).
Pull requests to add more languages to this plugin are always welcome!


More features using other plugins
---------------------------------

The Video.js community created
[lots of plugins](https://github.com/videojs/video.js/wiki/Plugins)
that can be used to enhance the player's functionality. Plugins actually
tested with `videojs-record`:

- [videojs-persistvolume](https://github.com/theonion/videojs-persistvolume) -
  Saves user's volume setting using `localStorage`, but falls back to cookies
  if necessary.

Development
-----------

Install `grunt-cli`:

```
sudo npm install -g grunt-cli
```

Install dependencies using npm:

```
npm install
```

Or using bower:

```
bower install
```

Build a minified version:

```
grunt
```

Generated files are placed in the `dist` directory.

License
-------

This work is licensed under the [MIT License](LICENSE).
