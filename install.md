# MCD Install

Create your working directory in `scratch/` and `cd` there. The download is 4.3GB.

## Download and export

The creators of the MCD ask that people do not share the download URL, but instead get it from https://www-mars.lmd.jussieu.fr/MCD_pro/mcd_pro.html so that they can track use of their code.

```sh
$ wget <you can get the download URL from https://www-mars.lmd.jussieu.fr/MCD_pro/mcd_pro.html>
$ tar xvf MCD_6.1.tar.gz
$ wget https://downloads.unidata.ucar.edu/netcdf-fortran/4.6.1/netcdf-fortran-4.6.1.tar.gz
$ tar xvf netcdf-fortran-4.6.1.tar.gz
$ wget https://downloads.unidata.ucar.edu/netcdf-c/4.9.2/netcdf-c-4.9.2.tar.gz
$ tar xvf netcdf-c-4.9.2.tar.gz
```

## Setup

```sh
$ ml git gcc hdf5/1.14.1-2
```

## Install NETCDF

```sh
$ cd netcdf-c
$ CPPFLAGS=-I/share/apps/centos7/hdf5/gcc/12.1.0/1.14.1-2/include/ LDFLAGS=-L/share/apps/centos7/hdf5/gcc/12.1.0/1.14.1-2/lib/ ./configure --prefix=/data/home/faw513/install/netcdf/
$ make -j$NSLOTS
$ make check install
$ export NCDIR=~/install/netcdf
$ export LD_LIBRARY_PATH=${NCDIR}/lib:${LD_LIBRARY_PATH}
$ export NFDIR=~/install/netcdf
$ cd netcdf-fortran
$ CPPFLAGS="-I${NCDIR}/include -O2" LDFLAGS=-L${NCDIR}/lib \
./configure --prefix=${NFDIR}
$ make -j$NSLOTS
$ make check install
```

## testing MCD

If all of the data is downloaded for all the scenarios, the full tests will pass.

```sh
$ cd MCD_6.1/mcd/
$ ln -s ../data/ MCD_DATA
# edit `compile` and replace NCDFINC and NCDFLIB with netcdf install location
$ ./compile
$ ./test_mcd < test_mcd.def
$ cd testcase/
$ ./TEST_SCRIPT
# some failures as data/warm/ doesn't exist at all
```

## Python bindings

```sh
$ git clone https://github.com/aymeric-spiga/mcd-python.git
$ cd mcd-python
$ ml python/3.6.3
$ python3 -m venv env
$ source env/bin/activate
$ pip3 install numpy
$ export PYTHONPATH=$PYTHONPATH:/data/scratch/faw513/mcd-python
$ python3 quicktest.py
```
