---
title: "Running the Lossless Pipeline"
teaching: 35
exercises: 10
questions:
- "How do we run a batch of EEG files through the Lossless pipeline?"
- "How do we submit Lossless pipeline jobs to a remote parallel computing cluster?"
objectives:
- "Submitting a batch of EEG files to run remotely through the Lossless pipeline."
keypoints:
- "Ensure you have all the pipeline files and your data files on both the **local** and **remote** machine."
- "The input file for the Lossless pipeline is `*_eeg.edf` and the output files are `*.edf`, `*icasphere.tsv`, `*icaweights.tsv`, `*dipole.mat`,  `*_annotations.mat`, `*_annotations.tsv`, and `*_annotations.json`."
- "Remember to always pay attention to whether you are running a bash command on your **local** machine versus the **remote** computer cluster."
---

## Copy files from local to remote

1. In a terminal window opened to your **local** drives, navigate to your **local** project directory:

    ```bash
    >> cd path/to/project/directory/Face13/
    ```

2. Copy the **local** subject folders to the **remote** computer cluster, leaving these files in BIDS format (`sub-*/eeg/`). Again, replace [user_name] with your own username and [project_name] with your project name (the project name for the tutorial is 'Face13'):

    ```bash
    >> rsync -rthvv --prune-empty-dirs --progress --include="*sub*" --include="*/" --exclude="*" --exclude="/*/*/*/*/" sub-* [user_name]@gra-dtn1.computecanada.ca:/scratch/[user_name]/[project_name]/
    ```
    > ## Note 
    > Windows does not have support for rsync. Use the following command instead:     
    >
    > ```bash
    > >> scp -r sub-* [user_name]@gra-dtn1.computecanada.ca:/scratch/[user_name]/[project_name]/
    > ```
    > {: .source}
    {: .callout}

## Configuration file setup

1. Open MATLAB and navigate to your **local** project directory to make it your current path.

2. Open EEGLAB by typing the following into the command window:
 
    ```matlab
    >> addpath derivatives/BIDS-Lossless-EEG/code/install
    >> lossless_path
    >> eeglab
    ```

3. In the EEGLAB drop-down menu, navigate to **File->Batch->Context Configuration** and click `| Load context config |` to load a default configuration file that can then be modified and saved. To load the deafult context configuration file, navigate to the `derivatives/BIDS-Lossless-EEG/code/config/` directory and select the file named `contextconfig.cfg`. 

    ![Context Config Window]({{ page.root }}/fig/contextconfig.png)

4. Modify the appropriate fields under Remote Locations, which will correspond to the remote paths for the project. Change the `remote_user_name` field to you user name on the remote system. The `[remote_project_archive]` field can be left as the default. In the `remote_project_work` and `remote_dependency` fields, change the  `[user_name]` and `[project_name]` to your remote user name and your project name. Once you have finished modifying the context configuration file, select `| Save as |` and navigate to the `derivatives/BIDS-Lossless-EEG/code/config/` directory and save the configuration file here. It is recommended to save the context configuration file as `contextconfig_[user_name]_[project_name].cfg`. 

    > ## Note
    > The `remote_exec_host` field is the host and domain of the remote system. The `remote_project_archive` field is the remote path to the archieve location for where you would like to store processed files long term. On Graham, typically this is the [project file system](https://docs.computecanada.ca/wiki/Project_layout). Finally, the `remote_project_work` directory is the remote path to the location of the work root project directory, where the actual jobs will be run, and the `remote dependency` is the same as the `remote_project_work` directory, but a few levels deeper (`derivatives/BIDS-Lossless-EEG/code/dependencies/`). For more info, see the Batch Context wiki about [Context configuration files](https://github.com/BUCANL/Batch-Context/wiki/Context-Configuration-Files).
    >
    > {: .source}
    {: .callout}

5. In the EEGLAB drop-down menu, navigate to **File->Batch->Batch Configuration** and click `| Get batch config file names |` to load the default batch configuration file(s) that can then be modified and saved. The configuration files we want to edit for the Face13 data are named `c01-c05` and are located in the `derivatives/BIDS-Lossless-EEG/code/config/face13_sbatch` folder. It is recommended that you load, edit and save the configutation files **one at a time**. 

6. Once you have a file selected, click the `| Clear/Load |` button to load the file into the property grid.

   ![Batch Config Window]({{ page.root }}/fig/batchconfig.png)

7. Change the `submit_options` field in each batch configuration file to `--account=[group_name]`, where [group_name] is the name of your allocation group on Graham. The rest of the parameters in batch configuration files are optimized for the Face13 tutorial dataset.

8. Once you are done editing the parameters, you can click `|Save as|` and navigate back to the `derivatives/BIDS-Lossless-EEG/code/config/face13_sbatch` folder and save the configuration file with the same name as before. 

    > ## Note
    > If you have loaded and edited multiple configuration files at once, after you click `| Save as |`, you **must** select all of the files and iteratively save each file.
    >
    >{: .source}
    {: .callout}

9. Click the `| Clear/New |` button and repeat the process for all seven of the batch configuration files (c01-c05).

> ## Common Errors
> An easy way to check for errors is to compare your newly created files against the default Face13 configurations on the [BUCANL Github](https://github.com/BUCANL/BIDS-Lossless-EEG/tree/master/code/config/face13_sbatch). In particular, the `order` subfield of each config file should remain exactly the same as the corresponding default file. If these have changed, you have most likely overwritten some of your config files. In this case, you will have to update these files and go through the editing process again. To update the files, you can copy and paste directly from the Github page to the configuration file in your project. 
>
>{: .source}
{: .discussion}

## Submit jobs

1. In the EEGLAB drop-down menu, navigate to **File->Batch->Run history template batch**.

2. If your context configuration file is not already loaded, click `| Load context config |` and load the context configuration file you saved in step 3 above.

3. If your batch configuration files are not already loaded, click `| Load batch config |` and then click `| Batch configuration file |` on the pop-up window. The configuration files we want to select for the Face13 data are in the `derivatives/BIDS-Lossless-EEG/code/config/face13_sbatch` folder. You want to select the seven files that are named 'c01-c05'.

4. Click `| History file |` and load all the scripts (s01-s05) corresponding to each of the batch configuration files (c01-c05). These scripts are located in the `derivatives/BIDS-Lossless-EEG/code/scripts/` directory.

5. Open up a terminal window, and navigate to your local project directory again:

    ```bash
    >> cd path/to/project/directory/Face13/
    ```

6. List all the data files you’d like to run through the pipeline. This can be done using the find command by typing:

    ```bash
    >> find sub-* -type f -name "*_eeg.edf"
    ```

7. This will print a list of all your initialized `*.edf` files, including the path, which you can then copy straight from the terminal into the **file** field in the run history template batch window, with one path/filename per line. 

8. Click `| Ok |` and type your Graham password in the command window when prompted. You will have to enter your password several times, unless you have [ssh keys](https://docs.computecanada.ca/wiki/Using_SSH_keys_in_Linux) set up. Your jobs should now start submitting for each data file, sequentially, one script at a time.

   ![Run History Template Batch Window]({{ page.root }}/fig/runhtblossless.png)

## Query running jobs

1. To check the status of your submitted jobs, go to a terminal window and log into Graham again:

    ```bash
    >> ssh [user_name]@graham.computecanada.ca
    ```

2. To list all currently queued and running jobs and check their status, type:	

    ```bash
    >> squeue -u [user_name]
    ```

3. To check if any particular file succeeded or failed running during a particular script, you may check the corresponding `*.log` file in the `derivatives/BIDS-Lossless-EEG/log/` folder. A stack trace of any error will be printed here.

## Copy files from remote to local

1. Once the files have successfully run through each stage of the pipeline, you should end up with an identical folder structure (`sub-*/eeg/`) in your **remote** `derivatives/BIDS-Lossless-EEG/` folder for each of the data files that ran through the pipeline. There will be intermediary files and several final output files for each participant. The final output files are `*.edf`, `*icasphere.tsv`, `*icaweights.tsv`, `*dipole.mat`, `*_annotations.mat`, `*_annotations.tsv`, `*_annotations.json`, and `*_iclabel.mat`. 

2. To check if all files have in fact made it through the entire pipeline, you may locate these `*.edf*` files using the find command, and seeing if there are any files missing:

    ```bash
    >> find derivatives/BIDS-Lossless-EEG/sub-* -type f -name "*.edf*"
    ```
    > ## Note 
	> Files that are missing from this find command did not make it all the way through the pipeline. The log files for the job submission can be investigated to determine why a file did not successfully complete. These log files are located in 'derivatives/BIDS-Lossless-EEG/log/'.   	 
	>
	> {: .source}
	{: .callout}

3. Now, you may copy these output files back to your **local** project directory. Make sure you are currently in your **local** project directory, if you aren’t already:

    ```bash
    >> cd path/to/project/directory/Face13/
    ```

4. Now, transfer the files using the following command in the terminal, replacing [user_name] with your own username and [project_name] with 'Face13':

    ```bash
    >> rsync -rthvv --prune-empty-dirs --progress --include="*dipole.mat" --include="*iclabel.mat" --include="*.edf" --include="*icaweights.*" --include="*icasphere.*" --include="*_annotations*" --include="*/" --exclude="*" --exclude="/*/*/*/*/" [user_name]@gra-dtn1.computecanada.ca:/scratch/[user_name]/[project_name]/derivatives/BIDS-Lossless-EEG/sub-* derivatives/BIDS-Lossless-EEG/
    ```

    > ## Note 
    > Windows does not have support for rsync. Use the below command instead. Note that more files will be downloaded as a result of scp grabbing **all** files in the subject folders.
    >
    > ```bash
    > >> scp -r [user_name]@gra-dtn1.computecanada.ca:/scratch/[user_name]/[project_name]/derivatives/BIDS-Lossless-EEG/sub-* derivatives/BIDS-Lossless-EEG/
    > ```
    > {: .source}
    {: .callout}


5. Once this procedure is completed, you should notice all the output files in your **local** `derivatives/BIDS-Lossless-EEG/` directory. These files can now be put through the [QC procedure](https://bucanl.github.io/SDC-LOSSLESS-QC/index.html) for further processing.


{% include links.md %}

---
