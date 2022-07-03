
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
