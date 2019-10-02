---
title: "Co-registering EEG Data"
teaching: 20
exercises: 10
questions:
- "Why is co-registering the data important?"
- "How do I co-register EEG data?"
objectives:
- "Understand why co-registering data is important"
- "Understand how to co-register EEG data"
keypoints:
- "Co-registering "
---

## Introduction

- Co-registering is the process of aligning the recording montage with a standard montage

Co-registration of the data to the standard MNI head will occur during the pipeline in the s01_scalprat script. Prior to running the pipeline, the appropriate transformation matrix that will warp the recorded data to the standard montage needs to be determined. The transformation matrix that will be used for the study can be determined using the `ll_validate.m` script. 

This transformation matrix can then be input into [montage_info] in the batch configuration file c01.

The recorded data will be warped to the standard montage based off of a transformation matrix that is stated in [montage_info] in the batch configuration file c01. Before files are submitted to the pipeline, the transformation matrix that will be used for the study needs to be determined using the `ll_validate.m` script. 

## Co-registration Procedure 

1. Open MATLAB and navigate to your **local** project directory to make it your current path.

2. Open EEGLAB by typing the following into the command window:

    ```matlab
    >> addpath derivatives/BIDS-Lossless-EEG/code/install
    >> lossless_path
    >> eeglab
    ```

3. To determine the correct transformation matrix to co-register your data to a standard montage (the MNI head) run the `ll_validate.m` script in the MATLAB command window:

    ```matlab
    >> ll_validate
    ```
4. In the open file dialog window navigate to `sub-001/eeg/` and select the `sub-001_task-FaceFO_eeg.edf` file. You may need to change the `Files of Type` field to `All Files`. Determining the transformation matrix only needs to be done once per study and this process can be completed using any of the data files. 

5. A popup window that plots the recording montage and the standard montage will appear. The goal is to have the two montages sit on the same surface with corresponding electrodes as close to each other as possible. This can be done by adjusting the parameters of the transformation matrix that are located at the bottom of the window.

        ![CoregistrationMontage]({{ page.root }}/fig/rotated_coreg.png)

Hints: 
- the recording montage is on a very different scale than the standard montage- this can be adjusted by putting large numbers in the Resize(x), Resize(y), and Resize(z) fields.
- The nose direction of the montages differs. This can be corrected by putting a negative number in the Yaw(rad) field (-1.58).
- Once the montages are the same size and facing the same direction, use the Move right, Move front, and Move up fields to adjust where the montages are in space relative to each other
- Trying to get the montages to exactly overlap
- Look at the fiducials (LPA, Nz, RPA) to see how well they are lining up 
- The head that is shown with the montages can be toggled by pressing the `Mesh on/off` button

> ## Solution
> ![WarpedMontage]({{ page.root }}/fig/aligned_montageon.png)
> [0.500 -22.000 -48.000 -0.065 0.000 -1.580 1060.000 1260.000 1220.000]
>
> {: .output}
{: .solution}

6. When you are satisfied with the alignment between the two montages, select the `Ok` button.

7. In the MATLAB command window, it will print: [montage_info] for c01_scalpart.cfg followed by the transformation matrix (nine numbers). Copy and paste these values into the c01_scalpart_remote.cfg file located in `derivatives/BIDS-Lossless-EEG/code/config/face13_sbatch`. These numbers need to be within square brackets. 

        ![TransformationMatrix]({{ page.root }}/fig/highlighted_transformmatrix.png)

8. In the EEGLAB drop-down menu, navigate to **File** and select `Clear study/ Clear all` to remove the dataset from memory.

{% include links.md %}

