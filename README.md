# SoundTouchJS

SoundTouchJS is a JavaScript audio time-stretching and pitch-shifting library, based on the C++ implementation of
[Soundtouch](https://www.surina.net/soundtouch/). The earliest implementation in JavaScript was written by
[Ryan Berdeen](https://github.com/also/soundtouch-js) and later expanded by [Jakub Faila](https://github.com/jakubfiala/soundtouch-js).
I have further expanded this library into a distributable package, refactored for es2015 development.

This package includes the `getWebAudioNode` utility written by [Adrian Holovaty](https://github.com/adrianholovaty), as
well as the user-friendly `PitchShifter` wrapper from [Jakub Faila](https://github.com/jakubfiala/soundtouch-js).

## Installation

You can easily install **SoundTouchJS** for use in your project:

```
npm install soundtouchjs
```

## General Usage

You can use whatever method you prefer to **get** your audio file, but once you have the data you must decode it into
an [AudioBuffer](https://developer.mozilla.org/en-US/docs/Web/API/AudioBuffer). Once you've decoded the data you can
then create a new [PitchShifter](#PitchShifter).

```javascript
import {PitchShifter} from 'soundtouchjs';

const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
const gainNode = audioCtx.createGain();
let shifter;

// here you retrieved your file with 'fetch' or a new instance of the 'FileReader', and from the data...
    if (shifter) {
        shifter.off(); // remove any current listeners
    }
    audioCtx.decodeAudioData(buffer, audioBuffer => {
        shifter = new PitchShifter(audioCtx, audioBuffer, 1024);
        shifter.on('play', (detail) => {
            // do something with detail.timePlayed;
            // do something with detail.formattedTimePlayed;
            // do something with detail.percentagePlayed
        });
        shifter.tempo = 1;
        shifter.pitch = 1;
    });
```

To begin playback you connect the `PitchShifter` to the WebAudio destination (or another node), and disconnect it to
pause. It's important to note that the `PitchShifter` is a pseudo-node, and cannot be connected to.

```javascript
const play = function () {
    shifter.connect(gainNode); // connect it to a GainNode to control the volume
    gainNode.connect(audioCtx.destination); // attach the GainNode to the 'destination' to begin playback
};
```

## Running The Example

An example has been included with the package to see some basic functionality. It has been written in pure javascript,
but could easily be integrated with your favorite framework. To run the example:

```
npm run example
```

then open your browser to `http://localhost:8080`. A royalty free music file is included under Creative Commons License
with this repository.

Music: "Actionable" from [Bensound.com](http://bensound.com).

This is a limited use license, and we do not grant any permissions beyond theirs. Please refer to
[their licensing](https://www.bensound.com/licensing) for further information.

All core components of the package are available as separate entities for more advanced audio manipulations. See the
source code for greater understanding.