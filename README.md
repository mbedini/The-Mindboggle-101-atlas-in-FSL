## The CerebrA atlas in FSL

Here we imported the recently released volumetric version of the Mindboggle 101 atlas (CerebrA; Manera et al. 2020) to utilize it within FSL and FSLeyes.
After importing the atlas in FSL, it will be possible to visualize it in FSLeyes and to use the atlasquery utility to interrogate it from the terminal.

This is not part of an official release so please use it with care and always refer to the [FSL wiki](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/Atlases) for an updated list of the available atlases.

The process of mapping the Mindboggle 101 atlas to the MNI152 nlin sym template is described in [Manera et al. 2020](https://www.nature.com/articles/s41597-020-0557-9).
The volumetric version of the Mindboggle 101 atlas was downloaded from the corresponding [MNI repository](http://nist.mni.mcgill.ca/icbm-152-nonlinear-atlases-2009/).

The Mindboggle 101 atlas has 51 labels per hemisphere, each associated with its discrete intensity value and label index (from 1 to 51 for the right hemisphere, and from 52 to 102 for the left hemisphere). The Talairach atlas in /usr/local/fsl/data/atlases was used as a template .xml file and modified to create the file associated with the Mindboggle 101 atlas following the instructions found in the [FSLeyes documentation](https://users.fmrib.ox.ac.uk/~paulmc/fsleyes/userdoc/latest/customising.html#customising).

The ordered list of Mindboggle 101 labels was retrieved from the CerebrA_LabelDetails.csv file.

#### Importing the atlas in FSL

We recommend dowloading the latest version of the files from TemplateFlow with Datalad:

```
$ datalad install -r ///templateflow $ cd templateflow/tpl-MNI152NLin2009cSym/ $ datalad get -r *
```

First, the atlas was reoriented to match the orientation that it's best liked by FSLeyes with the command:

```
$ fslreorient2std mni_icbm152_CerebrA_tal_nlin_sym_09c.nii mni_icbm152_CerebrA_tal_nlin_sym_09c.nii
```

Then the following command was used to get the center of gravity of each label (the -K pre-option allows to treat them as separate sub-masks):

```
$ fslstats -K mni_icbm152_CerebrA_tal_nlin_sym_09c.nii mni_icbm152_CerebrA_tal_nlin_sym_09c.nii -C >> CerebrA_COG.txt
```

These values were used to replace the list of labels and coordinates from the example template "Talairach.xml" file.

To import the atlas, download the file "CerebrA.xml" from this repository, and copy all the files in your FSL directory. For example, in a bash terminal, type (prefix all the commands with sudo if you don't have write permission, or switch to root with 'sudo -s'):

```
$ mkdir /usr/local/fsl/data/atlases/CerebrA
$ cp /your_downloads_path/mni_icbm152_CerebrA_tal_nlin_sym_09c.nii /usr/local/fsl/data/atlases/CerebrA/
$ cp /your_downloads_path/CerebrA.xml /usr/local/fsl/data/atlases/
```

#### Using the atlas in FSL and FSLeyes

In FSLeyes you can now either load the file, or click on Settings - Orto View 1 - Atlas panel (shortcut Ctrl+Alt+6) to open the atlas panel window, and tick the corresponding box ("CerebrA") to explore your data.
In a bash terminal, type the following commands to query a nifti mask or a single coordinate, and to retrieve the Mindboggle 101 label that corresponds to it:

```
$ atlasq ohi -a CerebrA -m your_mask.nii.gz
$ atlasq ohi -a CerebrA -c X,Y,Z
```

That's it! You can now analyze the relationship between your ROIs and the average sulcal/gyral morphology in MNI space using FSL.
For questions please send an email to marco.bedini@unitn.it.

Author and affiliation:
Marco Bedini,
Ph.D. student,
Center for Mind/Brain Sciences,
University of Trento.

#### References
- Manera, A. L., Dadar, M., Fonov, V., & Collins, D. L. (2020). CerebrA, registration and manual label correction of Mindboggle-101 atlas for MNI-ICBM152 template. *Scientific Data, 7*(1), 1-9. https://doi.org/10.1038/s41597-020-0557-9
