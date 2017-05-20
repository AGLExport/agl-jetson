NVIDIA Jetson TK1/TX1 AGL Distro Scripts
====================================================================
These scripts aim at building an AGL distro on Jetson TK1.

AGL Home:  
https://www.automotivelinux.org/

AGL Wiki:  
https://wiki.automotivelinux.org/


Host System Preparation:
====================================================================
You need a install various packages.

exsample: Ubuntu14.04  
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \  
build-essential chrpath socat libsdl1.2-dev xterm make xsltproc docbook-utils \  
fop dblatex xmlto autoconf automake libtool libglib2.0-dev


Target System Preparation:
====================================================================
You need a setup to boot from SD card at Jetson TK1.

Please proceed to the official guide of section 3.
http://developer.download.nvidia.com/embedded/L4T/r21_Release_v4.0/l4t_quick_start_guide.txt

At that time, you will need to replace command "sudo ./flash.sh ${BOARD} mmcblk0p1" to "sudo ./flash.sh 1jetson-tk1 mmcblk1p1".


Build AGL Distro (chinook)
====================================================================
cd /path/to/work

git clone https://github.com/watatuki/agl-jetson.git -b chinook

git clone https://github.com/watatuki/meta-jetson.git -b chinook

repo init -b chinook -u https://gerrit.automotivelinux.org/gerrit/AGL/AGL-repo

repo sync

cp -R agl-jetson/meta-agl/templates/machine/jetson-tk1 meta-agl/templates/machine/

source meta-agl/scripts/aglsetup.sh -m jetson-tk1 agl-demo agl-appfw-smack

bitbake agl-demo-platform


Build AGL Distro (pre DD)
====================================================================
cd /path/to/work

git clone https://github.com/watatuki/agl-jetson.git

git clone https://github.com/watatuki/meta-jetson.git

repo init -u https://gerrit.automotivelinux.org/gerrit/AGL/AGL-repo

repo sync

cp -R agl-jetson/meta-agl/templates/machine/jetson* meta-agl/templates/machine/

source meta-agl/scripts/aglsetup.sh -m jetson-tk1 agl-demo agl-appfw-smack

or

source meta-agl/scripts/aglsetup.sh -m jetson-tx1 agl-demo agl-appfw-smack


bitbake agl-demo-platform


"master" is unstable. If you find a problem, please write an Issue.


Install to SD card
====================================================================
SD card will assume that it is "/dev/sdx1".

sudo umount /dev/sdx1

sudo mkfs.ext4 /dev/sdx1

sudo mkdir -p /mnt/sdcard

sudo mount /dev/sdb1 /mnt/sdcard

cd /mnt/sdcard

sudo tar xvjf /path/to/work/agl-k1/tmp/deploy/images/jetson-tk1-upstream/agl-demo-platform-jetson-tk1-upstream.tar.bz2

sudo sync


AGL demo running Jetson TK1 board.
====================================================================
Power ON

login by root

cd /usr/AGL/apps
./installAllApps.sh

reboot

