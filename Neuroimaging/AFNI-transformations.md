This file talks about very commonly used neuroimaging views, file formats, templates etc: 

+ File formats
  - **AFNI native format (.HEAD + .BRIK)**
  - **NIfTI (.nii, .nii.gz)**: single file format.
+ Views
  - `+orig`: Data in its native/original scanner space.
  - `+tlrc`: Data transformed into a standardized space (Talairach/MNI).
+ Space (stored in header metadata):
  - **ORIG Space**: Subject’s native anatomical space.
  - **TLRC Space**: Standardized space (Talairach-Tournoux or MNI).
  
  A dataset with `+orig` can still be **in TLRC space** if it has been transformed but not renamed. To check, use
  ```bash
  3dinfo -av_space dataset+tlrc
  ```

## **Converting Between Spaces**
- **Transform to TLRC (e.g., TT_N27 or MNI152 template)**:
```bash
@auto_tlrc -base TT_N27+tlrc -input anat+orig -no_ss
 ```

