### What is it

https://github.com/pure-data/pure-data


This intro starts from building a simple metronome in Pd goes upto Sound Synthesis applications

https://sites.google.com/site/introtopd

Pure Data is a visual dataflow programming language. Dataflow languages model a program as a directed graph of the data flowing between operations

This works like a modular synth by creating a graphical environment that models the flow of the control and audio. Along with basic  mathematical, logical, and bitwise operators PD also support Digital Signal Processing functions like  wavetable oscillators, Fast Fourier transforms (fft~) and a bunch of filters while also accepting standard audio input from mic, MIDI via OSC or generated on the fly, and stored in tables, which can then be read back and used as audio signals or control data.

### 4 Basic Entities

Pd supports four basic types of entities:

- **objects**

Different functionalities of PD like print & oscillator. There are more than 100 different objects in Pd. Each object functions differently

There are two types of objects
	
	- Tilde (~) objects deal with audio signal
	- Control objects deal the rest (e.g.  MIDI data)

- **atoms**

Atoms are the most basic unit of data in Pd, and they consist of either a float, a symbol, or a pointer to a data structure (in Pd, all numbers are stored as 32-bit floats)

- **messages**

Messages are composed of one or more atoms and provide instructions to objects, there is a special type of message with null content called a bang is used to initiate events and push data into flow, much like pushing a button.


- **comments**

A bit of text that can be used to add context to a PD program


