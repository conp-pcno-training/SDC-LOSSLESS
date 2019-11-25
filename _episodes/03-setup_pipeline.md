---
title: "Downloading the Lossless Pipeline"
teaching: 35
exercises: 10
questions:
- "How do we download and set up the Lossless pipeline?"
objectives:
- "Setting up the Lossless pipeline to prepare for submitting jobs."
keypoints:
- "Ensure the pipeline is set up on both the **local** and **remote** machine."
- "Remember to always pay attention to whether you are running a bash command on your **local** machine versus the **remote** computer cluster."
---

## Download/Setup the pipeline (local)

1. In the terminal, navigate to the root of your project (this will be the Face13 folder):

    ```bash
    >> cd path/to/project/directory/Face13
    ```

2. If there is not already a derivatives folder within your project folder, create a derivatives folder:

    ```bash
    >> mkdir derivatives
    ```

3. Change directory into the derivatives folder:
    
    ```bash
    >> cd derivatives
    ```

4. Within the derivatives folder, clone/download the pipeline repository. **NOTE:** Use the recursive flag in order to clone all the required submodules:

    ```bash
    >> git clone --recursive https://github.com/BUCANL/BIDS-Lossless-EEG.git
    ```

## Download/Setup the pipeline (remote)

1. Open a new terminal window and log into Graham, replacing [user_name] with your Graham username:

    ```bash
    >> ssh [user_name]@graham.computecanada.ca
    ```

2. Navigate to the location where you want to download the pipeline onto the remote cluster, replacing [user_name] with your Graham username:

    ```bash
	>> cd /scratch/[user_name]
    ```

3. Create a project folder (we will call it 'Face13' here):

    ```bash
    >> mkdir Face13
    ```

4. Change directory into the Face13 folder:

    ```bash
    >> cd Face13
    ```

5. In the Face13 folder, create a derivatives folder: 

    ```bash
    >> mkdir derivatives
    ```

6. Change directory into the derivatives folder:

    ```bash
    >> cd derivatives
    ```

7. Within the derivatives folder, clone/download the pipeline repository. **NOTE:** Use the recursive flag in order to clone all the required submodules:

    ```bash
    >> git clone --recursive https://github.com/BUCANL/BIDS-Lossless-EEG.git
    ```

## Remote setup

If you are starting a new study, this process will need to be repeated. However, the Octave package installation will typically proceed much quicker if the new study is being run on the same account. 

> ## Note 
> If the `setup-remote.sh` procedure fails, or needs to be rerun for any reason, the **lock** files must be removed. These files prevent portions of the `setup-remote.sh` procedure from being run. An important distinction is that these files are created upon success **or** failure. This means that if something has gone wrong during the process, the lock files will need to be removed or `setup-remote.sh` will skip that section of the procedure. Below are the bash commands to remove the four lock files that are created. These commands are meant to be run from the project root on the remote.
>
> ```bash
> >> rm derivatives/BIDS-Lossless-EEG/code/misc/locks/amica-make.lock
> >> rm derivatives/BIDS-Lossless-EEG/code/misc/locks/executable-files.lock
> >> rm derivatives/BIDS-Lossless-EEG/code/misc/locks/fieldtrip-made.lock
> >> rm derivatives/BIDS-Lossless-EEG/code/misc/locks/octave-pkgs.lock
> ```
>
>{: .source}
{: .callout}

1. Navigate to the folder that contains your octave packages:

    ```bash
    >> cd ~/octave
    ```

2. Check if any of the following are in your octave directory: IO, Signal, Struct, Control, Parallel. Remove these directories if they are present.

3. Navigate back to the root of your **remote** project folder, replacing [user_name] with your own username and [project_name] with your project name (the project name for the tutorial is `Face13`):

    ```bash
    >> cd /scratch/[user_name]/[project_name]
    ```

4. Run the remote setup script and follow the on screen prompts:

    ```bash
    >> bash derivatives/BIDS-Lossless-EEG/code/install/setup-remote.sh
    ```

{% include links.md %}

---
