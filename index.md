---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---
The purpose of this lesson is to teach users how to submit remote jobs for the Lossless pipeline to a remote cluster. In this lesson, we will be using the Batch Context plugin for EEGLAB to run a batch of EEG data files in parallel through the Lossless Pipeline by submitting jobs to the Compute Canada Graham cluster. The same procedue should work for any other parallel computer cluster using a slurm scheduler.

<!-- this is an html comment -->

{% comment %} This is a comment in Liquid {% endcomment %}

> ## Prerequisites
- All of the previous BIDS and BIDS EEG sessions, as well as Matlab
- Familiarity with EEGLAB and the Batch Context plugin
- A Notion of Statistical Approaches to EEG
- BASH usage for scp/ssh
{: .prereq}

{% include links.md %}
