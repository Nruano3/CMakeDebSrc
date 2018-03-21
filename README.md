# cmake-deb-src

Building debian binary packages is easy with CMake/CPack, but CMake/CPack lack
the ability to generate debian source packages that can be uploaded to
LaunchPad and other debian package building systems, such as Open Build
Service. This project is a collection of CMake functions to simplify building,
testing, and uploading debian source packages.

# Setup

## Install required packages

On Ubuntu 16.04, install the required packages:

    $ sudo apt-get install pbuilder ubuntu-dev-tools

## Setup an Ubuntu Distribution pbuild Environment

    $ pbuilder create
    $ pbuilder-dist xenial create

Allow pbuilder to have access to the network during build-time:

    $ echo 'USENETWORK=yes' | sudo tee -a /etc/pbuilderrc



# Setup your GPG Key

See [Using Passwords and Encryption Keys to manage OpenPGP keys](https://help.launchpad.net/YourAccount/ImportingYourPGPKey)

# Build the Example

    $ cd ./example
    $ mkdir build && cd build
    $ cmake .. -DPPA=ppa:kevin-demarco/cmake-project-template \
               -DGPG_KEY_ID=3951DA01

where the PPA and GPG\_KEY\_ID are correct for your system.

## Create a debian source package

    $ make pybind11-debuild

## Use pbuilder to perform a local test build of your source package

    $ make pybind11-local-test

## Use the debian source package to LaunchPad

    $ make pybind11-upload-ppa

# Understanding the Example

In the example directory, there is a CMake project called "MyCoolProject" that
uses the "BuildDebSrcFromRepo" CMake function to generate a debian source
package from a git repository. Make sure you use the
"find_package(CMakeDebSrc)" cmake command to get access to the
"BuildDebSrcFromRepo" function. Every debian source package requires a debian
folder with at least the following files: changelog, control, copyright, and
rules. You will find the debian directory for the pybind11 package under
./example/packages/pybind11/debian. You can use these files as a starting point
for packaging your own project.
