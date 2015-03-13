# cvsetup
A short guide to setting up a Python environment for Computer Vision (CV) on a Mac

### Homebrew

Homebrew is a package manager for the mac that provides Python package dependencies. You can get brew at [http://brew.sh](http://brew.sh), or via the command:

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

1. Install Python. It's often a good idea to keep keep your Python environment separate to what the system requires, so we'll install an updated version of Python using brew:

        brew install python
    
  This also installs pip, the package manager, which will be used to install the Python packages
