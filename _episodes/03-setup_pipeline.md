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
- "Remember to always pay attention to whether you are running a BASH command on your **local** machine versus the **remote** computer cluster."
---

## Download/Setup the pipeline (local)

1. You will need the git package. You likely already have it natively installed with your system. If this is not the case, open up a terminal window, and type:

    ```bash
    >> sudo apt-get update
    >> sudo apt-get install git
    ```

2. In the terminal, navigate to the root of your project (this will be the Face13 folder):

    ```bash
    >> cd path/to/project/directory/Face13
    ```

3. If there is not already a derivatives folder within your project folder, create a derivatives folder:

    ```bash
    >> mkdir derivatives
    ```

4. Change directory into the derivatives folder:
    
    ```bash
    >> cd derivatives
    ```

5. Within the derivatives folder, clone/download the pipeline repository. **NOTE:** Use the recursive flag in order to clone all the required submodules:

    ```bash
    >> git clone --recursive https://github.com/BUCANL/BIDS-Lossless-EEG.git
    ```

## Download/Setup the pipeline (remote)

1. Open a new terminal window and log into Graham, replacing [user_name] with your Graham username:

    ```bash
    >> ssh [user_name]@graham.computecanada.ca
    ```

2. Navigate to the location where you want to download the pipeline onto the remote cluster:

    ```bash
	>> cd scratch/
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

1. Navigate to the folder that contains your octave packages:

    ```bash
    >> cd ~/octave
    ```

2. Remove folders if any of the following (IO, Signal, Struct, Control, Parallel) are located in your octave directory:

    ```bash
    >> rm -rf *
    ```

3. Navigate back to the root of your **remote** project folder, replacing [user_name] with your compute canada user name and [project_name] with `Face13`:

    ```bash
    >> cd /scratch/[user_name]/[project_name]
    ```

4. Run the remote setup script and follow the on screen prompts:

    ```bash
    >> bash derivatives/BIDS-Lossless-EEG/code/install/setup-remote.sh
    ```

{% include links.md %}

---
