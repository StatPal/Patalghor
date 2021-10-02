# High Performance Computing (HPC) cluster:

+ [Log in:](#log-in)
    * [To include Graphical outputs](#graphical-outputs)
+ [Data transfer](#data-transfer)
    * [Filezilla:](#filezilla)
    * [scp:](#scp)
+ [Run a job:](#run-a-job)
+ [Control job(s):](#control-jobs)


For many computation intensive works, one have to use high performance computing facilities usually available in universities. Sometimes universities set it up with jupyter notebook server or Rstudio server and life becomes easy. However, those are very specific slolution, specially if there are many types of users using many types of softwares. 

Enough chit-chat, let's go to the **main workflow** of an HPC, what you need to do:
 - <ins>Log in to HPC cluster</ins>: each time you want to work in HPC, you have to *login to your specified account in the specific HPC*, usually using command line interface. Often, you have to be inside campus internet, else, use VPN. If you don't have an account, contact IT/HPC guys. 
 - Data Transfer
 - Script file
 - Run a job
 - Control a job

(*Many HPC uses slurm-workload manager as cluster-management and job scheduling system. 
I am assuming that the HPC uses this facilty. To know more, see https://slurm.schedmd.com/overview.html *)


## Log in:
The HPC cluster nodes usually run some Linux/Unix-like Operating System. You have to use secure shell(**ssh**) to log-in (unfortunately no GUI type desktop manager to log-in.) **ssh** is available in Linux, Mac(use terminal); Windows(open Powershell). For old Windows, use *Putty ssh client*, or use *Rstudio terminal pane*(Rstudio users).

**To log-in, you have to something like:**
```bash
ssh YournameID@cluster.its.iastate.edu
```
Then you have to give your current Google Authenticator number (You have to set-up it for the first time) and your password. 
After successful login, you'll see something like: `[YournameID@cluster ~]$` by default. 



#### Graphical outputs: 
By default, graphical outputs are not coming to your computer. To get graphical outputs(in interactive mode, see later) you can actually do something like: 
```bash
ssh -Y YournameID@cluster.its.iastate.edu
```
(i.e., adding a -Y flag.) However, the connection gets relatively slow. 
In `R`, if you do nothing, it will automatically print all images stacked in a pdf file named `Rplots.pdf`. You can see it using Filezilla/scp/Globulus etc. 


## Data transfer

#### Filezilla:


#### scp: 
(secure copy)`scp` is basically like linux command `cp` (copy). The basic syntax of `cp` is:
`cp <path/if/needed/first_file_name> <path/if/needed/target_file_name>`
or **if you are copying to a different directory with the same name:**
```bash
cp <path/if/needed/first_file_name> <target/directory/path>
```

Similarly, **the scp in your case would be:**
```bash
scp <file> YournameID@condodtn.its.iastate.edu:/home/YournameID/needed/path/target_filename 
```
If you want to put the file in home diretory, you can just do something like: `scp <file> YournameID@condodtn.its.iastate.edu:`
For large files, you would possibly need to put files in `/work/<group>/<user>` folder. Your command would be something like:
`scp <file> YournameID@condodtn.its.iastate.edu:/work/<group>/<user>`




## Run a job:
To run a job after logging in (for interactive runs, see later), usually, you have to 
 - first have a program file to be run (such as `file.R` or `file.py` or `file.cpp` etc.)
 - Create a script/batch file (say `scriptfile.sh` or `scriptfile.script` etc.) which have the instructions about time-limit, memory-limit etc and how to run the program:
	 - Start with `#!/bin/bash` at the first line. 
	 - Specify max-time-limit etc such as `#SBATCH --time=01:00:00`
	 - Load the module which should run the job. (such as `module load r` or `module load python` or `module load gcc`)
	 - If you need python virtual enviornment load that by `source activate venv` where `venv` is the virtual env you want to work on. To create venv or other things, search by **conda cheat sheet**. (You will possibly need `module load miniconda3` or somehing beforehand.)
	 - Code to run your code (e.g., `Rscript --no-save file.R`, `python file.py` etc.)
 - Submit the job by `sbatch scriptfile.sh` (see Control jobs section for more control)


An example script:
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


## Control job(s):
To see all running or pending jobs:
`squeue`
To see the jobs corresponding to from your account:
```bash
squeue | grep <your name/id>
```




Many of the guides are written according to Iowa state high performance clusters (HPC).
There should be some similar ideas and commands to get the job done in some other computers too.
(https://www.hpc.iastate.edu/guides)

