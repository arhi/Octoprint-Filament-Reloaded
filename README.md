# OctoPrint-FilamentReloaded (OrangePi)

[OctoPrint](http://octoprint.org/) plugin that integrates with a filament sensor hooked up to a Orange Pi GPIO pin and allows the filament spool to be changed during a print if the filament runs out.

Forked from [Octoprint-Filament-Reloaded](https://github.com/kontakt/Octoprint-Filament-Reloaded) plugin by kontakt based on the [Octoprint-Filament](https://github.com/MoonshineSG/Octoprint-Filament) plugin by MoonshineSG.

## Required sensor

Using this plugin requires a filament sensor. OrangePi OPi.GPIO library do not support pullup/pulldown configuration yet, nor it does support debouncing so a proper sensor must be created. One side of the switch should go to ground, other side of the switch should go to GPIO pin one want to use. 10k resistor needs to be connected between 3V and the GPIO pin in use and 0.1uF capacitor needs to go between the GPIO pin in question and ground (positive side of the capacitor on GPIO pin). 


## TODO

* Add sowftware debounce of noisy sensors.
* rename?
* Add a way to switch between OPi and RPi?

## Installation

* Install via the bundled [Plugin Manager](https://github.com/foosel/OctoPrint/wiki/Plugin:-Plugin-Manager).
* Manually using this URL: https://github.com/arhi/Octoprint-Filament-Reloaded-OrangePi/archive/master.zip

## Configuration

After installation, configure the plugin via OctoPrint Settings interface.

## Issues with OPI GPIO library

Note that OPI GPIO library has a race condition that does not allow it to properly set privs to the GPIO so it's a huge problem. The "hack" to this problem is to edit  /home/pi/OctoPrint/venv/lib/python2.7/site-packages/OPi/GPIO.py and add a delay as explained here: https://discourse.octoprint.org/t/solved-octoprint-filament-reloaded-orangepi/3576/3?u=arhi
