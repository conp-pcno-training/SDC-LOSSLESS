---
title: "Running the Lossless Pipeline"
teaching: 45
exercises: 15
questions:
- "How do we run a batch of EEG files through the Lossless pipeline?"
- "How do we submit Lossless pipeline jobs to a remote parallel computing cluster?"
objectives:
- "Submitting a batch of EEG files to run remotely through the Lossless pipeline."
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

### Download/Setup the pipeline (local)

- **Linux and Mac users**

1. You will need the git package. You likely already have it natively installed with your system. If this is not the case, open up a terminal window, and type:
	
    `>> sudo apt-get update`  
    `>> sudo apt-get install git`  

2. In the terminal, navigate to the location where you want to download the pipeline:

    `>> cd path/to/project/directory`

3. Clone/download the pipeline repository into the the above directory. NOTE: Use the recursive flag in order to clone all the required submodules:

    `>> git clone --recursive https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg.git`

4. Optional: Rename the pipeline directory to your desired project name (we will call it face_13 here):

    `>> mv bids_lossless_eeg face_13`

5. Copy the downloaded `sub-*` folders and files from the Setup procedure into the root face_13 directory.

- **Windows users**

1. Go to the [bids_lossless_eeg][bids_lossless_eeg] Gitlab repository, click the **Download ZIP** icon, and extract the contents into a desired location on your local drive.

2. Optional: Rename the pipeline directory to your desired project name (we will call it face_13 here).

3. Copy the downloaded `sub-*` folders and files from the Setup procedure into the root face_13 directory.

### Download/Setup the pipeline (remote)

1. Open a new terminal window (or Powershell in Windows) and log in to Graham, replacing [user_name] with your Graham username:

    `>> ssh [user_name]@graham.computecanada.ca`

2. Navigate to the location where you want to download the pipeline onto the remote cluster, again replacing [user_name] with your Graham username, and [group_name] with the name of your group:

	`>> cd /project/[group_name]/[user_name]`

3. Clone/download the pipeline repository into the the above directory. NOTE: Use the recursive flag in order to clone all the required submodules:

    `>> git clone --recursive https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg.git`

4. Optional: Rename the pipeline directory to your desired project name (we will call it face_13 here):

    `>> mv bids_lossless_eeg face_13`

### Remote setup

1. Navigate to the code folder within your newly downloaded remote project directory:

    `>> cd face_13/code`

2. In case the setup-remote.sh script in this folder is not executable, run the following line in the bash terminal:

    `>> chmod +x setup-remote.sh`

3. Now, you can run the script by simply typing the following line into the terminal, and following the on-screen instructions.

    `>> ./setup-remote.sh`

**NOTE:** On Graham, when the prompts ask to enter the initialization filenames for Amica and Octave, you may simply use the defaults by hitting **Enter** after each prompt.

### Staging Script

1. Write a staging script to add time marks for out-of-task time and in-task time, as well as any other study-wide criteria, warping to the standard montage, filtering, and other possible processes to normalize the dataset before proceeding through the pipeline. A sample staging script has been written for the face_13 dataset (face13_staging.m), and is already located in the derivatives/lossless/code/scripts/ folder. If you need to write a new staging script, you can name it staging_script.m and save it in the local derivatives/lossless/code/scripts/ folder. This script will then need to be copied over to the remote end, described below.

### Copy files from local to remote

1. In a terminal window opened to your local drives (NOT a terminal logged into Graham), navigate to your local project directory:

    `>> cd path/to/project/directory/face_13`

2. Copy the local `*.set` and `*.fdt` files to the remote computer cluster, leaving these files in the BIDS folder structure (`sub-*/ses-*/eeg/`). Again, replace [user_name] with your own username, and [group_name] with the name of your group:

    `>> rsync -rthvv --prune-empty-dirs --progress --include="*_eeg.*" --include="*/" --exclude="*" --exclude="/*/*/*/*/" sub-*`  
    `[user_name]@gra-dtn1.computecanada.ca:/home/[user_name]/projects	/[group_name]/[user_name]/face_13/`  

3. If a new staging_script was created above, this will also need to be copied from the local to the remote end:	

    `>> rsync -rthvv --progress derivatives/lossless/code/scripts/staging_script.m`  
    `[user_name]@gra-dtn1.computecanada.ca:/home/[user_name]/projects/[group_name]/[user_name]/face_13/derivatives/lossless/code/scripts/`  

### Configuration file setup

1. Open MATLAB (tested on 2012b and later versions) and navigate to your local project directory to make it your current path.

2. Open EEGLAB by typing the following into the console window:
 
    `>> addpath code`  
    `>> lossless_path`  
    `>> eeglab`  

3. In the EEGLAB drop-down menu, navigate to File->Batch->Context Configuration and click **Load context config** to load a default configuration file that can then be modified and saved. Here, you will need to fill out the appropriate fields under Remote Locations, which will correspond to the remote paths for the project. For more info, see the Batch Context [wiki](https://github.com/BUCANL/Batch-Context/wiki/Context-Configuration-Files) about Context configuration files.

4. In the EEGLAB drop-down menu, navigate to File->Batch->Batch Configuration and click **Get batch config file names** to load the default batch configuration file(s) that can then be modified and saved. The default pipeline configuration files are located in `derivatives/lossless/code/config/remote_sbatch/`, and they are the files that begin with a ‘c’ (c01-c05). The configuration files we want to select for the face_13 data are in the face13_sbatch folder. Once the files have been selected, click **Clear/Load** to load the files into the property grid.

**NOTE:** For most new projects, you might need to adjust some of the parameters here for running the pipeline. For the face13 data, you can leave the config files as they are, except change the submit_options field in each config file to `--account=[group_name]`, where [group_name] is the name of your group on Graham. The most common changes for most other projects would be adjusting the memory and time_limit properties to optimize job runtimes. Once you are satisfied with all the parameters, you may click **Save as** to save each of the files with their new parameters, so that they can easily be loaded for future use. For more info, see the Batch Context [wiki](https://github.com/BUCANL/Batch-Context/wiki/Batch-Configuration-Files) about Batch configuration files. 

### Submit jobs

1. In the EEGLAB drop-down menu, navigate to File->Batch->Run history template batch.

2. If your context configuration file is not already loaded, click **Load context config** and load the context configuration file you saved in step 8c (FIXME).

3. Click **Load batch config** and load all the batch configuration files from step 4 above.

Click **History file** and load all the scripts corresponding to each of the batch configuration files (s01-s05). These scripts are located in the `derivatives/lossless/code/scripts/` directory.

4. Open up a terminal window, and navigate to your local project directory again:

    `>> cd path/to/project/directory/face_13`

5. List all the data files you’d like to run through the pipeline. This can be done using the find command. If using the BIDS directory structure, simply type:

    `>> find sub-* -type f -name "*_eeg.set"`

6. This will print a list of all your initialized *.set files, including the path, which you can then copy straight from the terminal into the file field in the Run history template batch window, with one path/filename per line.

7. Finally, in the drop down menu at the bottom of the Run history template batch window, select the ssh2 option, to avoid having to enter your password several times upon job submission.

8. Click **Ok** and type your Graham password into the window that appears. Your jobs should now start submitting for each file, sequentially, one script at a time.

### Query running jobs

1. To check the status of your submitted jobs, you need to log into Graham again:

    `>> ssh [user_name]@graham.computecanada.ca`

2. To list all currently queued and running jobs and check their status, type:	

    `>> squeue -u [user_name]`

3. To check if any particular file succeeded or failed running during a particular script, you may check the corresponding .log file in the derivatives/lossless/log folder. A stack trace of any error will be printed here. This file can be accessed using an in-terminal text editor such as Vim:

    `>> vi derivatives/lossless/log/s0*/*.log`

### Copy files from remote to local

1. Once the files have successfully run through each stage of the pipeline, you should end up with an identical folder structure (sub-*/ses-*/eeg/) in your derivatives/lossless/ folder for each of the data files that ran through the pipeline. You will notice many intermediary files in these folders, but the final output files will end in *_ll.set and *_ll.fdt.

2. To check if all files have in fact made it through the entire pipeline, you may locate these *_ll.* files using the find command, and seeing if there are any files missing:

    `>> find derivatives/lossless/sub-* -type f -name "*_ll.*"`

3. Now, you may copy these output files back to your local project directory. Make sure you are currently in your local (NOT remote) project directory, if you aren’t already:

    `>> cd path/to/project/directory/face_13`

4. Now, transfer the files using the following command in the terminal:

    `>> rsync -rthvv --prune-empty-dirs --progress --include="*_ll*" --include="*/" --exclude="*" --exclude=”/*/*/*/*/”`  
    `[user_name]@gra-dtn1.computecanada.ca:/home/[user_name]/projects/[group_name]/[user_name]/face_13/derivatives/lossless/sub-* derivatives/lossless/`

5. Once this procedure is completed, you should notice all the *_ll.* files in your local derivatives/lossless/ directory. These files can now be put through the manual QC procedure for further processing.


{% include links.md %}

