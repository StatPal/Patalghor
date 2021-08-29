# Installation guide for different programs/softwares in linux:


# Usual way to install packages from source:
 - Locate and download the source code (which is usually compressed)
 - Unpack the source code
 - Compile the code
 - Install the resulting executable
 - Set paths to the installation directory


The simplest way to compile a package (with `make`) is:
 - Goto the directory containing the package's source code.
 - Type `./configure` to configure the package for your system.
 - Type `make` to **compile the package**.
 - Optionally(*but recommended*), type `make check` to run any tests that come with the package.
 - Type `make install` to install the programs and any data files and documentation.
 - Optionally, type `make clean` to remove the program binaries and object files from the source code directory



I will show how to install nano-text-editor (latest stable version, to be installed **somewhere in your $HOME directory, not system-wise**)
This is **important** if you want to install something in HPC **without root access**

```bash
## Goto home directory
cd

## First make a directory specifically to download these kind of programs:
mkdir install_dir

## Download the source-code - copy the link from it's official website (https://nano-editor.org/download.php)
wget https://nano-editor.org/dist/v5/nano-5.8.tar.gz 

## Check installed tar-gzipped file and extract (unzip and untar, see notes for details) it:
tar -xvzf nano-5.8.tar.gz
## (x - extract, v - verbose, z - unzip at the same time, f - file)

## Goto the directory
cd nano-5.8

## mkdir new subdirectory(or any other directory where this program will be installed)
mkdir $HOME/install_dir/install_sub

## Configure utility settings with YOUR SPECIFIC DIRECTORY
./configure --prefix=$HOME/install_dir/install_sub

## build the package:
make

## Optional and recommanded check: 
make check 

## Install the package
make install

## (Optional) Clean the mess
make clean
```

This package is installed in the `$HOME/install_dir/install_sub` - but inside the `bin` folder there.
Some programs are installed in the exact folder (i.e., not in another `bin` subfolder.)
So, if you type `$HOME/install_dir/install_sub/bin/nano` it will open nano.
However, this is not always convenient. 

**Here, we would use a trick to open it (this version) by default**
**tl;dr**
Add this line to your ~/.bashrc or ~/.zshrc or equivalent:
```bash
export PATH=$HOME/install_dir/install_sub/bin:$PATH
```

Details:
We would export(*prepend*) this path of binary file (the `bin` location) to the system `PATH` so that when you just type `nano`, the nano is opened from this new path. For that, type this command in your terminal (each time you log in). 
```bash
export PATH=$HOME/install_dir/install_sub/bin:$PATH
```
To refrain yourself from doing this all the sessions, just add this line to the ~/.bashrc or ~/.zshrc or equivalent.








I have meant both unzip and untar by 'extract'. 
 - `zip`/`unzip` or `gzip`/`gunzip` (or equivalent) commands are to zip/unzip/gzip/un-gzip single files.
 - `tar` is the process of making a directory into one file.
Both are compressed into one command here. 
If you want to split the commands, it would be something like: 
```bash
gunzip nano-5.8.tar.gz 		## To un-gzip the file.

tar -xvf nano-5.8.tar 		## To unter the 'tar-file' into a proper folder/directory
```

For statisticians, you might come across many large `*.csv` data/other files. It is advisable to `gzip` them to save space.
**gzip** and **gunzip** are two simple related commands. R/python(using pandas) can read/write them directly.








## Using `pacman` for installation (arch or arch-derivative systems like Manjaro):


### Remove old installed packages, leave 1:
```bash
sudo paccache -rvk1
```


### Clean AUR built packages:

```bash
#Remove orphaned libraries
#pamac remove --orphans

#clean cache
pamac clean --build-files
```





### Clean temporary build files (in your home directory)
**From https://forum.manjaro.org/t/cleaning-up-and-freeing-disk-space/6703/31**
```bash
rm -rf ~/{.bundle,.cargo,.cmake,.dotnet,.electron,.electron-gyp,.gem,.gradle,.lazarus,.node-gyp,.npm,.nuget,.nvm,.racket,.rustup,.stack,.yarn} || true
rm -rf ~/.cache/{electron,electron-builder,go-build,node-gyp,pip,yarn} || true
sudo rm -rf ~/go || true
```





### Snap:
If you use snap, there is a high cance of creating huuuge files. One trck is to remove old versions and retain only last two:
```bash
sudo snap set system refresh.retain=2
```

To clean cache of snap
```bash
sudo rm /var/lib/snapd/cache/*
```

