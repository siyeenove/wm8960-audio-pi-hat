# wm8960 audio pi hat

The drivers of [wm8960 audio pi hat] for Raspberry Pi.

http://siyeenove.com

### Install wm8960-soundcard
Get the wm8960 soundcard source code. and install all linux kernel drivers

```bash
pi@raspberrypi:~ $ git clone https://github.com/siyeenove/wm8960-audio-pi-hat
pi@raspberrypi:~ $ cd wm8960-audio-pi-hat
pi@raspberrypi:~/wm8960-audio-pi-hat $ sudo ./install.sh 
pi@raspberrypi:~/wm8960-audio-pi-hat $ sudo reboot
```

While the upstream wm8960 codec is not currently supported by current Pi kernel builds, upstream wm8960 has some bugs, we had fixed it. we must it build manually.

Check that the sound card name matches the source code wm8960-soundcard.

```bash
pi@raspberrypi:~ $ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
  Subdevices: 7/7
  Subdevice #0: subdevice #0
  Subdevice #1: subdevice #1
  Subdevice #2: subdevice #2
  Subdevice #3: subdevice #3
  Subdevice #4: subdevice #4
  Subdevice #5: subdevice #5
  Subdevice #6: subdevice #6
card 0: ALSA [bcm2835 ALSA], device 1: bcm2835 ALSA [bcm2835 IEC958/HDMI]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: wm8960soundcard [wm8960-soundcard], device 0: bcm2835-i2s-wm8960-hifi wm8960-hifi-0 []
  Subdevices: 1/1
  Subdevice #0: subdevice #0
pi@raspberrypi:~ $ arecord -l
**** List of CAPTURE Hardware Devices ****
card 1: wm8960soundcard [wm8960-soundcard], device 0: bcm2835-i2s-wm8960-hifi wm8960-hifi-0 []
  Subdevices: 1/1
  Subdevice #0: subdevice #0

```
If you want to change the alsa settings, You can use `sudo alsactl --file=/etc/wm8960-soundcard/wm8960_asound.state  store` to save it.


### Usage:
```bash
#It will capture sound an playback on hw:1
pi@raspberrypi:~ $ arecord -f cd -Dhw:1 | aplay -Dhw:1
```
[Note:] "-Dhw:1" is the recording(or playback device number) , depending on your system this number may differ (for example on Raspberry Pi 0 it will be 0, since it doesn't have audio jack) We can find it via "arecord -l" and "aplay -l".


```bash
#capture sound 
#arecord -d 10 -r 16000 -c 1 -t wav -f S16_LE test.wav
pi@raspberrypi:~ $ arecord -D hw:1,0 -f S32_LE -r 16000 -c 2 test.wav
```

```bash
#play sound file test.wav
pi@raspberrypi:~ $ aplay -D hw:1,0 test.wav
```

```bash
#configure sound settings and adjust the volume
pi@raspberrypi:~ $ alsamixer
```
Please use the [F6] to select [wm8960-soundcard] device first. 
The Left and right arrow keys are used to select the channel or device and the Up and Down Arrows control the volume for the currently selected device. Quit the program with ALT+Q, or by hitting the Esc key. More information.  
alsamixer is a graphical mixer program for the Advanced Linux Sound Architecture (ALSA) that is used to configure sound settings and adjust the volume.   


### uninstall wm8960-soundcard
If you want to upgrade the driver , you need uninstall the driver first.

```bash
pi@raspberrypi:~/wm8960-audio-pi-hat $ sudo ./uninstall.sh 
...

------------------------------------------------------
Please reboot your raspberry pi to apply all settings
Thank you!
------------------------------------------------------
```

Enjoy !
