Prompt: Make a spectrogram of an mp3 file using webaudio in javascript

```js
const audioCtx = new AudioContext();
const analyser = audioCtx.createAnalyser();
analyser.fftSize = 2048;
const bufferLength = analyser.frequencyBinCount;
const dataArray = new Uint8Array(bufferLength);
console.log("dataArray:", dataArray)

// Load the mp3 file
const audioElement = new Audio();
audioElement.src = 'path/to/mp3/file.mp3';

// Connect the analyser to the audio source
const source = audioCtx.createMediaElementSource(audioElement);
source.connect(analyser);

// Retrieve the data and copy it into our array
// analyser.getByteTimeDomainData(dataArray);
analyser.getFloatFrequencyData(dataArray);

// Plot the data onto an HTML <canvas>
const canvas = document.querySelector('.visualizer');
const canvasCtx = canvas.getContext('2d');
canvasCtx.clearRect(0, 0, canvas.width, canvas.height);
canvasCtx.fillStyle = 'rgb(200, 200, 200)';
canvasCtx.fillRect(0, 0, canvas.width, canvas.height);
canvasCtx.lineWidth = 2;
canvasCtx.strokeStyle = 'rgb(0, 0, 0)';
canvasCtx.beginPath();

const sliceWidth = canvas.width * 1.0 / bufferLength;
let x = 0;

for (let i = 0; i < bufferLength; i++) {
  const v = dataArray[i] / 128.0;
  const y = v * canvas.height / 2;

  if (i === 0) {
    canvasCtx.moveTo(x, y);
  } else {
    canvasCtx.lineTo(x, y);
  }

  x += sliceWidth;
}

canvasCtx.lineTo(canvas.width, canvas.height / 2);
canvasCtx.stroke();
```