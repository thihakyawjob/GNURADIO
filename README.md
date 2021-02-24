# GNURADIO

# Installing gnuradio 3.8

from: https://gist.github.com/gbevan/8e583b9cf87aa3c58102251454fa48a6

## Setup PyBOMBS

sudo apt-get install python-pip python-apt
sudo -H pip install PyBOMBS
mkdir ~/sdr
cd ~/sdr
pybombs auto-config
pybombs recipes add gr-recipes git+https://github.com/gnuradio/gr-recipes.git
pybombs recipes add gr-etcetera git+https://github.com/gnuradio/gr-etcetera.git
pybombs prefix init ~/sdr


## Edit setup_env.sh for python3

sed -i 's/2\.6/3.6/g' setup_env.sh
sed -i 's/2\.7/3/g' setup_env.sh


## Edit gnuradio recipe
* Edit `~/.pybombs/recipes/gr-recipes/gnuradio.lwr`
* Ensure the "gitbranch" is `maint-3.8`.
* In gnuradio.lwr recipe file, add "-DENABLE_CTRLPORT_THRIFT=OFF" in "config_opt".

## Install uhd and prereqs

source ~/sdr/setup_env.sh
pybombs install uhd
sudo apt install git cmake g++ libboost-all-dev libgmp-dev \
  swig python3-numpy python3-mako python3-sphinx python3-lxml \
  doxygen libfftw3-dev libcomedi-dev libsdl1.2-dev libgsl-dev \
  libqwt-qt5-dev libqt5opengl5-dev python3-pyqt5 liblog4cpp5-dev \
  libzmq3-dev python3-yaml python3-click python3-click-plugins


## Install gnuradio 3.8

pybombs install gnuradio


## Fix gr-osmosdr and gr-iqbal
### gr-iqbal

category: common
depends:
- gnuradio
- libosmo-dsp
description: Gnuradio I/Q balancing
#gitbranch: master
gitbranch: maint-3.8
inherit: cmake
# Let's always build this from source, not binaries
#source: git+https://git.osmocom.org/gr-iqbal
source: git+https://github.com/velichkov/gr-iqbal.git


## gr-osmosdr

category: common
depends:
- uhd
- rtl-sdr
- osmo-sdr
- hackrf
- gnuradio
- gr-iqbal
- bladeRF
- airspy
- soapysdr
description: Interface API independent of the underlying radio hardware
#gitbranch: master
gitbranch: maint-3.8
inherit: cmake
# Let's always build this from source, not binaries
#source: git+https://git.osmocom.org/gr-osmosdr
source: git+https://github.com/velichkov/gr-osmosdr.git


## Install gr-osmosdr

pybombs install gr-osmosdr


## Running gnuradio-companion

source ~/sdr/setup_env.sh
gnuradio-companion


