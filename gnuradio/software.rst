SDR software
============

GQRX
----

GQRX_ Is an easy to use tool to simply have a look at the spectrum and offers a nice way of validating the hardware is working properly. It is part 
of pybombs but for source install on need to replace the configuration resolve order e.g. from "native,src" into "src,native"

cat ~/.pybombs/config.yml::

	config:
	  default_prefix: main
	  satisfy_order: src,native
	prefix_aliases:
	  main: /home/keesj/projects/sdr
	recipes:
	  gr-etcetera: git+https://github.com/gnuradio/gr-etcetera.git
	  gr-recipes: git+https://github.com/gnuradio/gr-recipes.git

.. _GQRX: http://gqrx.dk/ 



Web based UI:
csdr 

Software Defined Radio Academy 
https://www.youtube.com/playlist?list=UU1GAlgAQrkjeeLmIkCB8pgQ


Awesome links:

http://www.windytan.com/
