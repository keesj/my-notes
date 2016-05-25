Building GNURadio using pybombs
-------------------------------

There are different ways of building GNURadio and the way of building also
kinda constantly change. What has worked well for me was using `pybombs`_ and generally
talking on irc (freenode #gnuradio) or filing bugs when something was not working.

With that in mind my last installation when quite well. My requirements are that I do not 
want to install the GNURadio components as root (nor the pybombs code).


.. _pybombs: https://github.com/gnuradio/pybombs


First install pyboms in pybombs in ~/opt/pip and add the paths to PYTHONPATH and PATH

install pybombs::

	pip install git+https://github.com/gnuradio/pybombs.git

and add the following to you bashrc to make it understand where code can be found::

	export PATH="${PATH}:${HOME}/.local/bin"


Add the default recipes::

	pybombs recipes add gr-recipes git+https://github.com/gnuradio/gr-recipes.git  
	pybombs recipes add gr-etcetera git+https://github.com/gnuradio/gr-etcetera.git


Next setup a place where your GNURadio code will be. in this case my source and GNURadio will
be in ~/projects/sdr::

	pybombs prefix init ${HOME}/projects/sdr -a main

Pybombs has a feature to use multiple workspaces but this is only usefull for advanced users (who might want to create a cross
compile environement) so best is to configure the above workspace to be the  the default place for your projects::

	pybombs config default_prefix main

Try and install a few components::

	pybombs -vvv install gnuradio gr-osmosdr

Add setup_env to you bashrc::

	. ${HOME}/projects/sdr/setup_env.sh

Package built this way to not install udev rules so you need to sometimes copy specific files
to udev. For rtl-sdr for example::

	cd ~/projects/sdr
	sudo cp ./src/rtl-sdr/rtl-sdr.rules /etc/udev/rules.d/
	sudo cp src/hackrf/host/libhackrf/53-hackrf.rules /etc/udev/rules.d/
	sudo udevadm control --reload
