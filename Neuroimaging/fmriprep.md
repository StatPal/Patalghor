In short, `fmriprep` is an fMRI **preprocessing tool** which uses AFNI, FSL (optionally freesurfer) and other tools for a unified setup. I will show you how to run it in a setup without any sudo access. 


To run it: 
```
export TMPDIR=/home/subrata/tmp_fmriprep_clean
export APPTAINERENV_TMPDIR=/home/subrata/tmp_fmriprep_clean
export TEMPLATEFLOW_HOME=/home/subrata/templateflow_cache
export APPTAINERENV_TEMPLATEFLOW_HOME=/home/subrata/templateflow_cache

nohup apptainer run --cleanenv --contain   -B $PWD:/data   -B /home/subrata/tmp_workdir_fmriprep:/work   -B /home/subrata/tmp_fmriprep_clean:/home/subrata/tmp_fmriprep_clean   -B  /home/subrata/templateflow_cache:/home/subrata/templateflow_cache   ../fmriprep_latest_sandbox   /data /data/derivatives participant --fs-no-reconall  --fs-license-file /data/license.txt --work-dir /work > fmriprep_all.log 2>&1 &
```

- If you want a few specific subjects, add something like `--participant-label 01 02`.
- If you do not want time taking `freesurfer` to run, run `--fs-no-reconall`


-------------------
Help links: 
- [usage](https://fmriprep.org/en/stable/usage.html)
- [Singularity container](https://fmriprep.org/en/1.5.9/singularity.html)
- [Output structure](https://fmriprep.org/en/1.5.9/outputs.html)
- [FAQ](https://fmriprep.org/en/1.5.9/faq.html)
- [External help for singularity container in HPC](https://docs.rcc.fsu.edu/software/fmriprep/#example-job-using-a-tutorial)
