# High Performance Computing (HPC) cluster:

For many computation intensive works, one have to use high performance computing facilities usually available in universities. Sometimes universities set it up with jupyter notebook server or Rstudio server and life becomes easy. However, those are very specific slolution, specially if there are many types of users using many types of softwares. 

## Main workflow
Enough chit-chat, let's go to the **main workflow** of an HPC:
 - ***[Log in to HPC cluster](#log-in):*** Each time you want to work in HPC, you have to *login to your specified account in the specific HPC*, usually using command line interface. You can edit files, start/run jobs, stop jobs etc from logged in state. Often, you have to be inside campus internet, else, use VPN. If you don't have an account, contact IT/HPC guys. There can be some common confusions about graphics unavailibility which are answered [here](#qn-i-cant-see-my-plots)
 - ***[Data Transfer:](#data-transfer)*** You have to transfer data/code file from your computer to HPC and vice versa. You can do it using GUI [Filezilla](#filezilla) or command line based [scp](#scp) or Globulus. Each has it's own pros and cons. 
 - <ins>***[Script file and running a job:](#run-a-job)***</ins> Usually HPC have many nodes/parts. When you login to cluster, you are in *head node*. Here you may do some small jobs there, not heavy jobs. There are other *computing nodes* where you can *submit* your job/code though a *script file.* The *script file* would contain: 
    + Specification of time limits, (and RAM limit, core usage), email notification system etc 
    + Possibly loading the *modules* or preinstalled programs (such as `module load r`)
    + Command to run your original R/python/c/cpp etc script. 
 - <ins>[Control a job](#control-jobs)</ins>: Abort a job or check how it is going...

(*I am assuming that the HPC uses this slurm-workload manager as cluster-management and job scheduling system and the commands are according to this manager. To know more, see https://slurm.schedmd.com/overview.html*)


------

## Log in:
The HPC cluster nodes usually run some Linux/Unix-like Operating System. You have to use **ssh**(secure shell) to log-in (unfortunately no GUI as I know.) **ssh** is available in Linux, Mac; Windows(open Powershell). For old Windows, use *Putty ssh client*, or use *Rstudio terminal pane*(Rstudio users).

**To log-in, write like this (or equivalent):**
```bash
ssh YournameID@nova.its.iastate.edu
```
After pressing enter, it will take your current Google Authenticator number (You have to set-up it for the first time) and then your password. 
After successful login, you'll see something like: `[YournameID@nova ~]$`. 

If you log-in to some other machine, the part after the `@` would change, such as `condo2017.its.iastate.edu` etc. 

If you are done, you can use `Ctrl+D` or `logout` to log out. 



## Data transfer

#### Filezilla:
The steps to open Filezilla and use the program to up/download files to the HPC server would be:
First, let's talk about settings(one time for each HPC machine)
 - Open Filezilla
 - File -> SiteManager (or click in the small icon beneath 'file')
 - New Site
 - In host (new site will be highlighted, give it a memorable simple name, e.g., `condo`)
 - Goto Right Hand Side. In the general tab on the right side, put:
     * Host: condodtn.its.iastate.edu
     * Change protocol to SFTP 
     * set user to your NETID
     * set login type to "Interactive"
     * (Optional, for security) Go to transfer settings tab
     * (Optional, for security) check limit number of simultaneous connections
     * (Optional, for security) Make sure maximum is 1 (this is crucial)
     * (Optional, for security) click ok

Now to connect (each time),
 - Open Filezilla
 - Go to file -> sitemanager (or click in the small icon beneath 'file')
 - select the site you created
 - click connect
 - 
 - When asked for password, first enter the token from google authenticator. 
 - Hit enter. 
 - And then enter the actual password.

(This part is little bit tweaked from Somak Dutta's email)


#### scp: 
** For Intermediate users,

`scp` (secure copy) is basically like linux command `cp` (copy). The main idea is `cp first_file target_file_name_or_directory` (with proper path if needed). Similarly, **the scp in your case would be:**
```bash
scp <file> YournameID@condodtn.its.iastate.edu:/home/YournameID/needed/path/target_filename 
```
If you want to put the file in home diretory, you can just do something like: `scp <file> YournameID@condodtn.its.iastate.edu:`

*For large files, you would possibly need to put files in `/work/<group>/<user>` folder. Your command would be something like:*
`scp <file> YournameID@condodtn.its.iastate.edu:/work/<group>/<user>`


(The basic syntax of `cp` is:
`cp <path/if/needed/first_file_name> <path/if/needed/target_file_name>`
or *if you are copying to a different directory with the same name:*
`cp <path/if/needed/first_file_name> <target/directory/path>`)


## Run a job:
To run a job after logging in (for interactive runs, see [later](#interactive-jobs)), usually, you have to 
 - First, have a program file to run (such as `file.R` or `file.py` or `file.cpp` etc.)
 - Create a script/batch file (say `scriptfile.sh` or `scriptfile.script` etc.) which have the instructions about time-limit, memory-limit etc and how to run the program:
	 - Start with `#!/bin/bash` at the first line. 
	 - Specify max-time-limit etc such as `#SBATCH --time=01:00:00`
	 - Load the module which should run the job. (such as `module load r` or `module load python` or `module load gcc`)
	 - If you need python virtual enviornment load that by `source activate venv` where `venv` is the virtual env you want to work on. To create venv or other things, search by **conda cheat sheet**. (You will possibly need `module load miniconda3` or somehing beforehand.)
	 - Code to run your code (e.g., `Rscript --no-save file.R`, `python file.py` etc.)
 - Submit the job by `sbatch scriptfile.sh` (see Control jobs section for more control)

to create the script file, you may use any text editor (nano, vim etc - look [here](text-editor.md) for text editing in linux) while you are logged in -- or, you can create the script file in your local machine and then upload that to HPC. 


An example script: (you can get a script generator [here for condo](https://www.hpc.iastate.edu/guides/condo-2017/slurm-job-script-generator-for-condo) or [here for nova](https://www.hpc.iastate.edu/guides/nova/slurm-script-generator-for-nova))
```bash
#!/bin/bash

# Copy/paste this job script into a text file and submit with the command:
#    sbatch thefilename
# job standard output will go to the file slurm-%j.out (where %j is the job ID)

#SBATCH --time=1:00:00   					# walltime limit (HH:MM:SS); (required)
#SBATCH --nodes=1   						# number of nodes
#SBATCH --ntasks-per-node=16   				# 16 processor core(s) per node 
#SBATCH --mem=100G   						# maximum memory per node
#SBATCH --mail-user=yourname@domain.edu   	# email address
#SBATCH --mail-type=BEGIN					# To get email when the job starts
#SBATCH --mail-type=END						# ...
#SBATCH --mail-type=FAIL					# ... to 'unsubscribe', delete the corresponding line(s)

# LOAD MODULES 
module load r 

# RUN YOUR PROGRAMS HERE 
Rscript --no-save  filename.R
```

To run the job, you have to issue `sbatch example.script` For an intermediate user, I would suggest to look at `sbatch --help` or `sbatch -h` or this too much detailed [link](https://slurm.schedmd.com/sbatch.html). 

## Control job(s):
As you have possibly noticed, to run a script file, you have to use
```bash
sbatch example.script
```
To see all running or pending jobs:
```bash
squeue
```
To see the jobs corresponding to from your account:
```bash
squeue | grep <your name/id>
```

## Interactive Jobs
Many times, you need to run the program for a small data/instance or debug before/after the actual program to run. In that case, you might want an interactive R/python where you can basically run line by line. Or you can run a C file, and check errors, and then edit the file accordingly and run again and check errors etc. In these types of cases, you would possibly need an **interactive session** as opposed to a *job submission* where you technically have no control if your job have started running. 

To have that interactive session for say 1 hour, you have to issue:
```bash
salloc -N1 -t 1:00:00
```
The `-N1` part is saying that you need 1 node and `-t 1:00:00` is asking such a node for 1 hour. You can add more options here also. For an intermediate user, I would suggest to look at `salloc --help` or `salloc -h`. 


### FAQ

#### Qn: I can't see my plots when I am running my program: 
Suppose, you are plotting an image in some software like R (possibly line by line, i.e., interactively), but you are not getting any graphical outputs. What can you do? 

By default, graphical outputs will not be forwarded from HPC to your computer. To get graphical outputs, while logging in, you can actually do something like: 
```bash
ssh -Y YournameID@cluster.its.iastate.edu
```
(i.e., adding a -Y flag.) However, the connection gets relatively slow. 
If you are running `R`, if you do nothing, it will automatically print all images stacked in a pdf file named `Rplots.pdf` in the same folder. You can see it using Filezilla/scp/Globulus etc. 

***Warning***, do all these experiments in **interactive mode**. Heavy computations in head node is discouraged. 



Many of the guides are written according to Iowa state high performance clusters (HPC).
There should be some similar ideas and commands to get the job done in some other computers too.
(https://www.hpc.iastate.edu/guides)

