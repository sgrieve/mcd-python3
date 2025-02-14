### Python 3 Fork

This is a minimal fork of MCD-PYTHON to port it to Python 3, and get it running on the QMUL Apocrita HPC system. This has been tested using Python 3.6.3 - newer versions of python seem to cause issues that may or may not be resolved in the future.

**THIS VERSION REQUIRES A SYMLINK TO BE CREATED IN THE ROOT OF THIS REPO POINTING TO THE MCD DATA**

The original readme is reproduced below:

**MCD-PYTHON: python-based interface to the Mars Climate Database**

Open source code and contact information [available on github](https://github.com/aymeric-spiga) [no registration needed]

* To get sources through git
~~~
git clone https://github.com/aymeric-spiga/mcd-python
~~~

* To get sources through SVN
~~~
svn co https://github.com/aymeric-spiga/mcd-python/trunk mcd-python
~~~

* To get a static ZIP file of the current version of the code,
[click here](https://github.com/aymeric-spiga/mcd-python/archive/master.zip)

----

**How to install?**

It is assumed you were able to compile successfully the `mcd` sources with `gfortran`.

The `netCDF` library should be installed on your system.
Moreover, from our experience, it seems that it must have been build
using the `-fPIC` (for `gfortran`; the name of the option changes with compilers)
option which generates position independent code suitable for use in a shared library.
An example script is given in the `netcdf` folder.

The installation below relies on `f2py` utility, which is part of the `numpy` package.

 1. Getting the environment variables right: add the `mcd-python` folder to `PYTHONPATH` in your environment file (e.g. `.bashrc`)

        export PYTHONPATH=$PYTHONPATH:adapt_to_your_own/mcd-python

 2. Modify the compile script `compile_fmcd.sh` to link your local `netCDF` libraries and `mcd` distribution (Fortran sources)

 3. Check that `f2py` is included in your `python` library suite.

 4. Run `compile_fmcd.sh` and check for the created `.so` file (its size should be about 1 Mo)

----

**Quick test**

~~~
quicktest.py
~~~

Next step is to try and learn about the use of `mcd` Python library with the `tutorial` folder.

A more advanced example (direct use of `fmcd` compiled with `f2py`) is provided in the `test_mcd` folder.

----

**Python 3**

*Solution suggested by Aaron Berliner*

This can be done using the 2to3 package and the reindent

    Run 2to3 -v -n -W -f all mcd.py
    Run 2to3 -v -n -W -f all mcdcomp.py
    Run reindent mcd.py
    Run reindent mcdcomp.py

Then upgrade to the appropriate basemap in python3.
