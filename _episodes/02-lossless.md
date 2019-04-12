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
2. Standard deviation or quantiles used for rejecting comically bad epochs (% goes to bad channel).
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
15. Generate an initial AMICA param file based on remaining data.

![Channel Standard Deviation Marks](https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg/uploads/a9c836eef4244f3c251a3f36ce19cc4f/diag111.png)

![Channel Correlation Marks](https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg/uploads/861bc4d68ab7c8d074bef2f3bc3e215b/diag222.png)

![Time Correlation Marks](https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg/uploads/54b21404919e9a41a4663b36a4376179/diag333.png)

### Amica (s02, s04a, s04b, s04c)

- The Amica script is run once initially, and then three more times in parallel after the component artefact rejection to ensure that the Amica procedure is replicable if run multiple times on the same dataset.

![AMICA Data Passing](https://git.sharcnet.ca/bucanl_pipelines/bids_lossless_eeg/uploads/bc498981a3b21de4b3a2252d4cd1031c/diag444.png)

### Compart (s03)

1. Load a `*_sa.set` file.
2. Load initial AMICA model.
3. Create time marks based on the calculated log likelihood marks for each time point of the AMICA model (values between 0 and 1).
4. Standard deviation or quantiles used for rejecting comically bad epochs based on abnormally high component activation.
5. Mark any remaining small gaps of 2 seconds or less to avoid having too many excessively short segments.
6. Save a `*_compart_data.set` file.
7. Generate AMICA A, B, and C param files based on remaining data.

### Concat and Dipfit (s05)

1. Load a `*_compart_data.set` file.
2. Load AMICA models A, B, and C.
3. Create time marks based on the calculated log likelihoods for each time point of each of the AMICA models.
4. Perform ISCtest to identify reliably replicable components, and flag those that aren't.
5. Standard deviation or quantiles used for rejecting comically bad epochs based on abnormally high component activation (second pass).
6. Calculate and flag time periods high in delta/theta, alpha, beta, and low/high gamma, but don't add them to the manual mark.
7. Mark any remaining small gaps of 2 seconds or less to avoid having too many excessively short segments.
8. Perform dipole fit.
9. Save a `*_ll.set` file.

{% include links.md %}

