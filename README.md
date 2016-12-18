# astroberry-piface-cad
Astroberry PiFace CAD provides INDI driver for PiFace Control and Display addon board to Raspberry Pi.

The project provides INDI drivers using PiFace Control and Display (2x16 LCD display + 5 buttons + nav button)

The project is the result of splitting Astroberry PiFace at its version 2.0.2

# How to start?
First, you need to download and install INDI server and libraries. See [INDI site](http://indilib.org/download.html) for more details.
In most cases it's enough to run:
```
sudo apt-add-repository ppa:mutlaqja/ppa
sudo apt-get update
sudo apt-get install libindi1
```
Second, download and install astroberry-pifacecad.

Compiling from source:
```
sudo apt-get install libindi-dev
git clone https://github.com/rkaczorek/astroberry-piface-cad.git
cd astroberry-pifacecad-cad
mkdir build
cd build
cmake ..
make
make install
```

Installing from binaries:
```
wget https://github.com/rkaczorek/astroberry-piface-cad/binaries/astroberry-piface-cad_2.0.1-1_armhf.deb
dpkg -i astroberry-piface-cad_2.0.1-1_armhf.deb
```

#How to use it?
Start your INDI server with Astroberry PiFace CAD driver:

`indiserver -l /var/log/indi -f /var/run/indi -p 7624 indi_piface_cad`

Start KStars with Ekos, connect to your INDI server and enjoy!
