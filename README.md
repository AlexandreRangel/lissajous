### LISSAJOUS

Lissajous is a tool for real time audio performance using Javascript. It wraps succinct tools for creating oscillators and samplers into a chainable API, allowing performers to build and improvise songs with a minimum of code.

Play with Lissajous. Requires the latest stable Chrome build & a relatively recent OS.

```javascript
// make a triangle wave that loops the notes 69, 67, then 60 in quarter note intervals
t = new track()
t.tri().beat(4).notes(69,67,60)

// load a sample, set the beat to quarter notes, set the note length to a half measure,
// set the envelope to give it a little attack and release, and loop the notes 69, 67, then 60
s = new track()
s.sample(buffer);
s.beat(4).nl(8).adsr(0.1,0,1,1).notes(69,67,60)

// load an array of AudioBuffers called 'drums', play them in 8th notes and give them
// the sequence drums[0], drums[2], drums[1], drums[2]
d = new track()
d.sample(drums);
d.beat(2).sseq(0,2,1,2)
```

#### Basic Concepts of Lissajous

A performance in the Chrome console using Lissajous takes place in the global namespace. You'll probably create a lot of variables with names like `s` or `a` or `b1` or `b2` or `doodle`, and this sort of usage is encouraged. Need to start over? Refresh the page.

The key to making the most of Lissajous is to add scripts (e.g. `extras.js`) that load all of your samples into the environment on page load. A clever or ambitious performer might write some additional functions to coordinate changes, control transitions, or execute other group-oriented operations with style and grace. Sky's the limit.

#### Every track has a step sequencer.

Tracks make sound when they are given a beat. Here's the minimum needed to generate some sound:

```javascript
t = new track()
t.beat(4)
```

Beat is a step sequencer. Calling `t.beat(4)` says "play a note every four 16th notes." The Lissajous API supports an arbitrary number of arguments, allowing us to make more complicated patterns:

```javascript
t.beat(4,3,2,1,4,2)
```

If we visualized this in a classic step sequencer view, it would look like this:

```
 1           5           9           13
[x][ ][ ][ ][x][ ][ ][x][ ][x][x][ ][ ][ ][x][ ]
```

The clock ticks in 32nd notes, and the API is written to support both 16th and 32nd note expressions.

```
t.beat(1) // play a note every 16th note
t.beat32(1) // play a note every 32nd note
```

#### Everything reacts to the step sequencer.

Most of the parameters of a track can be given more than one value (we call this a pattern). For example, with notes:

```javascript
t = new track()
t.beat(4).notes(69, 67, 60)
```

Tracks are monophonic. Since we supplied three notes, they will play one at a time, looping back to the beginning when they reach the end. There's no limit to the number of notes you can have in a pattern.

Patterns for each parameter are self-contained and do not rely on each other, allowing us to play with patterns of different lengths:

```javascript
t = new track()
t.beat(4).nl(4,2).notes(69, 67, 60)
```

Here we toggle between a note length of 4 and 2 every time a note is hit, but we cycle through three different notes.


#### Samples