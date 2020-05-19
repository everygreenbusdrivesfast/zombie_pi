# zombie_pi
Documentation for making Zombie radio warnings
# Zombie Radio Warnings #

We used the [tutorial](https://makezine.com/projects/pirate-radio-throwies/) for a Pirate Radio Throwie, as we liked the versatility of an off grid, urban situation.

## Antenna ##

We chose not to use an antenna as then there is only a 200cm radius output of signal, so we don't interfere with any radio signals. However we considered using [jumper wires](https://thepihut.com/products/jumper-wires-9-f-f-10-pack), and were satisfied that they would increase the range. Copper wire with srink tubing would be another good option.

## Download and Install Raspbian ##

Using our laptop we used the Raspberry Pi Foundation website and downloaded a copy of [Raspbian Buster Lite](https://www.raspberrypi.org/downloads/raspbian/). Then we used the [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/)(choose your operating system from the imager download list) to flash the Raspbian Buster Lite Image onto the SD card. 

We chose to add an ssh folder to the sd card after burning,and a wpa_supplicant.conf file to headlessley connect to wifi and be able to ssh in.

Our example wpa_supplicant.conf file that you can save on your computer, edit using sublime text, or alternative text editor, then put on pi before booting:

```
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=GB

network={
     ssid="write the nameofyourwifi here"
     psk="write your password here"
     key_mgmt=WPA-PSK
}
```

Please note GB may need changing, depending on where you are in the world, eg US.

## Boot the Pi and ssh in##

Once the SD card is ready, we put it in the Raspberry Pi SD card slot and connected the power cable to the micro USB port. 

We use the [Fing Ap](https://play.google.com/store/apps/details?id=com.overlook.android.fing&hl=en_GB) on our android phones, or [Angry IP](https://angryip.org/download/#linux) on our laptop to obtain the IP address of our Pi Zero. There are loads of other ways of getting the IP address, however this is what we use and can personally recommend.

Once we know the IP address we open a terminal on the laptop and type:
```
ssh-copy-id pi@192.168.1.4 
```
(Or whatever your ip is). Then we type;
```
yes
```
followed by the default password of the pi which is 
```
raspberry
```
Then we type this command: 
```
ssh pi@192.168.1.4
```
and do not need to put the password in again. Next we want to name the pi and update/upgrade....

```
sudo raspi-config 
```
Once the raspi-config command has run you will see a blue box appear on the terminal. This is the config/settings page.

You need to select Network, then select hostname by pressing enter. You will then need to use back space to delete the default raspberrypi and replace with your own choice. We have had problems with long names, so keep it fairly straight forward and consider this if you have problems mentioning hostname.

# Get the Software #

Open a terminal and type in the command below to duplicate the files in the remote code repository to your home directory on the Pi. 
```
    git clone https://github.com/rm-hull/pifm
```
Change directories to the pifm. 
```
    cd pifm
```
    Now compile the pifm application by typing the following command, then hitting return. 
```
    sudo make
```
    Next update the package repositories for Raspbian to ensure the next command downloads the latest software. Simply enter in this command: 
```
    sudo apt-get update
```
    Still in the terminal install the command line audio player mpg123 by typing: 
```
    sudo apt-get install mpg123
```

## Get the MP3 Sound Files onto your Pi ##

DOCUMENT SCP FROM PHONE OR LAPTOP TO PI AND PUT OWN WORDING IN

Download a few of your favorite mp3 files to the Pi, or you can scp them. We used Voice Recorder from the Google Ap store as we found it to be smoother than the Ubuntu Store alternatives. Make sure to put them in the /home/pi/Music directory.From the command line, type in the following command. The number 88.3 is the frequency the Pi will broadcast at. 
```
   sudo /usr/bin/mpg123 -4 -s -Z /home/pi/Music/* | sudo /home/pi/pifm/pifm - 88.3
```
To put your mp3 files in a different directory, simply change the directory path that's in the command above. 

## Is it Broadcasting your Zombie Apocalypse Warning? ##

Grab a radio and tune your dial to 88.3 to listen to the audio output of your Pi Zero.

## Make it Work as soon as the Pi Boots up! ##

To make PiFM run when the Pi boots up from the battery pack, we used the simple shell script, from the tutorial, and add it into the init system.

Using 
```
sudo nano /etc/init.d/pirateRadio.sh
```
a file called pirateRadio.sh, is created in the /etc/init.d/ directory.
Cut and paste the following code into your /etc/init.d/pirateRadio.sh file. 
```
#!/bin/bash # /etc/init.d/pirateRadio.sh case "$1" in start) echo "StARRRRRRRting Pirate Radio" sudo /usr/bin/mpg123 -4 -s -Z /home/pi/Music/* | sudo /home/pi/pifm/pifm - 88.3 ;; stop) echo "Stopping Pirate Radio" killall pifm ;; *) echo "Usage: /etc/init.d/pirateRadio.sh start|stop" exit 1 ;; esac exit 0
```
Change the permission on the pirateRadio.sh script by typing the following command: 
```
sudo chmod 755 /etc/init.d/pirateRadio.sh
```
Lastly add the pirateRadio.sh script into the startup scripts default profile. 
```
sudo update-rc.d pirateRadio.sh defaults 
```

## Make a Magnetic Case with Batteries to hide the Pi somewhere sneaky! ##

Get a USB battery pack and use hot glue to attach some strong magnets to one side of the [battery pack](https://www.ebay.co.uk/itm/USB-4-AA-Battery-Emergency-Charger-Power-Bank-Case-For-Cell-Phone/293435543643?hash=item44521f105b:g:tNcAAOSwffReJpIi), and velcro to attach the Pi to the other side of the battery pack.

After that just use a short USB Micro cable to power the Pi from the battery pack, and you’re ready to broadcast the Zombie warnings! The device made is meant for throwing at metal structures, say the bridge through town or something! Looks good!
