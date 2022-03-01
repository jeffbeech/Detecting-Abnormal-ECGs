# ECG Anomaly Detection

## Introduction

The electrocardiograph (ECG) is a universally accepted diagnostic tool for physicians to analyze heartbeats.  Since its invention in 1902 by Dutch physician Willem Einthoven, it ushered in "a new era in which various machines and technical procedures gradually replaced the physician's unaided senses and the stethoscope as the primary tools of cardiac diagnosis." [ (source)](https://pubmed.ncbi.nlm.nih.gov/8184849/#:~:text=The%20invention%20of%20the%20electrocardiograph,arrhythmias%20and%20acute%20myocardial%20infarction.). Without doubt, the identification of heart anomalies through ECG's is an essential activity, and the use of machine learning models to aid physicians in this task would prove quite useful.

## Business Problem

Doctors are in short supply worldwide.  Some areas, such as in central Africa and western and central Asia, have less than 1 medical doctor per 1,000 people (2018 data from the World Bank.)  [Medic Mobile](https://medic.org/) has contracted my company to research and develop a viable model that can identify and tag anomalous electrocardiographs from a dataset that contains normal and abnormal ECGs.  Medic Mobile is a nonprofit organization that strives to improve health care for those living in hard-to-reach communities.  Medic Mobile now impacts 14 countries in Africa and Asia, having trained and equipped 24,463 health workers.

## The Dataset

This dataset, "ECG5000" is a 20-hour long ECG originating from Physionet. It consists of 5,000 heartbeats randomly selected from a single patient with congestive heart failure, recorded over a 20 hour period.  The set contains 2,919 normal heartbeats, labeled as “1”, and 2,079 abnormal heartbeats, labeled as “0”.  Each heartbeat has been made equal length, and consists of 140 datapoints.

## Modeling
The problem at hand will be solved with a neural network using an autoencoder.  The graph below illustrates how this works, but it is easiest to think of it in the same way your phone camera works – your phone takes an image, compresses it for storage purposes, and recreates it from the compressed version when you look at it on your phone screen.  An autoencoder takes data, compresses it, and then attempts to reassemble it as closely as possible to the original.

### Illustration

For our model, the autoencoder will be trained to recognize only normal ECGs.  The difference between each original and “reconstructed” ECG will measure and recorded as the amount of error.  We’ll take the errors and add a threshold amount – anything greater than that total amount will be identified as an abnormal ECG when we show the model our mixed set of normal and abnormal ECGs.
