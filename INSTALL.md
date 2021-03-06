# Audiogram installation

Audiogram has a number of dependencies:

* [Node.js/NPM](https://nodejs.org/) v0.11.2 or greater
* [node-canvas dependencies](https://github.com/Automattic/node-canvas#installation)
* [FFmpeg](https://www.ffmpeg.org/)
* [libgroove](https://github.com/andrewrk/libgroove)

If you're using a particularly fancy distributed setup you'll also need to install [Redis](http://redis.io/).

Installation has been tested on Ubuntu 14.04 and 15.04. It has also been tested on various Mac OS X environments, with various degrees of Homebrew Hell involved.

This would theoretically work on Windows, but it hasn't been tested.

## Ubuntu 14.04+ installation

An example bootstrap script for installing Audiogram on Ubuntu looks like this:

```sh
# 14.04 only: add PPAs for FFmpeg and Libgroove
# Not required for 15.04
sudo add-apt-repository ppa:mc3man/trusty-media --yes
sudo apt-add-repository ppa:andrewrk/libgroove --yes

# Update/upgrade
sudo apt-get update --yes && sudo apt-get upgrade --yes

# Install:
# Node/NPM
# Git
# node-canvas dependencies (Cairo, Pango, libgif, libjpeg)
# FFmpeg
# node-waveform dependencies (libgroove, zlib, libpng)
sudo apt-get install git nodejs npm \
libcairo2-dev libjpeg8-dev libpango1.0-dev libgif-dev build-essential g++ \
ffmpeg \
libgroove-dev zlib1g-dev libpng-dev \
--yes

# Install Redis if you plan to use it to share rendering among multiple processes/servers
# If you don't need to handle multiple users, you can skip this step
sudo apt-get install redis-server --yes

# Fix nodejs/node legacy binary nonsense
sudo ln -s `which nodejs` /usr/bin/node

# Upgrade node to the latest stable version
# Or use a different method of your choosing (e.g. nvm)
# Any node version later than v0.11.2 should work
sudo npm install -g n
sudo n stable

# You'll probably need to reconnect to see the new Node version reflected

# Clone the audiogram repo
git clone https://github.com/nypublicradio/audiogram.git
cd audiogram

# Install local modules from NPM
npm install

# If this worked, you're done
# If you get an error about `make` failing,
# you may need to ensure that node-gyp is up-to-date
# You may even need to run this command twice, because computers
sudo npm install -g node-gyp

# If you had to update node-gyp, try again
npm install
```

## Mac OS X installation

Installing on a Mac can get a little rocky. Essentially, you need to install three things:

1. [Node.js/NPM](https://nodejs.org/)
2. [node-canvas dependencies](https://github.com/Automattic/node-canvas#installation)
4. [libgroove](https://github.com/andrewrk/libgroove)

You probably don't need to install FFmpeg separately because libgroove depends on it.

You can install Node.js by [downloading it from the website](https://nodejs.org/).

Installation of node-canvas dependencies and libgroove might look like the following with [Homebrew](http://brew.sh/) (you'll want to make sure [XCode](https://developer.apple.com/xcode/) is installed and up-to-date too):

```sh
# Install Git if you haven't already
brew install git

# Install Cairo, Pango, libgif, libjpeg, libgroove, and FFmpeg
# You may not need to install zlib
brew install pkg-config cairo pango libpng jpeg giflib libgroove ffmpeg

# Go to the directory where you want the audiogram directory
cd /where/to/put/this/

# Clone the repo
git clone https://github.com/nypublicradio/audiogram.git
cd audiogram

# Install from NPM
npm install
```

## Mac troubleshooting

If things aren't working on a Mac, there are a few fixes you can try.

### Brew troubleshooting

Follow the [Homebrew troubleshooting guide](https://github.com/Homebrew/brew/blob/master/share/doc/homebrew/Troubleshooting.md#troubleshooting), particularly making sure that XCode is up to date.

### Installing libgroove manually

If you're on a Mac and installing [libgroove](https://github.com/andrewrk/libgroove) with Homebrew failed for some reason, you can try following the instructions in the repo to install it more manually.

### Installing XCode command line tools

Audiogram uses [waveform](https://github.com/andrewrk/waveform), which requires the development version of `zlib`. Your Mac might already have this, but if it doesn't, installing XCode command line tools might help:

```sh
xcode-select --install
```

### Updating node-gyp

Updating node-gyp to a current version with:

```sh
npm install -g node-gyp
```

may help with `npm install` errors.

### Updating Node.js

If you get an error about `path.isAbsolute` not being a function, you're running a pretty old version of [Node.js/NPM](https://nodejs.org/). Upgrading to anything later than v0.11.2 should help.

### Installing FFmpeg with the compilation guide

If FFmpeg installation is failing, you can try following the [compilation guide](https://trac.ffmpeg.org/wiki/CompilationGuide).

### Installing node-canvas dependencies manually

You can try installing the node-canvas dependencies with their detailed [Installation instructions](https://github.com/Automattic/node-canvas/wiki/_pages).  You don't need to install `node-canvas` itself, just everything up to that point.
