# General Workflows

## Setting up the SDK

Follow the guide at
https://wiki.merproject.org/wiki/Platform_SDK


## Setting up OBS access using osc

### Installing tools

On Debian, the following packages will pull in what's needed:
```
apt-get install osc spectacle obs-build
```

On Ubuntu, this failed for me, since obs-build seems to not be in the repositories anymore. The package from Debian unstable works for me, though:

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

With that out of the way, we can start grabbing the package sources.

## Checking out packages

For that purpose, and to keep them all in one place, I've created a directory ~/Mer in my home directory. That's where all the packages go, separated into subdirectories per "obs project".

Also, you'll need an account at merproject.org. Get one via https://bugs.merproject.org/createaccount.cgi . Warning: This may involve manual steps by a Mer admin, so it may take a bit.

Once you have a Mer account, try the following:
```
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

https://build.merproject.org/project/show/home:plfiorini:phone
