# General Workflows

This document describes some workflows central to Plasma Phone development.

* Accessing packages through OBS
* Setting up the SDK to build packages locally
* Hacking-Testing workflow (TODO)
* Building new images (TODO)
* Branching and updating packages (TODO)
* Creating new packages (TODO)

## Tools

* osc is the open build service command line client, it allows to fetch and upload new packages to OBS
* spectacle is a tool to convert yaml package descriptions to spec files
* The Mer SDK is needed to build packages locally. This allows to test changes locally on the device by creating a package on your machine, then uploading it to and installing it on the phone
* zypper is the tool used for package management, packages in Mer are in RPM format.

To make the following steps a lot more convenient, I suggest to follow <https://github.com/plasma-mobile/docs/blob/master/Convenience_Setup.md> first.

## Setting up the SDK

The Mer SDK is basically a chroot environment that you can install on your local system. It has package repos, tools and everything you'll need set up already. To make data sharing between the SDK chroot and your local system easy, it has your home directory mounted inside the SDK, so be careful!

Follow the guide at <https://wiki.merproject.org/wiki/Platform_SDK>

In very short (but really, read the guide!):
```
export MER_ROOT=$HOME/Mer
cd $HOME; curl -k -O https://img.merproject.org/images/mer-sdk/mer-i486-latest-sdk-rolling-chroot-armv7hl-sb2.tar.bz2 ;
sudo mkdir -p $MER_ROOT/sdks/sdk ;
cd $MER_ROOT/sdks/sdk ;
sudo tar --numeric-owner -p -xjf $HOME/mer-i486-latest-sdk-rolling-chroot-armv7hl-sb2.tar.bz2 ;
echo "export MER_ROOT=$MER_ROOT" >> ~/.bashrc
echo 'alias sdk=$MER_ROOT/sdks/sdk/mer-sdk-chroot' >> ~/.bashrc ; exec bash ;
echo 'PS1="MerSDK $PS1"' >> ~/.mersdk.profile ;
cd $HOME
sdk
```

With that out of the way, we can start grabbing the package sources.

## Checking out packages

For that purpose, and to keep them all in one place, I've created a directory ~/Mer in my home directory. That's where all the packages go, separated into subdirectories per "obs project".

Also, you'll need an account at merproject.org. Get one via https://bugs.merproject.org/createaccount.cgi . Warning: This may involve manual steps by a Mer admin, so it may take a bit.

Once you have a Mer account, try the following:
```
sdk # to enter the SDK environment
cd ~/Mer
osc -A https://api.merproject.org ls home:plfiorini:phone
```

Now, check out the Phone repository:
```
cd ~/Mer
osc co home:plfiorini:phone
```
This will ask for your username and password. The -A parameter only needs to be specified once, from now on, osc should remember the API endpoint, and life becomes easier.

Now you should find a subdirectory home:plfiorini:phone in your ~/Mer directory, containing a bunch of packages with familiar-looking names.

Note: The "osc" tool is actually a wrapper around the svn command. It works similarly to it, and many things you may have learned from subversion apply here as well.

## Project Status

If you want to check the build status of a package:
https://build.merproject.org/project/show/home:plfiorini:phone


## Setting up OBS access using osc

Generic instruction for Mer can be found here <https://wiki.merproject.org/wiki/Osc_Setup>

### Installing tools

Note: You can use the Mer SDK. If you're within the SDK environment, tools like osc and spectacle are already available.

On Debian, the following packages will pull in what's needed:
```
apt-get install osc spectacle obs-build
```


On Ubuntu, this failed for me, since obs-build seems to not be in the repositories anymore in 14.10. The package from Debian unstable works for me, though:

```
sudo apt-get install rpm debugedit spectacle osc
wget http://ftp.us.debian.org/debian/pool/main/o/obs-build/obs-build_20141024-1_all.deb
sudo dpkg -i obs-build_20141024-1_all.deb
```

(Possibly run sudo apt-get install -f to install packages that may still be missing at this point.)

### Fixing the tools
obs-build installs a script called obs-build, but osc expects a script called "build". Symlinking this in your PATH will help:
```
cd /home/sebas/bin
ln -s /usr/bin/obs-build build
```
