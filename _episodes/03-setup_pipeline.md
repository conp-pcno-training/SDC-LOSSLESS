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

2. In the terminal, navigate to the root of your project (this will be the Face_13 folder):

    ```bash
    >> cd path/to/project/directory/Face_13
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

1. Open a new terminal window and log in to Graham, replacing [user_name] with your Graham username:

    ```bash
    >> ssh [user_name]@graham.computecanada.ca
    ```

2. Navigate to the location where you want to download the pipeline onto the remote cluster, again replacing [user_name] with your Graham username, and [group_name] with the name of your group:

    ```bash
	>> cd /project/[group_name]/[user_name]/
    ```

3. Create a project folder (we will call it 'Face_13' here):

    ```bash
    >> mkdir Face_13
    ```

4. Change directory into the Face_13 folder:

    ```bash
    >> cd Face_13
    ```

5. In the Face_13 folder, create a derivatives folder: 

    ```bash
    >> mkdir derivatives
    ```

6. Within the derivatives folder, clone/download the pipeline repository. **NOTE:** Use the recursive flag in order to clone all the required submodules:

    ```bash
    >> git clone --recursive https://github.com/BUCANL/BIDS-Lossless-EEG.git
    ```

## Remote setup

1. Navigate back to the root of your **remote** project folder:

    ```bash
    >> cd ..
    ```

2. Run the remote setup script and follow the on screen prompts:

    ```bash
    >> bash derivatives/BIDS-Lossless-EEG/code/install/setup-remote.sh
    ```

{% include links.md %}

---
