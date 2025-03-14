This help file talks about plotting neuroimaging data and the obstacles often faced while doing that, more specifically, 
- [Using SUMA for surface-based plotting](#using-suma-for-surface-based-plotting)
- [R code to read and write nii file](#r-code-to-read-and-write-nii-file)
- [Other resources to consider](#other-resources-to-consider)


# Using SUMA for surface-based plotting:
SUMA does a surface-level analysis and plotting of a brain image in conjunction with AFNI, which is specifically very good for surface-level plotting. 

For quick display (mostly) of Talairach or MNI data:
* Use the already prepared [Talairach](http://afni.nimh.nih.gov/pub/dist/tgz/suma_TT_N27.tgz) or [MNI](http://afni.nimh.nih.gov/pub/dist/tgz/suma_MNI_N27.tgz) surfaces created from the N27 brain dataset using FreeSurfer.
* Ready to use, no surface creation or alignment needed (but match to your subject’s anatomy will not be very good)
* To make a specific surface for your subject, use @SUMA_Make_Spec_FS for FreeSurfer surfaces

The simplest code is:
```bash
# Go to the folder where the thing is
cd suma_demo/afni

# Start AFNI in a mode so that it can listen to connections from SUMA
afni -niml &
# underlay is DemoSubj_SurfVol_Alnd_Exp, overlay is DemoSubj_EccExpavir
# Both in orig view (try the original view, even the skull is not stripped)

suma -spec ~/CD/suma_demo/SurfData/SUMA/std.DemoSubj_both.spec -sv DemoSubj_SurfVol_Alnd_Exp+orig
# It can be `~/CD/suma_demo/` instead of `~/suma_demo` # Don't know why ../ does not work!
```
Then press `t` to communicate, and you'll see the results. Ideally, they should align exactly. But if you use a different spec than your anatomical image, it may be a little different. Be careful that it is not too much different or you have to use `@SUMA_Make_Spec_FS` or something equivalent. 

Working: 
 * Navigation: Up/down/left/right/ Left button drag
 * Separating hemisphere: Ctrl + left button drag; ctrl + double click to undo
 * Rotate in Z axis: Shift + left button drag; Shift + double click to undo
 * Translation: Shift + arrow
 * Zoom: Z and z
 * Record: `r` then left drah will increase/decrease brightness. Then Alt+Right Click to save
 * Record directly to disk: Ctrl + r
 * Background color change (ctrl + b not working): Go to `~/.sumarc` (or locate where that file is) and change something like `SUMA_BackgroundColor = 0.9,0.9,0.9` and then `suma -update_env`

See more [here](https://afni.nimh.nih.gov/pub/dist/doc/htmldoc/SUMA/Viewer.html)


Now, suppose we have a different directory, and the data to be plotted is in Talairach view. 
```bash
cd Research/Tensor/MDD
afni -niml &
# Put the Talairach view with underlay as TT_N27, overlay with all_cases.nii

Put the surface there, and please check the contours and the original/talairach view.
suma -spec ~/CD/suma_demo/SurfData/SUMA/std.DemoSubj_both.spec -sv DemoSubj_SurfVol+tlrc.HEAD &
```

SUMA can also use functions like `3dVol2Surf`, which gives a direct command line mapping from a 3d volume dataset directly into a cortical dataset (Check 43 page onwards of [this pdf](https://afni.nimh.nih.gov/pub/dist/edu/latest/suma/suma.pdf)). 
Check these later:
```
suma -spec ~/Downloads/suma_demo/SurfData/SUMA/std.DemoSubj_both.spec  -sv anat_final.sub-02 &
suma -spec ~/CD/suma_demo/SurfData/SUMA/std.DemoSubj_both.spec -sv DemoSubj_SurfVol+orig.BRIK &
```


# R code to read and write nii file:
The R code used to create the `all_cases.nii` file: (Now I can't really remember how the first all_case+tlrc.BRIK was created). 
```R
library(fmri)
filename <- paste0("all_case.nii")
abc <- RNifti::readNifti(filename)
tmp_all <- readRDS("/mnt/maitra-lab/subrata/MDD/Act_map_perm_group_analysis_added_all.rds")
dim(tmp_all)
summary(tmp_all[,,,,1:2])
dim(abc)

for(ii in 1:5){
  abc[,,,1,ii]     <- tmp_all[ii,,,,1]
  abc[,,,1,5 + ii] <- tmp_all[ii,,,,2]
}

RNifti::writeNifti(abc, "tmp2")
# system("3dcopy /home/subrata/Research/Tensor/MDD/tmp2.nii tmp3")
```
CHECK THE TT_N27 map already created using the aforementioned link instead of the file `DemoSubj_SurfVol+tlrc.HEAD` created by myself. 


# Other resources to consider:
Apart from the wonderful [Andy's book](https://andysbrainbook.readthedocs.io/en/latest/) and [Andy's brain blog](https://www.andysbrainblog.com/), there are many(!?) R packages to help neuroimaging plots. For example, mostly by [John Muschelli](https://github.com/muschellij2)
+ https://github.com/muschellij2/imaging_in_r/tree/gh-pages/pdfs
+ https://github.com/muschellij2/brainR/blob/master/man/write4D.file.Rd
+ https://cran.r-project.org/web/packages/fslr/

