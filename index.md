---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---
The purpose of this lesson is to teach users about the benefits of EEG preprocessing pipelines, the inputs, procedures, and outputs of the [BIDS Lossless pipeline][bids_lossless_eeg], as well as how to submit remote pipeline jobs to a remote cluster. In this lesson, we will be using the Batch Context plugin for EEGLAB to run a batch of EEG data files in parallel through the Lossless pipeline by submitting jobs to the Compute Canada Graham cluster. The same procedure should work for any other parallel computer cluster using a slurm scheduler.

<!-- this is an html comment -->

{% comment %} This is a comment in Liquid {% endcomment %}

> ## Prerequisites
- MATLAB
- All of the previous BIDS and BIDS EEG lessons
- Familiarity with EEGLAB and the Batch Context plugin
- A Notion of Statistical Approaches to EEG
- BASH usage for scp/rsync/ssh
{: .prereq}

{% include links.md %}
