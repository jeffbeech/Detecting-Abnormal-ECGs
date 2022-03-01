# ECG Anomaly Detection

## Introduction

The electrocardiograph is a universally accepted diagnostic tool for physicians to analyze heartbeats.  Since its invention in 1902 by Dutch physician Willem Einthoven, it ushered in "a new era in which various machines and technical procedures gradually replaced the physician's unaided senses and the stethoscope as the primary tools of cardiac diagnosis." [Source here](https://pubmed.ncbi.nlm.nih.gov/8184849/#:~:text=The%20invention%20of%20the%20electrocardiograph,arrhythmias%20and%20acute%20myocardial%20infarction.). Without doubt, the identification of heart anomalies through ECG's is an essential activity, and the use of machine learning models to aid physicians in this task would prove quite useful.

## Business Problem

## The Dataset and Approach to the Problem
From timeseriesclassification.com... The original dataset for "ECG5000" is a 20-hour long ECG downloaded from Physionet. The name is BIDMC Congestive Heart Failure Database(chfdb) and it is record "chf07". It was originally published in "Goldberger AL, Amaral LAN, Glass L, Hausdorff JM, Ivanov PCh, Mark RG, Mietus JE, Moody GB, Peng C-K, Stanley HE. PhysioBank, PhysioToolkit, and PhysioNet: Components of a New Research Resource for Complex Physiologic Signals. Circulation 101(23)". The data was pre-processed in two steps: (1) extract each heartbeat, (2) make each heartbeat equal length using interpolation. This dataset was originally used in paper "A general framework for never-ending learning from time series streams", DAMI 29(6). After that, 5,000 heartbeats were randomly selected. The patient has severe congestive heart failure and the class values were obtained by automated annotation. The dataset is stationary.

The problem at hand will be solved with a neural network using an autoencoder.  An autoencoder for anomaly detection "follows the general principle of first modeling normal behavior and subsequently generating an anomaly score for each new data sample. To model normal behavior, we follow a semi-supervised approach where we train the autoencoder on normal data samples. This way, the model learns a mapping function that successfully reconstructs normal data samples with a very small reconstruction error. This behavior is replicated at test time, where the reconstruction error is small for normal data samples, and large for abnormal data samples. To identify anomalies, we use the reconstruction error score as an anomaly score and flag samples with reconstruction errors above a given threshold." [source](https://ff12.fastforwardlabs.com/)

