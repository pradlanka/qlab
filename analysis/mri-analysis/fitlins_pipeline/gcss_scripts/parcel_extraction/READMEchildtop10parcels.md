# Top Percent of Voxel Analysis for Language Localizer data ASD vs TD: Top 10% parcels, Child Version, Language Localizer

Greetings! This uses Dr. Terri Scott's run_froi_resp_mag.m codeto isolate the cumulative values of the top x% of thvoxels for each given subject restricted based on each given parcel based on a dataset of individual subject fMRI volumes. 

This code requires Freesurfer and Matlab.


To Run:
0) Prior to running, you need to have all the language localizer higher level analysis done. Zstat files should be retrived from subject_flolder > language localizer gfeat file > cope 1 > stats > zstat.nii.gz file for the participant. This file should be copied over for each participant into either the data > langloc_child > TD > zstats folder for TD children or the  data > langloc_child > ASD > zstats folder for ASD children. They should be renamed as 'x_zstat.nii.gz', with x being the relative number of the participant to the rest of the group - for instance, if you have 18 TD children with higher level language localizer data, you would rename the ztstat for the loweest number of these '1_zstat.nii.gz', then the second lowest '2_zstat.gz', etc up to the highest at '18_zstat.gz.' This would be done seperately for the TD and ASD goup, so each would have a 1_ztstat.nii.gz', a '2_ztat.nii.gz' etc.Similarly, the corresponding cope fies for each child should be retrieved from the same source folder and copied over into the data > langloc_child > TD > copes folder for TD children or the data > langloc_child > ASD > copes folder for ASD children. The same naming conventions should be followed and care should be taken to making sure that a participant's cope an zstat file correspond numerically. Ensure that the actual IDs (ex: blast_c_475) are recorded in the same order in a text file that can be easily found within the project directory; a seperate file for TD and ASD children should be made, and each file shuld have one ID per line.
Note: the standard for this prohram is to get the top 10% ov voxels in each parcel. If a different percentage is desired, tenpct_n_voxels on approximatel line 80 should be altered so that tenpct_n_voxels = round(n_voxels*0.10) is changed to tenpct_n_voxels = round(n_voxels*x) where x is teh decimal form of the desired percentage. 
Note: In many cases, the dimensions of the language localizor masks may not correspondto the dimensions of the child's zstat and cope data. To fix this, the language localizar data should be resampled. To do so:
A) cd into the folder containing the masks you wish to resample. 
B) For each mask that is to be resampled, run 3dresample -input mask_to_resample.file_type -master zstat_file_to_sample_from.file_type -prefix resampled_mask.file_type. If one of these files isn't or should not be in the current folder, set the file's file path as part of teh file name. An example of this is:
3dresample -input seg_parcels/RightMidPostTemp.nii -master /Volumes/data/projects/blast/data/derivatives/fitlins/analyzed/fitlins_alice/sub-blasta012/level-run_name-runlevel_sub-blasta012_run-1_contrast-intactGtDegraded_stat-z_statmap.nii.gz -prefix seg_parcels_resampled_lang_loc/resampled-RightMidPostTemp.nii.gz
C) If the resampled files and the origional files were saved to teh same folder, either move one set or delete the origionals from that folder. 
1) open run_froi_resp_mag_fedorenko_lh.m. Set PROJECT_DIR to the path where this code and all your subfolders you'll need to reference are located. Set PARCEL_DIR to the path within the project directory where the individual parcels you wish to use as maps are located, SUBJ_NAME_LIST to the path ending with the name of the file within the project directory where the actual ID of the subjects and not just their reassigned IDs from step 0 are located, SUBJ_DEFINE_DATA_DIR to the path within the project directory where the desired group's zstat files are located, SUBJ_MEASURE_DATA_DIR to the path within the project directory where the desired group's cope files are located, RESULTS_DIR to the path within the project directory where the resulting csv files should be saved, COND to the name of the contrast being analyzed SUBJ_IDS to range from 1 to the highest ID value of the input subjects (fir instance, if there are three subjects, this should be a 3 to corrospond to 3_cope.nii.gz), VOL_VALS to the number of parcels being input for analysis, and the filepath in writetables (~ line 66) to the desired output filepath for the anaysis ending in the desired file name
Note: If you get an error such as 'Struct contents reference from a non-struct array object.' after the gunzip commands are displayed, this likely means that either the file paths set earlier or the file names in DEFINE_VOL or MEASURE_VOL are not correct. Alternatively, the range set in SUBJ_IDS may be too large. One of these is likely the issue with any gunzip errors that may arise.
Note: If you get an error such as Index exceeds matrix dimensions, this likely means that the volumes of your participant cope/zstat files and the volumes of your parcels are not in alignment.To fix, see the resampling instructions of step 0 to resample your parcels to the appropiate dimensions. 
2) Open the resulting CSV file and change the percel names if desired; parcel names will be the name of the given parcel's file with the .nii and .gz removed, so if these names are not as desired, they should be altered. These parcels should be in teh same order as in the origional folder input, so that can be used as a reference. 
3) To graph the output results, open plotting_lang_loc_top_10.R. Ensure that the file path and file names are as desired, and that graph features and layout are as desired as well. Run the code. The t-test comparison, controlled for multiple comparisons, can be found printed in teh program itself, and the resulting graph of the data will be saved to the current working directory,