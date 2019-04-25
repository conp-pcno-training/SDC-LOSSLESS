---
title: Setup
teaching: 0
exercises: 10
questions:
- "How do we download the Lossless Pipeline?"
- "How do we set up the Lossless Pipeline?"
objectives:
- "Downloading and setting up the pipeline."
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

### Download and install MATLAB

- You will need a fairly recent version of MATLAB (procedure tested on MATLAB 2012b and newer versions). For detailed installation instructions, click [here](https://www.mathworks.com/help/compiler/install-the-matlab-runtime.html).

### Set up Globus

1. Open up your favourite browser and enter the [Globus][globus] website.

2. Create a Globus account using one of your existing emails.

3. Once logged in, create a Globus endpoint (this is basically a link to a location on your local machine). To do this, click on Endpoints in the sidebar and click the “+” icon in the top right to create a new endpoint, and select [Globus Connect Personal][gcp]. Follow the steps to generate a setup key, and download the appropriate version for your operating system. This process should be fairly straightforward for Windows and Mac users.

- **Linux users**

    - Make sure you have the required package, tcllib, installed. You can manually install the tcllib package by downloading the newest version directly from [here][tcllib].

    - Click the link to the newest package (near the top).

    - Select the Tar.gz snapshot link from the Github Mirror.

    - Once the compressed file has been downloaded, extract it, navigate to the extracted directory in the terminal, and install the package:

        `>> cd path/to/tcllib-1.18/`  
	    `>> ./installer.tcl`  

    - Extract the downloaded Globus Connect folder, navigate to the extracted folder in the terminal, and run Globus Connect:

        `>> cd path/to/globusconnectpersonal-2.3.6/`  
        `>> ./globusconnect`  

    - Once Globus Connect is running, input the Globus setup key into the Security Code field. Then, in the main Globus Connect window, click **Connect**.

    ![Globus Initial Setup]({{ page.root }}/fig/GlobusInitialSetup.png "Globus Initial Setup")
    ![Globus Connect Main]({{ page.root }}/fig/GlobusConnectMain.png "Globus Connect Main")

    **NOTE:** By default, your endpoint will only give you access to your home drive. If your project is in a different location, it will need to be added to the directory access list. In Windows and Mac, this should be the first window that appears after installation, and you can simply add the drives and directories you need. In Linux, you will initially only have access to the `~/home/` directory, so if you are working from an external drive, you will need to add `~/media/` as a directory in the **File->Preferences** window.

    ![Globus Drop Down Preferences Menu]({{ page.root }}/fig/GlobusDropDownPreferences.png "Globus Drop Down Preferences Menu")
    ![Globus Preferences Menu]({{ page.root }}/fig/GlobusPreferences.png "Globus Preferences Menu")

### Download the data in a BIDS-compliant folder structure

1. On the Globus website, navigate to the [File Manager][globusfm]. In the **Collection** field, search ‘bucanl_face13’.

2. Select all folders with the sub- prefix, and select the option **Transfer or sync to…**.

3. A new Destination header will appear with another Collection field to fill out. Select your Globus endpoint and navigate to your desired directory where you would like to download the data files.

4. Click **Start** to begin the transfer. This may take some time, depending on transfer speed.

{% include links.md %}
