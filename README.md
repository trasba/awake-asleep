# awake-asleep

This project is for a daytime sleeptime display for little kids.
The idea is that they learn when it is time to sleep on or if it is already time to get up.
It aims at kids to young to read the clock.

## realization

I implemented this by using a raspberry pi with a small lcd display.
The display is not dimable so, I intend to dim it manually by applying a tint film.

## prerequisites

* raspberry pi + display
*assumption the  display is setup and working*
* wifi connectivity (for time syncronization)
*included in raspberry pi 3 and higher
or by USB Wifi*

## installation

>I will asume that this is a blank installation of rasbian / raspberry pi os

 * install a webserver (e.g. nginx or apache) + unclutter we'll use nginx in this example
`sudo apt update && sudo apt upgrade -y && sudo apt install nginx unclutter`
 * clone the repository
`git clone https://github.com/trasba/awake-asleep.git`
 * symlink the folder so that it is hosted
`ln -s awake-asleep /var/www/aa` 
 * copy config.js from config.js.example
 `cp awake-asleep/config.js.example awake-asleep/config.js `

-> now you should be able to view the website by browsing to localhost
`chromium-browser localhost/aa`

 * all left to do is to configure the raspberry to boot into a fullscreen state
 create a file /home/pi/.config/lxsession/LXDE-pi/autostart with the following content
```
#hide mouse pointer
@unclutter
#deactivate screen safer
#@xscreensaver -no-splash
@xset s off
@xset -dpms
@xset s noblank
@point-rpi
# load chromium in fullscree after reboot
@chromium-browser --kiosk http://localhost/aa
```
## configuration
the config.js file contains the whole configuration

```
export  const  srcPath = {
	update_cycle:  5000, /* set a reasonable number e.g. 5 min */
	asleep:  "./dummy-asleep.png",
	awake:  "./dummy-awake.png",
	// do not use 00 or any leading 0 this will result in the error
	// "Uncaught SyntaxError: Octal literals are not allowed in strict mode."
	waketime_hour:  7,
	waketime_minute:  30,
	bedtime_hour:  12,
	bedtime_minute:  30
};
```

| parameter      | description |
| :----------- | :----------- |
| update_cycle	|	cycle time in milli seconds in which the script is executed  |
| asleep| relative path to the asleep image that is shown during bedtime |
| awake| relative path to the awake image that is shown during waketime |
| waketime_hour| waktime hour (don't use leading 0) |
| waketime_minute| waketime minute (don't use leading 0) |
| bedtime_hour| bedtime hour (don't use leading 0) |
| bedtime_minute|bedtime minute (don't use leading 0) |

***that's it have fun*** :tada: