
midi@3:14 is a home-made electronic keyboard for playing music.

It has been designed with the following requirements:

* Small size, light weight.
* Uniform layout.
* Compatible with any computer hardware (PC, Raspberry Pi, &hellip;) and free software synthesizers.

As a result, midi@3:14 has the following characteristics:

* [Jankó](https://en.wikipedia.org/wiki/Jank%C3%B3_keyboard) layout with 3 rows of 14 keys.
* 5 potentiometers that can be used to change the volume, or any other control value supported by the MIDI standard.
* A USB-MIDI interface.

![midi@3:14 keyboard assembly](images/final-assembly.jpg)

This repository contains two Arduino sketches for the Pro Micro module:

* `midi314-keyboard`: the firmware of the MIDI keyboard.
* `midi314-joystick`: the firmware of an additional MIDI controller composed of a joystick and a pressure sensor to be used as a breath interface.

Build and upload instructions
=============================

Using the Arduino CLI:

```
arduino-cli core install arduino:avr
arduino-cli compile --fqbn arduino:avr:leonardo --libraries libraries midi314-keyboard
arduino-cli upload --fqbn arduino:avr:leonardo --port /dev/ttyACM0 midi314-keyboard
```

Monitoring the MIDI port
========================

If your Linux distribution supports Pipewire, you can use `pw-link` to manage MIDI connections,
and `pw-mididump` to print the MIDI messages from the keyboard.

In a terminal, run:

```
pw-mididump
```

In another terminal, list the available MIDI output ports and locate the port name assigned to the Arduino module:

```
pw-link -o | grep Arduino
```

This command should print a port name like this: `Midi-Bridge:Arduino Leonardo:(capture_0) Arduino Leonardo MIDI 1`

List the available MIDI input ports and locate the port of `pw-mididump`.

```
pw-link -i | grep dump
```

You should get: `midi-dump:input`.

Connect the keyboard's MIDI port to `pw-mididump`:

```
pw-link "Midi-Bridge:Arduino Leonardo:(capture_0) Arduino Leonardo MIDI 1" "midi-dump:input"
```

Press the keys of the keyboard and turn the potentiometers.
Check that `pw-mididump` prints the MIDI messages from the keyboard.

Playing sound
=============

We will run the synthesizer [FluidSynth](https://www.fluidsynth.org/) from the command line
with a custom MIDI port name like this:

```
fluidsynth -p FluidSynth
```

You can check the MIDI port name associated with FluidSynth by executing this command:

```
pw-link -i | grep FluidSynth
```

It should print `Midi-Bridge:FluidSynth:(playback_0) FluidSynth`.

Connect the keyboard's MIDI port to FluidSynth:

```
pw-link "Midi-Bridge:Arduino Leonardo:(capture_0) Arduino Leonardo MIDI 1" "Midi-Bridge:FluidSynth:(playback_0) FluidSynth"
```

... and play some notes.
