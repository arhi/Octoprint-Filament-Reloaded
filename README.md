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

## Plugin Configuration

After installation, configure the plugin via OctoPrint Settings interface.

## OrangePI OS Configuration

Since we are accessing the GPIO as a non root user we need to configure the OS to allow this. Here's the copy of the library documentation on how to do it:

 If you want to be able to use the library as a non root user, you will need to setup a `UDEV` rule to grant you permissions first. 
 This can be accomplished as follows: 
 ```
 $ sudo usermod -aG gpio <current_user>
 $ sudo nano /etc/udev/rules.d/99-gpio.rules
 ```
 That should add your user to the GPIO group, create a new ``UDEV`` rule, and open it in the Nano text editor. 
 Enter the following into Nano
 ```
   SUBSYSTEM=="gpio", KERNEL=="gpiochip*", ACTION=="add", PROGRAM="/bin/sh -c 'chown root:gpio /sys/class/gpio/export /sys/class/gpio/unexport ; chmod 220 /sys/class/gpio/export /sys/class/gpio/unexport'" 
   SUBSYSTEM=="gpio", KERNEL=="gpio*", ACTION=="add", PROGRAM="/bin/sh -c 'chown root:gpio /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value ; chmod 660 /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value'"
 ```   
 press ``ctrl-x``, ``Y``, and ``ENTER`` to save and close the file. 
 Finally, reboot and you should be ready to use ``OPi.GPIO`` as a non root user. 


## Issues with OPI GPIO library

https://github.com/rm-hull/OPi.GPIO/pull/28 solves the issues with PI GPIO library and the race condition that was preventing normal operations. It is available in release v0.3.4 and up so make sure you have latest OPI GPIO library. To upgrade to latest OPI GPIO library login to your octoprint server as user ``pi`` and execute:

```
 $ cd OctoPrint/ 
 $ venv/bin/pip install --upgrade OPi.GPIO
```
