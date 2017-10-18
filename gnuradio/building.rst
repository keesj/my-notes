Building GNURadio using pybombs
-------------------------------

There are different ways of building GNURadio and the way of building also
kinda constantly change. What has worked well for me was using `pybombs`_ and generally
talking on irc (freenode #gnuradio) or filing bugs when something was not working.

With that in mind my last installation when quite well. My requirements are that I do not 
want to install the GNURadio components as root (nor the pybombs code).


.. _pybombs: https://github.com/gnuradio/pybombs

tools::

	lxc-create -t download -n lxc-sdr -- --dist ubuntu --release xenial --arch amd64
	lxc-start --name lxc-sdr
	lxc-attach --name lxc-sdr
	apt-get update
	apt-get upgrade
	apt-get install libboost-all-dev libusb-1.0-0-dev python-mako doxygen python-docutils cmake build-essential tmux moreutils git vim python-pip python-apt openssh-server
	#qt stuff 
	apt-get -y install git-core cmake g++ python-dev swig pkg-config libfftw3-dev  libcppunit-dev libgsl0-dev libusb-dev libsdl1.2-dev  python-numpy python-cheetah python-lxml doxygen libxi-dev python-sip libqt4-opengl-dev libqwt-dev libfontconfig1-dev libxrender-dev python-sip python-sip-dev
	vim /etclocale.gen
	locale-gen
	pip install --upgrade pip
	userdel ubuntu
	adduser --disabled-password keesj
	gpasswd -a keesj sudo
	#edit sudoers files or add a passwd for the user
	su - keesj
	ssh-keygen
	vim .ssh/authorized_keys

First install pyboms in pybombs in ~/opt/pip and add the paths to PYTHONPATH and PATH

install pybombs::

	pip install --user git+https://github.com/gnuradio/pybombs.git

and add the following to you bashrc to make it understand where code can be found::

	export PATH="${PATH}:${HOME}/.local/bin"


Add the default recipes::

	pybombs recipes add gr-recipes git+https://github.com/gnuradio/gr-recipes.git  
	pybombs recipes add gr-etcetera git+https://github.com/gnuradio/gr-etcetera.git


Next setup a place where your GNURadio code will be. in this case my source and GNURadio will
be in ~/projects/sdr::

	mkdir ~/projects
	pybombs prefix init ${HOME}/projects/sdr -a main

Pybombs has a feature to use multiple workspaces but this is only usefull for advanced users (who might want to create a cross
compile environement) so best is to configure the above workspace to be the  the default place for your projects::

	pybombs config default_prefix main

Try and install a few components::

	ssh-keygen / copy authorized keys and ssh to machine (to have a controlling tty)
	pybombs -vvv install gnuradio gr-osmosdr

Add setup_env to you bashrc::

	. ${HOME}/projects/sdr/setup_env.sh

Package built this way to not install udev rules so you need to sometimes copy specific files
to udev. For rtl-sdr for example::

	cd ~/projects/sdr
	sudo cp ./src/rtl-sdr/rtl-sdr.rules /etc/udev/rules.d/
	sudo cp src/hackrf/host/libhackrf/53-hackrf.rules /etc/udev/rules.d/
	sudo udevadm control --reload

allow usb from the lxc container::

	root@700z:~# cat /var/lib/lxc/lxc-sdr/config 
	# usb
	lxc.mount.entry = /dev/bus/usb/ dev/bus/usb/  none bind,optional,create=dir
	lxc.cgroup.devices.allow = c 189:* rwm



https://bmullan.wordpress.com/2013/11/20/how-to-enable-sound-in-lxc-linux-containers/
/etc/pulse/default.pa::

	load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1;192.168.254.0/24 auth-anonymous=1

allow containers access to 4713:

sudo ufw allow from 10.0.3.1/24 to 10.0.3.1 port 4713

in you .bashrc:
export PULSE_SERVER=10.0.3.1:4713


gr-phosphor

http://sdr.osmocom.org/trac/wiki/fosphor
sudo apt-get install cmake xorg-dev libglu1-mesa-dev


