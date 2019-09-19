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
- "The input file for the Lossless pipeline is `*_eeg.edf` and the output file is `*_ll.set`."
- "Remember to always pay attention to whether you are running a BASH command on your **local** machine versus the **remote** computer cluster."
---

## Copy files from local to remote

1. In a terminal window opened to your **local** drives, navigate to your **local** project directory:

    ```bash
    >> cd path/to/project/directory/Face13/
    ```

2. Copy the **local** `*.edf` files to the **remote** computer cluster, leaving these files in the BIDS standard (`sub-*/eeg/`). Again, replace [user_name] with your own username and [project_name] with 'Face13':

    ```bash
    >> rsync -rthvv --prune-empty-dirs --progress --include="*sub*" --include="*/" --exclude="*" --exclude="/*/*/*/*/" sub-* [user_name]@gra-dtn1.computecanada.ca:/home/[user_name]/scratch/[project_name]/
    ```

## Configuration file setup

1. Open MATLAB and navigate to your **local** project directory to make it your current path.

2. Open EEGLAB by typing the following into the command window:
 
    ```matlab
    >> addpath derivatives/BIDS-Lossless-EEG/code/install
    >> lossless_path
    >> eeglab
    ```

3. In the EEGLAB drop-down menu, navigate to **File->Batch->Context Configuration** and click `| Load context config |` to load a default configuration file that can then be modified and saved. The default configuration file is named `contextconfig.cfg` and is located in the `derivatives/BIDS-Lossless-EEG/code/config/` directory.

> ## Note
> Here, you will need to fill out the appropriate fields under Remote Locations, which will correspond to the remote paths for the project. For example, you would change the `remote_user_name` field to your user name on the remote system. The `remote_exec_host` field is the host and domain of the remote system. The `remote_project_archive` field is the remote path to the location of the archived root project directory (the main folder of the lossless pipeline) where you would like to archive your project folder. Finally, the `remote_project_work` directory is the remote path to the location of the work root project directory, where the actual jobs will be run, and the `remote dependency` is the same as the `remote_project_work` directory, but a few levels deeper (`derivatives/BIDS-Lossless-EEG/code/dependencies/`). For more info, see the Batch Context wiki about [Context configuration files](https://github.com/BUCANL/Batch-Context/wiki/Context-Configuration-Files).
>
> {: .source}
{: .callout}

   ![Context Config Window]({{ page.root }}/fig/contextconfig.png)

4. In the EEGLAB drop-down menu, navigate to **File->Batch->Batch Configuration** and click `| Get batch config file names |` to load the default batch configuration file(s) that can then be modified and saved. The configuration files we want to select for the Face13 data are in the `derivatives/BIDS-Lossless-EEG/code/config/face13_sbatch` folder. You want to select the seven files that are named 'c01-c05'. Once the files have been selected, click `| Clear/Load |` to load the files into the property grid.

   ![Batch Config Window]({{ page.root }}/fig/batchconfig.png)

5. Change the `submit_options` field in each batch configuration file to `--account=[group_name]`, where [group_name] is the name of your group on Graham. The rest of the parameters in batch configuration files are optimized for the Face13 tutorial dataset.

6. Once you are done editing the parameters, you can click `|Save as|` and select all of the files to save each of the files with their new parameters.

> ## Note 
> For most new projects, you might need to adjust some of the parameters here for running the pipeline. The most common changes for most other projects would be adjusting the `memory` and `time_limit` properties to optimize job runtimes. Once you are satisfied with all the parameters, you can click `| Save as |` to save each of the files with their new parameters, so that they can easily be loaded for future use. For more info, see the Batch Context wiki about [Batch configuration files](https://github.com/BUCANL/Batch-Context/wiki/Batch-Configuration-Files). 
>
> {: .source}
{: .callout}

## Submit jobs

1. In the EEGLAB drop-down menu, navigate to **File->Batch->Run history template batch**.

2. If your context configuration file is not already loaded, click `| Load context config |` and load the context configuration file you saved in step 3 above.

3. If your batch configuration files are not already loaded, click `| Load batch config |` and load all the batch configuration files from step 4 above.

4. Click `| History file |` and load all the scripts (s01-s05) corresponding to each of the batch configuration files (c01-c05). These scripts are located in the `derivatives/BIDS-Lossless-EEG/code/scripts/` directory.

5. Open up a terminal window, and navigate to your local project directory again:

    ```bash
    >> cd path/to/project/directory/Face13/
    ```

6. List all the data files you’d like to run through the pipeline. This can be done using the find command by typing:

    ```bash
    >> find sub-* -type f -name "*_eeg.edf"
    ```

7. This will print a list of all your initialized `*.edf` files, including the path, which you can then copy straight from the terminal into the **file** field in the Run history template batch window, with one path/filename per line. 

8. Click `| Ok |` and type your Graham password in the command window when prompted. You will have to enter your password several times. Your jobs should now start submitting for each data file, sequentially, one script at a time.

   ![Run History Template Batch Window]({{ page.root }}/fig/runhtb_lossless.png)

## Query running jobs

1. To check the status of your submitted jobs, go to a terminal window logged into Graham if you're still logged in or log back into Graham again:

    ```bash
    >> ssh [user_name]@graham.computecanada.ca
    ```

2. To list all currently queued and running jobs and check their status, type:	

    ```bash
    >> squeue -u [user_name]
    ```

3. To check if any particular file succeeded or failed running during a particular script, you may check the corresponding `*.log` file in the `derivatives/BIDS-Lossless-EEG/log/` folder. A stack trace of any error will be printed here. This file can be accessed using an in-terminal text editor such as Vim:


    ```bash
    >> vi derivatives/BIDS-Lossless-EEG/log/
    ```

## Copy files from remote to local

1. Once the files have successfully run through each stage of the pipeline, you should end up with an identical folder structure (`sub-*/ses-*/eeg/`) in your **remote** `derivatives/BIDS-Lossless-EEG/` folder for each of the data files that ran through the pipeline. You will notice many intermediary files in these folders, but the final output files will end in `*_ll.set` and `*_ll.fdt`.

2. To check if all files have in fact made it through the entire pipeline, you may locate these `*_ll.*` files using the find command, and seeing if there are any files missing:

    ```bash
    >> find derivatives/BIDS-Lossless-EEG/sub-* -type f -name "*_ll.*"
    ```

3. Now, you may copy these output files back to your **local** project directory. Make sure you are currently in your **local** project directory, if you aren’t already:

    ```bash
    >> cd path/to/project/directory/Face13/
    ```

4. Now, transfer the files using the following command in the terminal, replacing [user_name] with your own username and [project_name] with 'Face13':

    ```bash
    >> rsync -rthvv --prune-empty-dirs --progress --include="*.edf" --include="*icaweights.*" --include="*icasphere.*" --include="*_annotations*" --include="*/" --exclude="*" --exclude=”/*/*/*/*/” [user_name]@gra-dtn1.computecanada.ca:/scratch/[user_name]/[project_name]/derivatives/BIDS-Lossless-EEG/sub-* derivatives/BIDS-Lossless-EEG/
    ```

5. Once this procedure is completed, you should notice all the `*_ll.set` and `*_ll.fdt` files in your **local** `derivatives/BIDS-Lossless-EEG/` directory. These files can now be put through the [manual QC procedure](https://bucanl.github.io/SDC-LOSSLESS-QC/index.html) for further processing.


{% include links.md %}

---
