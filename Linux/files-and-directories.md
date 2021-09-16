
## Wroking with files and directories

### ls i.e., list
`ls` **list**s the files in the current directory. 
```bash
ls
```

To list the *hidden* files also, you can add `-a` flag to the `ls`, i.e., 
```bash
ls -a
```


### mkdir i.e., make directory
`mkdir` makes a new directory with the specified name
```bash
mkdir new_dir
```

### cd i.e., change directory
`cd` changes the current directory to the specified name
```bash
cd new_dir
```
To go to the parent directory(see later), one have to do
```bash
cd ..
```
To go in a subdirectory directly, you can do something like:
```bash
cd new_dir/new_subdir
```

### pwd i.e., print working directory
```bash
pwd
```

## Linux filesystem
** Usually, in linux systems, there is a root directory, denoted by `/`, which usually requires special access permissions. In your personal machine, usually you can get that by proper password. However, in other machine like an HPC cluster, it is not so. 
Inside this root directory, usually there are different system-related directories like [source](https://www.linux.com/training-tutorials/linux-filesystem-explained/)
 - `bin`: /bin is the directory that contains binaries, that is, some of the applications and programs you can run. You will find the ls program mentioned above in this directory, as well as other basic tools for making and removing files and directories, moving them around, and so on.
 - `home`: **/home is where you will find your users’ personal directories.** In many cases, under /home there are directories corresponding to the users like: /home/statpal, which contains all my stuff; and /home/guest, in case anybody needs to borrow my computer.
 - `boot`: The /boot directory contains files required for starting your system. **DO NOT TOUCH!** unless you absolutely know what you are doing.
 - `lib`: /lib is where libraries live. Libraries are files containing code that your applications can use. They contain snippets of code that applications use to draw windows on your desktop, control peripherals, or send files to your hard disk.
 - `dev`: /dev contains device files. Many of these are generated at boot time or even on the fly. For example, if you plug in a new webcam or a USB pendrive into your machine, a new device entry will automagically pop up here.
 - ...

Now we will look at some common directories:
* A few common directories:
  + Parent directory corresponds to the '..'
  + Home directory corresponds to the '~'
  + Root directory corresponds to the '/'
  + Current directory is also denoted by './'

So, if you write `cd ~`, it will mean go to the home folder. If you write `cd ..`, you wil change the active directory to the parent directory. 

