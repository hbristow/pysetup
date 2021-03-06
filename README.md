# Python Setup for Scientific Computing
A short guide to setting up a Python environment for scientific computing on a Mac


### Introduction

This guide is designed to help you get a Python environment up and running for scientific computing and numerics, a la Matlab. It does not provide particular packages for domain specific tasks, but rather the package base to get started from. Effective numeric computing in Python is reliant on `numpy` with a fast BLAS backend. This guide puts a particular focus on getting that bit right.


### Homebrew

Homebrew is a package manager for the mac that provides Python package dependencies. In my experience of using both Homebrew and Macports, Homebrew is much more maintainable in the long run. You can get Homebrew at [http://brew.sh](http://brew.sh), or via the command:

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

1.  **Install Python**  
    It's potentially a good idea to keep your working Python environment separate from what the system requires, so we'll install an updated version of Python using brew:
    
        brew install python
    
    This generates a separate package namespace, and also installs pip, the package manager, which will be used to install the Python packages.

2.  **Install OpenBLAS**  
    OpenBLAS is a fast open-source BLAS and LAPACK implementation. We'll use it as a backend for Python's numeric library, `numpy`. It's best to build it from source on your architecture for optimal speed:
    
        brew install homebrew/science/openblas --build-from-source 

3.  **Install FFTW3 (Optional)**  
    FFTW is a library for performing Fast Fourier Transforms. We'll install a Python wrapper for it, but first we need the C/Fortran library. Again, build from source to get the maximum performance:
    
        brew install fftw --build-from-source


### PIP

PIP is the de facto package manager for Python. It installs packages from the Python Package Index (PyPI), as well as any URL pointing to a Git or Mercurial repository. If you installed Python using Homebrew, then you'll already have PIP, otherwise it can be installed manually:

    curl -fsSL https://bootstrap.pypa.io/get-pip.py > get-pip.py
    python get-pip.py
    rm get-pip.py


### Numpy Setup

Now that we have OpenBLAS and (optionally) FFTW, we want to build numpy against these libraries. The configuration is provided in `.numpy-site.cfg` and can be downloaded automatically into place with:

    curl -fsSL https://raw.githubusercontent.com/hbristow/pysetup/master/.numpy-site.cfg > ~/.numpy-site.cfg
    
NOTE: this file *must* be written to your home directory with the given name.

Next, install numpy using `pip`. The `--no-use-wheel` option forces it to be built from source. The `-march=native` flag passed to Clang will enable the full feature suite of your processor (SSE, AVX, etc):

    CFLAGS="-march=native" pip install --no-use-wheel numpy


### Python Packages

Now that the core numeric library, `numpy`, has been installed, a number of useful companion packages can be installed. I've put together a concise list in `requirements.txt`. `pip` can install all of these packages and their dependencies in one fell swoop:

    pip install -r https://raw.githubusercontent.com/hbristow/pysetup/master/requirements.txt


### Test

To test whether numpy has successfully built against OpenBLAS, boot up the new interpreter you just installed:

    ipython
    
Then import numpy and display the config:

    import numpy
    numpy.show_config()
    
This should contain information about paths to BLAS and LAPACK and, importantly, make mention of OpenBLAS if indeed it was found.
