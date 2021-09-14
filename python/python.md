# Python virtual environment (with conda):

- [Python (with conda):](#python--with-conda--)
  * [**tl;dr** Virtual environment(v.env.) with conda:](#--tl-dr---virtual-environment-venv--with-conda-)
  * [Details:](#details-)
    + [Creation:](#creation-)



## **tl;dr** Virtual environment(v.env.) with conda:
This will create virtual environments inside your home folder (or any folder you wish) even without root access.
You can make different virtual environments for different projects so that the versions are not mixed up.
It will try to resolve all the package dependencies by itself.
Also, you an export the corresponding versions with your package so that others may use that list of version to create a similar virtual environment on their machine. You can also take their corresponding files and make virtual environment in your machine which resembles theirs.


Basic command to create virtual environment with conda (`miniconda` or `anaconda`):
```bash
conda create --name yourenv1
## or
conda create -n yourenv1
```

After this, you will possibly want to go into that v.env. using
```bash
source activate yourenv1
## or, depending on your machine,
conda activate yourenv1
```
You will probably see some changes in your prompt showing your current environment (i.e., not the base environment)

Now, to install a package(say `numpy`) in that specific v.env., you have to be inside that environment and issue the command:
```bash
conda install numpy			#(as latest as possible without conflict)

## or a specific version:
conda install numpy=1.11	#(Fuzzy, i.e., 1.11.0 or 1.11.1 or ... )
conda install numpy==1.11 	#(i.e., 1.11.0, to install 1.11.1, give numpy==1.11.1)

## Or newer/older than some specific version:
conda install "numpy>=1.8,<2"
```

To search for a package:
```bash
conda search scikit-learn

## Or, if you don't know the full name:
conda search "*scikit*"

## In a specific channel, say in 'conda-forge' (see later for channel):
conda search 'numpy[channel=conda-forge]'
```

To be out from the v.env., just write
```bash
source deactivate
## or depending on your machine,
conda deactivate
```






## Details:

### Creation:
You can specify python version while creating a v.env.:
```bash
conda create --name py34 python=3.4
```



And you can manage multiple versions of `python`. 

