# rat_singularity
This repository provides an easy way to manage and organize multiple version of the Singularity def file used to build jsns2-RAT.

## Installing singularity
To use this def file to build (and run) a container containing RAT, you need to install [Singularity](https://singularity.lbl.gov/) first. 

Many Linux distributions have a pre-compiled version of singularity in their package manager, e.g. you can run

`sudo apt-get install singularity-container` for Debian based distributions of Linux (such as Ubuntu).

For Linux distributions not having Singularity in their official package repository, you can always build from source:
```
git clone --branch v3.6.4 https://github.com/singularityware/singularity.git
cd singularity
./autogen.sh
./configure --prefix=/usr/local
make
sudo make install
```

If you encounter any complications, please refer to the official [Singularity documentation](https://singularity.lbl.gov/install-linux)

## Building the Singularity container from the def file
Since the JSNS2-RAT repository is private, it is necessary to manually copy it into the container on build. First clone the JSNS2-RAT repository to your current working directory via

`git clone \<jsns2-rat repository\>`
  
We need to put the 'rat-pac-jsns2-v2.0' folder into our working from directory, from which we then call:

`sudo singularity build --sandbox sl7_rat sl_rat.def`

It is safer to specify the "--sandbox" flag here, since I had problems with singularity running out of temporary space. After your container finished building, you need to type into the CLI (we specify the "--writable" flag because we want our changes to be safed)
```
sudo singularity shell --writable sl7_rat
cd /opt/RAT/rat-pac-jsns2-v2.0
scons
```
and wait for scons to finish compiling. If everything worked an there were no error messages, you can take a well deserved sip of what ever beverage you prefer and move to the next step.

As and additional step, we convert the image from sandbox (where you can interact with the folder) to a more space efficient image in the singularity format via

`sudo singularity build sl7_rat.img sl7_rat`

## Running jsns2-rat using Singularity
To run RAT using your singularity container is quite easy. If you are not already in the singularity container shell, run

`singularity shell sl7_rat.img`

to get into it.

You can now use rat in the way you are used to it, except that you will not have any GUI capabilities. For example, you can run any macro via

`rat example.mac`

