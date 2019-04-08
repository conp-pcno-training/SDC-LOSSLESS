---
title: "The Lossless Pipeline"
teaching: 20
exercises: 10
questions:
- "What are the inputs and outputs of the Lossless pipeline?"
- "What is the flow of operations for the Lossless pipeline?"
objectives:
- "Understand the flow of operations used in the Lossless pipeline."
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

This section is designed to outline the key procedures of each of the [Lossless pipeline scripts](https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg/wikis/pipeline-scripts).

### Scalpart (s01)

1. Load an initialized `*_eeg.set` file.
2. Standard Deviation or quantiles used to reject comically bad epochs (% goes to bad channel).
3. Re-reference to interpolated average site of MNI surface: without flagged channels.
4. High pass filter.
5. Low pass filter.
6. Nearest Neighbour R on epochs, % goes to bad channel.
7. Low correlation.
8. High correlation.
9. Rank channel - highest correlation removed.
10. ALL BAD CHANNELS FLAGGED.
11. Re-reference to interpolated average site of MNI surface: without all flagged channels.
12. Nearest Neighbour R on epochs for time removal.
13. Low correlation only by time segment.
14. Save a `*_sa.set` file.

![Channel Standard Deviation Marks](https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg/uploads/a9c836eef4244f3c251a3f36ce19cc4f/diag111.png)

![Channel Correlation Marks](https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg/uploads/861bc4d68ab7c8d074bef2f3bc3e215b/diag222.png)

![Time Correlation Marks](https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg/uploads/54b21404919e9a41a4663b36a4376179/diag333.png)

### Amica (s02, s04a, s04b, s04c)

- The Amica script is run once initially, and then three more times in parallel after the component artefact rejection to ensure that the Amica procedure is replicable if run multiple times on the same dataset.

![AMICA Data Passing](https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg/uploads/bc498981a3b21de4b3a2252d4cd1031c/diag444.png)

### Compart (s03)

- FIXME

### Concat and Dipfit (s05)

- FIXME

{% include links.md %}

