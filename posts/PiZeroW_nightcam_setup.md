--- 
title: 'Nocturnal wildlife camera with Raspberry Pi'
date: '2023-09-16' 
---

We use 'cron' and 'raspivid' to schedule night time recordings each day, saving video recordings to our Pi Zero W and then we download the videos from our Pi Zero W to our computer hard drive via WiFi. Optional video processing can be done with 'ffmpeg'.

Items needed
- Raspberry Pi Zero W 
- ZeroCam NOIR 
- Powerbank 
- Mini USB to USB cable (to connect Pi to Powerbank)
- MicroSD card 

Setup Raspberry Pi OS (Raspbian) on Pi Zero W in headless mode, ensuring SSH, WiFi and user by using the advanced settings of the official Raspberry Pi Imager. [Offical Start Guide](https://www.raspberrypi.com/documentation/computers/getting-started.html#setting-up-your-raspberry-pi)

## Guide

Find out the internal IP address of your Pi Zero W and connect to it via SSH.

```bash
ssh -i privateKey user@host 
``` 

Then open your crontab to setup a recording schedule.

```bash
# Open crontab for editing
crontab -e 

# Add the following line to the bottom of your crontab
30 23 * * * raspivid -w 640 -h 480 -t 25200000 -f 25 -o ~/Pictures/"$(date +%Y%m%dT%H%M%S)".h264 
```

This will setup a schedule for seven hours of recording from 23.30 to 06.30 everyday. Video will be 640x480 pixels at the default 1080p quality and 25fps. 
The created file will be a raw bistream whose filename reflects the date and time at which it was created.

NB. 
Seven hours of footage will take up around 1.2GB of storage with this setup. Our Pi Zero W can run for around 12 hours on a 5V 2A power supply from a 250,000mAh powerbank. Your mileage may vary and if you are using your Raspberry Pi indoors, you may want to get one of the official Raspberry Pi power supplies like [this one](https://thepihut.com/products/raspberry-pi-zero-uk-power-supply) for a more reliable power supply.

Once we have our recordings, we can download them from Raspberry Pi to our personal computer. You can also save files to an external drive. 
After download, the .h264 files on Raspberry Pi can be safely deleted.

This can be done through the terminal by using 'rsync' or 'scp'. 
Download speeds through 'rsync' are about 1Mps so this should take 20 minutes.

We can then open the files in a video player like VLC media player.


FAQ 

### Activate Camera module. 

You will need to activate the 'Legacy Camera' option if you are using the ZeroCam.

```bash 
sudo raspi-config 

# Use arrow keys to navigate.
# Select 'Interface Options' > 'Legacy Camera' > 'Yes' 
# Then let Raspberry Pi reboot to apply the setting.
```
 
### Disable ZeroCam LED 

By default, the ZeroCam camera's red LED light will light up while the camera is in use. You may want to disable this behaviour for a more discreet camera setup that avoids disturbing noctural widlife.

To do this, add the line 'disable_camera_led=1' to your config.txt file under the /boot directory. 
Then reboot your Raspberry Pi. 

```bash 
sudo echo "disable_camera_led=1" >> /boot/config.txt
```

### Video processing 

We can use 'ffmpeg' to speed up the video and to convert it to a different file format.

For example, here is a way of converting from .h264 to .mp4.

```bash 
# Converts from .h264 to .mp4 keeping the original 25fps
ffmpeg -fflags +genpts -r 25 -i raw.h264 -c:v copy video.mp4

# +genpts adds presentation timestamps (.h264 files do not come with timestamps)
# -r sets the desired playback frames per second
# -i provides the path to the input file
```

### How to speed up video 

It is likely that nothing will be happening for a large part of the night and so by creating a shorter recording, we can quickly check if any significant widlife activity occurred during the night and decide on that basis whether it is worthwhile looking into the lengthier original recording.

Your media player will most likely have the option to speed up video playback. But if you want more control over this, you can use 'ffmpeg' for general purpose video processing.

Suppose we have a 7 hour video at 25fps that we want to condense into a 7 minute clip. 

We can increase the number of frames per second to achieve this. Because a video is a sequence of images, if we go through more images per fixed time interval, we are getting through the video at a faster rate, effectively shortening the video length by packing more content per second. 

With our example, we need 60x more frames per second to produce a 7 minute video clip. And so we increase the fps from 25 to 1500 when processing the video using 'ffmpeg'. 

```bash 
# Converts to .mp4 but speeds up the 7 hour video to a 7 minute video.
ffmpeg -fflags +genpts -r 1500 -i raw.h264 -c:v copy video.mp4
```

More generally, you can follow the formula described below to achieve your desired video speed.

actual duration in minutes / desired duration in minutes = fps multiplier. 

We can then open the mp4 file in a video player like Windows Media Player.


### Further reading 
[raspivid](https://www.raspberrypi.com/documentation/computers/camera_software.html#raspivid)

[speed up video with ffmpeg](http://trac.ffmpeg.org/wiki/How%20to%20speed%20up%20/%20slow%20down%20a%20video)
