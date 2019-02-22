# control_RCR-DP3-3711-A-IP20-FR-3726
Control your SilverCrest RCR DP3 3711-A IP20 FR 3726 RC sockets with your Raspberry Pi and 433 MHz transmitter.

This is pretty much untouched version of "433Utils" by Martin-Laclaustra.
In the RPi_utils folder are pre-compiled applications "send", "codesend" and "RFSniffer", as well as simple bash script "socket", wich is pretty much only thing written by me. :D

# Dependencies and installation

You will need git and wiringpi installed (both should be present in the latest release of raspbian), I think that is the only dependencies. As hardware, you will need Raspberry Pi of course, and 433 MHz transmitter (ideally you should have both transmitter and receiver, but it is possible to make it work without the receiver).

You can "install" this onto your Pi with just cloning this repository, and adding executable property to the files.

Here are the commands for the "lazy quick install":

```
cd ~
sudo apt update
sudo apt install -y git wiringpi
git clone https://github.com/Msprg/control_RCR-DP3-3711-A-IP20-FR-3726.git
cd control_RCR-DP3-3711-A-IP20-FR-3726/RPi_utils/
make clean
make
chmod +x RFSniffer send codesend socket
```

# Usage
*This repository is intended to be used with sockets SilverCrest RCR DP3 3711-A IP20 FR 3726 especially the ```socket``` bash script as it was tested, howewer I can not guarantee you, that it will work with your Pi, transmiter/receiever and RC sockets.*

## Hardware

As mentioned above, you will need at very least 433 MHz transmitter, but having a reciever too is very useful. Now we will need to connect the transmitter/receiever to the gpio pins of the RPi. I recommend you, especially, if you are beginner, to disconnect the Pi from the power source before connecting anything to the GPIO pins.

1. Open RPi GPIO schematic: https://pinout.xyz/pinout/wiringpi
2. Connect your 433 MHz transmitter GND (negative, -) pin to any Ground pin on the Pi header. (F.Ex. Pin No. 9)
3. Connect your 433 MHz transmitter VCC (positive, +) pin to the only one 3,3V pin (pin No. 1)
4. Connect your 433 MHz transmitter DATA              pin to pin No. 11 (WiringPi 0)
*Skip rest if does not have receiever*
5. Connect your 433 MHz receiever GND (negative, -) pin to any Ground pin on the Pi header. (F.Ex. Pin No. 14)
6. Connect your 433 MHz receiever VCC (positive, +) pin to the 5V pin (pin No. 2 or 4)
7. Connect your 433 MHz receiever DATA              pin to pin No. 13 (WiringPi 2)
8. Your hardware now should be set up!


## Software
After installation, in the RPi_utils folder is the ```socket``` script, wich have some "demo" codes already hardcoded, but these you can easily change by editing the script.


# Gaining control of your RC sockets

## 0. Capturing the codes of your remote
*You can (have to) skip thie step if you does not have the 433 MHz receiever or have destroyed/lost/not have a remote of your sockets.
1. Open some text editor or just find the way to tempotairly save the captured codes.
2. cd into the RPi_utils folder and run ```./RFSniffer``` command. (You might want to use the ```./RFSniffer -v``` command to see more info like pulselenght or protocol).
3. Start pressing the buttons on your remote. You would get 2-4 lines of the same recieved codes repeated. That is fine. (If you got different result, you might did something wrong. Check your steps!).
4. After each button press you should copy receieved code to your text editor, along with some decription (Wich button you pressed F. Ex.) to know wich code does what.
5. After capturing your code, you can exit the "recieving mode" by pressing the Control-C combination on the keyboard.

## 1. Test sending the code
*If you have captured your own codes, use them here, if not, you can skip this step too*

1. Run command ```./codesend CODE PROTOCOL PULSELENGHT``` for example: ```./codesend 4182153 2 125```
2. If you got no reaction, check previous steps, wether you did everything correctly. If you have a receiever, you can check that your transmitter actually transmit by opening the two terminal windows, in one running ```./RFSniffer``` command, and from the other running the ```./codesend CODE PROTOCOL PULSELENGHT``` command.

## 2. Make sending the codes a bit easier

The ```socket``` script is there for exactly this prupose. If your codes work as expected by running ```./codesend CODE PROTOCOL PULSELENGHT```, you shall edit the lines in the ```socket``` script accordingly.
Then you just run ```socket``` with syntax ```socket (A|B|C|D|ALL) (1|0)``` For example commmand ```./socket C 1``` Should turn on socket C.
The all parameter turns on or off all sockets at once by sending the "master code". Of course, you can rewrite the ALL function to just call the all other functions if you does not have the "master code" or because of something else.

##

# Here begin the official content of README.md from the https://github.com/Martin-Laclaustra/433Utils repository as from time I cloned it. 

##

# About 433Utils

433Kit is a collection of code and documentation designed to assist you in the connection and usage of RF 433MHz transmit and receive modules to/with your Arduino and Rapberry Pi.

It consists of two main sections- Arduino sketches and Raspberry Pi command line utilities.  You'll find those in appropriately named folders.

## Requirements

1. An Arduino (two is ideal)
2. A Raspberry Pi
3. The git commandline tool, on both your Arduino IDE machine and your RPi
4. RF 433MHz modules; a transmitter and receiver
5. A Ninja Block and account

## Installation

### Arduino\_Sketches

Place the Sketches in your normal Arduino Sketches location.  Install any required libraries:

1. Servo motor library (Should be included in the Arduino IDE)
2. _Arduino_ [rc_switch](http://code.google.com/p/rc-switch/) library

### RPi\_utils

From your RPi, clone this archive:

1. ```git clone --recursive git://github.com/ninjablocks/433Utils.git``` (recursive ensure that the rc-switch submodule gets populated which is needed by RPi\_utils)

2. ```cd 433Utils/RPi_utils```

3. (As per the original rc_switch distribution) Follow the instructions to install the  [wiringpi](https://projects.drogon.net/raspberry-pi/wiringpi/download-and-install/) library.
After that you can compile the example programs by executing *make*. 

## Usage

### send 
```sudo ./send systemCode unitCode command```

This command is unaltered from the original rc\_switch distribution.


### codesend 
```sudo ./codesend decimalcode```

This command uses a single Ninja Blocks compatible decimal code.  Sniff out codes using the RF\_Sniffer.ino Arduino sketch, included in this distribution.

You can use codesend to control the wireless servo on your Arduino.
(The sketch is Servo\_Receive\_Demo.ino)

I've implemented the most bare-bones of controls here.  Try:

```sudo ./codesend 500000 # to move the servo up```

```sudo ./codesend 500001 # to move the servo down```

```sudo ./codesend 500002 # to move the servo left```

```sudo ./codesend 500003 # to move the servo right```

```sudo ./codesend 500004 # to move the servo home```

## Issues

Due to limitiations in the implementation of interrupt-driven routines in the underlying RCSwitch library, it is currently not possible to use both the send and receive functionality within the one program.  
