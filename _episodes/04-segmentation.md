---
title: "Segmentation"
teaching: 10
exercises: 5
questions:
- "How do we purge bad time periods and components to prepare a file for segmentation?"
- "How do we segment files that have been processed by the Lossless pipeline?"
objectives:
- "To learn how to purge and segment files that have been pre-processed by the Lossless pipeline."
keypoints:
- "FIXME"
---

The segmentation of EEG data files may involve many different procedures. This may include warping/interpolating to a desired montage, purging the marked bad data, rereferencing, renaming files based on group and condition, filtering, and/or resampling. In this episode of the tutorial, we will focus on the most important parts of writing a segmentation script, which is essentially to purge the manual marks from the data and to create segments using the remaining data that can later be used to generate ERPs and perform other post-processing procedures.

Typically, we would not segment the `*_ll.set` files from the output of the pipeline. Instead, it is preferable and highly recommended to perform a manual [quality check prodcedure](https://bucanl.github.io/SDC-LOSSLESS-QC/index.html) to ensure that you agree with the automated decisions made by the pipeline. Although the automated decisions of the pipeline are usually quite accurate in flagging time periods and components with artefacts, it does not guarantee that it will make the most ideal decisions, so reviewing the data manually is still a crucial step in EEG pre-processing in order to maximize the amount of cortical signal versus noise.

With that in mind, this tutorial will assume you have files that have been manually quality checked already. These files would be identical to the`*_ll.set` files, except some of the annotations would have been manually modified by an expert quality reviewer.

## Basic Segmentation

To create a simple segmentation script, there are a few required steps. The steps are as follows:

> ## Segmentation Steps
> 1. Purge/remove all manually marked time segments, channels, and components.
> 2. Warp/interpolate to a desired montage.
> 3. Segment the data time-locked to event markers, or at regular intervals in the case of resting data.
> {: .source}
{: .checklist}

## Purge the data

In order to exclude any channels, time periods, and components that have been flagged for removal, this bad data must be purged from the dataset before any segmentation is performed. Otherwise, we will be segmenting the original data, which defeats the purpose of running the files through the pipeline in the first place. To achieve this, we purge the data with manual flags using the function `pop_marks_select_data()`, with the **'remove'** argument set to **'on'**:

```matlab
EEG = pop_marks_select_data(EEG,'channel marks',[],'labels',{'manual'},'remove','on'); % purge channel marks
EEG = pop_marks_select_data(EEG,'time marks',[],'labels',{'manual'},'remove','on'); % purge time marks
EEG = pop_marks_select_data(EEG,'component marks',[],'labels',{'manual'},'remove','on'); % purge component marks
EEG = eeg_checkset(EEG);
```

## Warp and interpolate the data

Typically, we would warp and interpolate to the standard 10/20 head, or back to the original montage used for recording. At this stage we need to determine the appropriate transformation matrix that will warp the remaining electrodes to the surface of the desired montage, and then subsequently interpolate the missing channels. There are a number of functions currently available in EEGLAB that can achieve this, but we will focus on a couple of these functions here.

Determining the correct transformation matrix might be a bit challenging, but there are tools available to help us with this procedure. For the purposes of this tutorial, let's assume that we are interpolating to the 10/20 sites. One way to do this is to call the following function:

```matlab
EEG = warp_locs(EEG,'derivatives/lossless/code/misc/standard_1020_bucanl19.elc','transform',[0,0,0,0,0,0,1,1,1],'manual','on');
```

![Coregistration]({{ page.root }}/fig/coregister1.png "Coregistration")

Notice, we set the **'manual'** argument to **'on'**, as this will create a popup window that will make it easy to see the current state of the montage plotted against the montage we want to interpolate to. The goal is to make these two montages sit on the same surface. We can do this by adjusting the parameters of the transformation matrix. Each of the nine values represents the following types of transformation: [shiftX, shiftY, shiftZ, pitch, roll, yaw, scaleX, scaleY, scaleZ]. In most cases it will not be required to use trial and error to determine these values. Instead, we can use the **Warp Montage** button, which uses the known electrode correspondences between the two montages to determine the optimal transformation matrix. Once you click **Ok**, you should notice both montages are now sitting more or less on the same surface. You can make slight adjustments to any of the nine transformation values if you still think the coregistration could have worked a little bit better, but usually the automatically calculated transformation matrix will be sufficient. Once you have these values, you may then change the **'transform'** values in the `warp_locs()` function to the new automatically calculated values, and switch **'manual'** back to **'off'** so that the popup window doesn't appear anymore:

```matlab
EEG = warp_locs(EEG,'derivatives/lossless/code/misc/standard_1020_bucanl19.elc','transform',[1.254,0.06426,0.1043,0.002743,-0.01086,-1.57,1.011,1.011,0.9958],'manual','off');
```

This procedure only warps the current montage, but doesn't actually interpolate yet. Now that we know both montages are sitting on the same surface, interpolation can be done. To interpolate, we will use the `interp_mont()` function:

```matlab
EEG = interp_mont( EEG,'derivatives/lossless/code/misc/standard_1020_bucanl19.elc','nfids',0,'manual','off');
```

Depending on the montage file you are using, it may have the first few electrodes set as non-data fiducial channels to help with the warping stage. Most montages will either have no fiducials or three fiducials, but it is always good to check this because the interpolation will not work properly otherwise. Once the interpolation is completed, you should notice the first dimension of the EEG.data structure (representing the number of channels) to match the number of channels from the interpolated montage. This is one way to check that the interpolation worked as expected. 

## Segment the data

Once the interpolation has been done, we can finally create segments out of the remaining data. For task-based paradigms (as opposed to resting paradigms), we will time-lock to specific events and create segments of a desired length of time around these events. For resting paradigms, we would simply create segments at regular intervals in order to do perform spectral analyses later. In this tutorial we will only focus on segmenting task-based paradigms. As an example, we will segment our files to be 1000ms, from -200 to 800ms before and after the event marker for a particular condition, using the first 200ms as a baseline for baseline correction.

Considering we will likely want to create a separate segmented file for each condition, we first want to store the original EEG structure in a temporary vairable:

```matlab
tmpEEG = EEG;
```
Next, we will want to create a separate segmented file for each condition by identifying the event markers to time-lock to and creating epochs around them. Then, a baseline correction is performed between -200ms and 0ms, and the new segmented file is saved with a new name:

```matlab
%% segment condition_1
EEG = pop_epoch(tmpEEG,{'cond1a' 'cond1b'},[-.2 .8],'newname','condition_1', 'epochinfo', 'yes');
EEG = pop_rmbase(EEG,[-200 0]);
EEG = pop_saveset(EEG, 'filename',['[batch_dfn,_,-1]_condition1_seg.set']);

%% segment condition_2
EEG = pop_epoch(tmpEEG,{'cond2a' 'cond2b'},[-.2 .8],'newname','condition_2', 'epochinfo', 'yes');
EEG = pop_rmbase(EEG,[-200 0]);
EEG = pop_saveset(EEG, 'filename',['[batch_dfn,_,-1]_condition2_seg.set']);
```

**NOTE:** Notice that in the `pop_epoch()` function, we are using the tmpEEG variable we created above as the first argument, rather than the original EEG structure, because the EEG structure becomes the segmented version of the first condition, so for all subsequent conditions we would want to use the tmpEEG variable which still contains the original purged, unsegmented data.


{% include links.md %}
