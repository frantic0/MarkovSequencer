## MaxiLib Documentation ##

### maxiAudio ###

This the audio context. You must always have one to produce sound with
maxiLib

#### methods ####

##### .init() #####
initialise the audio engine

##### .outputIsArray( isArray,  numChannels) #####
for multi channel  sound

- isArray = true or false
- numChannels = 2, 4, or 8

##### .loadSample(sampleUrl, maxiSample) #####
load a sample into a maxiSample object

#### properties ####

##### .play #####
the function which is the play loop

#####  .output #####
the current value of the audio output

#### example ####
```
var maxiAudio = new maximJs.maxiAudio();
var sample = new maximJs.maxiSample();

maxiAudio.init();
maxiAudio.loadSample('cheese.wav', sample);
```

***

### maxiSample ###

Stores and plays an audio sample

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     var sample1= new maximJs.maxiSample();
     var sample2= new maximJs.maxiSample();
     var clock = new maximJs.maxiClock();
     var beatCounter = 0;

     maxiAudio.init();
     maxiAudio.loadSample("bassdrum13.wav", sample1);
     maxiAudio.loadSample("hihat18.wav", sample2);
     clock.setTempo(87);
     clock.setTicksPerBeat(8);

     maxiAudio.play = function() {
         if (sample1.isReady()) {
             clock.ticker();
             var w=0;
             if (clock.isTick() ) {
                 if (beatCounter in [0,2]) {
                     sample1.trigger();
                 }
                 if (beatCounter in [1,3,4,5,7]) {
                     sample2.trigger();
                 }
                 beatCounter = (beatCounter+1) % 8;
             }
             w = sample1.playOnce();
             w += sample2.playOnce();
             this.output =  w;
         }
     };

</script>
```

#### methods ####

##### .play() #####
plays the sample at normal speed

##### .play(playRate) #####
plays the sample at the specified play rate

##### .playOnce() #####
plays the sample once at normal speed

##### .playOnce(playRate) #####
plays the sample once at specified play rate

##### .trigger() #####
set the playhead to zero (use in conjunction with playOnce)

##### .isReady() #####
returns true if sample is loaded

***

### maxiTimestretch ###

plays a sample at different rates leaving pitch unchanged

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     var sample = new maximJs.maxiSample();
     var ts = new maximJs.maxiTimestretch();
     var ts2 = new maximJs.maxiTimestretch();
     var delay = new maximJs.maxiDelayline();

     maxiAudio.init();

     // you can add files into the document by going to Settings (gear
icon) > Files
     // uncomment this and change the file name to your sample
     maxiAudio.loadSample('45379__feedback__mono.wav', sample);
     ts.setSample(sample);
     ts2.setSample(sample);

     maxiAudio.play = function() {
         if(sample.isReady()){
             var w=0;
             w = ts.play(0.5,0.1,2,0) ;
             w = w +  ts2.play(0.1,0.1,4,0.2);
             w = w  + delay.dl(w, 200, 0.98);
             this.output =  w;
         }
     };

</script>
```

#### methods ####

##### .setSample(maxiSample) #####

sets the sample play for timestretch to use

##### .play(rate, grainLength, overlaps, startPos) #####

- rate (eg. 0.5 = half speed)
- grainLength (a time in seconds)
- overlaps (normally 2 is good)
- startPos (where to start playing in the sample - in seconds)

##### .setPosition(startPos) #####

useful for resetting a sound

##### .getPosition() #####

returns position in ???

##### .getNormalisedPosition() #####

Useful for ending sample play back

<br><br>

### maxiPitchShift ###

plays a sample at different pitches leaving the speed unchanged

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     var sample = new maximJs.maxiSample();
     var ts = new maximJs.maxiPitchShift();
     var ts2 = new maximJs.maxiPitchShift();
     var delay = new maximJs.maxiDelayline();

     maxiAudio.init();

     // you can add files into the document by going to Settings (gear
icon) > Files
     // uncomment this and change the file name to your sample
     maxiAudio.loadSample('45379__feedback__mono.wav', sample);
     ts.setSample(sample);
     ts2.setSample(sample);

     maxiAudio.play = function() {
         if(sample.isReady()){
             var w=0;
             w = ts.play(0.5,0.1,2,0) ;
             w = w +  ts2.play(0.1,0.1,4,0.2);
             w = w  + delay.dl(w, 200, 0.98);
             this.output =  w;
         }
     };

</script>
```

#### methods ####

##### .setSample(maxiSample) #####

sets the sample play for pitchShift to use

##### .play(pitch, grainLength, overlaps, startPos) #####

- pitch (eg. 0.5 = half pitch)
- grainLength (a time in seconds)
- overlaps (normally 2 is good)
- startPos (where to start playing in the sample - in seconds)

##### .setPosition(startPos) #####

useful for resetting a sound

##### .getPosition() #####

returns position in ???

##### .getNormalisedPosition() #####

Useful for ending sample play back


***

### maxiPitchStretch ###

plays a sample with independent control of pitch and speed

#### methods ####

##### .setSample(maxiSample) #####

sets the sample play for timestretch to use

##### .play(pitch, rate, grainLength, overlaps, startPos) #####

- pitch (eg. 0.5 = half pitch)
- rate (eg. 0.5 = half speed)
- grainLength (a time in seconds)
- overlaps (normally 2 is good)
- startPos (where to start playing in the sample - in seconds)

##### .getPosition() #####

returns position in ???

##### .getNormalisedPosition() #####

Useful for ending sample play back

##### .setPosition(startPos) #####

useful for resetting a sound

***

### maxiDelayline ###

A simple delay line

#### methods ####

##### .dl(inputSignal, delayTime, foldback) #####

process a signal with delay

- inputSignal (any signal eg. output from an oscillator)
- delayTime (a value in milliseconds, max 2000)
- foldback (how much of the signal to feedback into the delay buffer -
determines how long echoes last), between 0 and 1

***

### maxiOsc ###

An oscillator with methods for a number of waveforms

#### methods ####

##### .sinewave(frequency) #####

outputs a sine wave at the given frequency between -1.0 & 1.0

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     maxiAudio.init();
     var myWave = new maximJs.maxiOsc();
     maxiAudio.play = function() {
         this.output = myWave.sinewave(100);
     };

</script>
```

##### .coswave(frequency) #####

outputs a cosine wave at the given frequency between -1.0 & 1.0

##### .triangle(frequency) #####

outputs a triangle wave at the given frequency between -1.0 & 1.0

##### .saw(frequency) #####

outputs a sawtooth wave at the given frequency between -1.0 & 1.0

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     maxiAudio.init();
     var myWave = new maximJs.maxiOsc();
     maxiAudio.play = function() {
         this.output = myWave.saw(100);
     };

</script>
```

##### .sawn(frequency) #####

outputs a band-limited sawtooth wave at the given frequency between -1.0
& 1.0

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     maxiAudio.init();
     var myWave = new maximJs.maxiOsc();
     maxiAudio.play = function() {
         this.output = myWave.sawn(100);
     };

</script>
```

##### .square(frequency) #####

outputs a square wave at the given frequency between -1.0 & 1.0

##### .pulse(frequency, pulsewidth) #####

outputs a square wave at the given frequency and pulsewidth between -1.0
& 1.0

- pulsewidth in the range 0 to 1

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     maxiAudio.init();
     var myWave = new maximJs.maxiOsc();
     maxiAudio.play = function() {
         this.output = myWave.pulse(100, 0.4);
     };

</script>
```


##### .phasor(frequency) #####

outputs a linear ramp at the given frequency between 0.0 & 1.0

##### .phasor(frequency, startPhase, endPhase) #####

outputs a linear ramp at the given frequency between 0.0 & 1.0

- startPhase and endPhase are the start and end values of the ramp,
between 0.0 & 1.0

```
<script>
     //alarm sound
     var maxiAudio = new maximJs.maxiAudio();
     maxiAudio.init();
     var myWave = new maximJs.maxiOsc();
     var myWave2 = new maximJs.maxiOsc();

     maxiAudio.play = function() {
         this.output = myWave.saw(200 + (myWave2.phasor(2,0.2,0.9) * 500));
     };

</script>

```

##### .noise() #####

white noise

```
<script>
     var maxiAudio = new maximJs.maxiAudio();
     maxiAudio.init();
     var myWave = new maximJs.maxiOsc();

     maxiAudio.play = function() {
         this.output = myWave.noise();
     };

</script>
```

##### .phaseReset(phase) #####

reset the phase to a specific value

- phase (a value between 0 & 1)

```
<script>
     //this will create silence by combining two out of phase sines
     var maxiAudio = new maximJs.maxiAudio();
     maxiAudio.init();
     var myWave = new maximJs.maxiOsc();
     var myWave2 = new maximJs.maxiOsc();
     myWave2.phaseReset(0.5);
     maxiAudio.play = function() {
         this.output = myWave.sinewave(100) + myWave2.sinewave(100);
     };

</script>
```


***

### maxiEnv ###

An adsr envelope.

#### methods ####

##### .setAttack(time) #####
- time = milliseconds

##### .setDecay(time) #####
- time = milliseconds

##### .setSustain(level) #####
- level = a value between 0.0 and 1.0

##### .setRelease(time) #####
- time = milliseconds

##### .adsr(level, trigger) #####
- level (the overall level of the envelope; everything will be scaled by
this value)
- trigger (envelope will begin attack when set to 1.0 and release when
set to 0.0)

***

### maxiFilter ###

A bunch of useful filter methods

#### methods ####

##### .lores(input, cutoff, resonance) #####

A lowpass resonant filter. Returns the filtered frequency.

- input =  input signal
- cutoff = cutoff frequency in Hz
- resonance = a value between 0.0 & 10.0

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     var myWave = new maximJs.maxiOsc();
     var myLFO = new maximJs.maxiOsc();
     var myFilter = new maximJs.maxiFilter();

     maxiAudio.init();


     maxiAudio.play = function() {
         var w=0;
         w = myWave.saw(100);
         w = myFilter.lores(w, 1000 + (myLFO.sinewave(1) * 800), 3);
         this.output =  w;
     };

</script>
```

##### .hires(input, cutoff, resonance) #####

A highpass resonant filter. Returns the filtered frequency.

- input =  input signal
- cutoff = cutoff frequency in Hz
- resonance = a value between 0.0 & 10.0

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     var myWave = new maximJs.maxiOsc();
     var myLFO = new maximJs.maxiOsc();
     var myFilter = new maximJs.maxiFilter();

     maxiAudio.init();


     maxiAudio.play = function() {
         var w=0;
         w = myWave.saw(100);
         w = myFilter.hires(w, 1000 + (myLFO.sinewave(1) * 800), 3);
         this.output =  w;
     };

</script>
```

##### .lopass(input, cutoff) #####

A one-pole low-pass filter, cutoff between 0 and 1

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     var myWave = new maximJs.maxiOsc();
     var myLFO = new maximJs.maxiOsc();
     var myFilter = new maximJs.maxiFilter();

     maxiAudio.init();


     maxiAudio.play = function() {
         var w=0;
         w = myWave.saw(100);
         w = myFilter.lopass(w, 0.25 + (myLFO.sinewave(1) * 0.25));
         this.output =  w;
     };

</script>
```

##### .hipass(input, cutoff) #####

A one-pole high-pass filter, cutoff between 0 and 1

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     var myWave = new maximJs.maxiOsc();
     var myLFO = new maximJs.maxiOsc();
     var myFilter = new maximJs.maxiFilter();

     maxiAudio.init();


     maxiAudio.play = function() {
         var w=0;
         w = myWave.saw(100);
         w = myFilter.hipass(w, 0.5 + (myLFO.sinewave(1) * 0.25));
         this.output =  w;
     };

</script>
```
***

### maxiSVF ###

A state variable filter, see
http://www.cytomic.com/files/dsp/SvfLinearTrapOptimised.pdf

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     var myWave = new maximJs.maxiOsc();
     var myLFO = new maximJs.maxiOsc();
     var myLFO2 = new maximJs.maxiOsc();
     var mySVF = new maximJs.maxiSVF();

     maxiAudio.init();

     mySVF.setCutoff(200);
     mySVF.setResonance(4);

     maxiAudio.play = function() {
         var w=0;
         mySVF.setCutoff(2000 + (myLFO.sinewave(1) * 1800));
         mySVF.setResonance(7 + (myLFO.coswave(0.1) * 6));
         w = myWave.saw(100);
         w = mySVF.play(w, 1.0, 0.0, 0.0, 0.0);
         this.output =  w;
     };

</script>
```

#### methods ####

##### .play(input, lowPassMix, highPassMix, bandPassMix, notchMix) #####

- input: a signal
- lowPassMix:  the amount of low pass filtering, between 0 and 1
- highPassMix:  the amount of high pass filtering, between 0 and 1
- bandPassMix:  the amount of band pass filtering, between 0 and 1
- notchMix:  the amount of notch filtering, between 0 and 1

##### .setCutoff(frequency) #####

frequency between 20 and 20000, although this filter sounds best below 5000

##### .setResonance(amount) #####

from 0 upwards, starts to ring from 2-3ish, cracks a bit around 10

***

### maxiFlanger ###

#### methods ####

##### .flange(input, delay, feedback, speed, depth)

- input: an input signal
- delay: delay time, in ms, max 2000
- feedback: between 0 and 1
- speed: lfo speed in Hz
- depth: between 0 and 1

```
<script>

     var maxiAudio = new maximJs.maxiAudio();
     var osc = new maximJs.maxiOsc();
     var flanger = new maximJs.maxiFlanger();
     maxiAudio.init();

     maxiAudio.play = function() {
         var w = 0;
         w = osc.pulse(40,0.1);
         w = flanger.flange(w, 100, 0.8, 0.2, 0.8);
         this.output = w;
     };

</script>
```
***

### maxiFFT ###

#### methods ####

##### .setup(fftSize, windowSize, hopSize) #####

must be called before using the FFT

- fftSize = (A power of two, 1024, 512 .. etc)
- windowSize = half the fftSize
- hopSize = half the windowSize

##### .process(sig) #####

returns true if successful

- sig = signal in

##### .getMagnitude(index) #####

get the magnitude of a particular bin

- index = A number between 0 and the fftSize/2

##### .getMagnitudeDB(index) #####

get the decibels of a particular bin

##### .magsToDb() #####

perform the conversion on all bins



***

### convert ###

A collection of conversion functions. Currently numbering one !

#### methods ####

##### .mtof(midi) #####

pass a midi value and its frequency is returned

***

### maxiMix ###

A multichannel mixer.

#### methods ####

##### .stereo(sig, outputArray, pan) #####

Makes a stereo mix.

- sig = inputsignal
- outputArray = VectorDbl array (see maxiTools)
- pan = a value between 0 & 1

***

### maxiTools ###

#### methods ####

##### .getArrayAsVectorDbl(inputArray) #####

Returns the array as a VectorDbl object.  (Needed for maxiMix).

### Undocumented classes ###

- maxiArray
- maxiChorus
- maxiClock
- maxiDCBlocker
- maxiDistortion
- maxiDyn
- maxiEnvelope
- maxiEnvelopeFollower (undefined)

- maxiFFTOctaveAnalyzer
- maxiHats
- maxiIFFT
- maxiKick
- maxiLagExp
- maxiMFCC
- maxiMap

- maxiSettings
- maxiSnare
- maxiTools