---
title: Setup
---

### Download and install MATLAB

- You will need a fairly recent version of MATLAB (procedure tested on MATLAB 2012b and newer versions). For detailed installation instructions, click [here](https://www.mathworks.com/help/compiler/install-the-matlab-runtime.html).

### Download the git package

- This tutorial requires the git package. You likely already have it natively installed with your system. To check if you have git, open a terminal window and type:

    ```bash
    >> git --version
    ```

- If you do not have git, contact your system administrator for installation help.

### Download the Face13 sample dataset

- The Face13 tutorial dataset can be downloaded from this [google drive](https://drive.google.com/drive/folders/1xq85woDpAYXhCtzdgjkXpjjjggiWSKtc). Download the `sourcedata` folder from the drive. This folder contains the raw EEG data, standard montages that are used during initialization, and task information.

- To run through this Lossless pipeline tutorial, the data needs to be initialized and in the BIDS standard. The steps to prepare data for running through the pipeline can be found in the [BIDS-EEG-EEGLAB tutorial](https://bucanl.github.io/SDC-BIDS-EEG-EEGLAB/). The BIDS-EEG-EEGLAB tutorial needs to be completed prior to this Lossless tutorial.

### Set up a Compute Canada account

- You will need access to a remote cluster to run data files through the pipeline. The Lossless pipeline is optimized to run on Compute Canada clusters. Information on setting up a Compute Canada account can be found [here](https://www.computecanada.ca/research-portal/account-management/apply-for-an-account/). 

{% include links.md %}
