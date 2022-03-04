# ECG Anomaly Detection

## Introduction

The electrocardiograph (ECG) is a universally accepted diagnostic tool for physicians to analyze heartbeats.  Since its invention in 1902 by Dutch physician Willem Einthoven, it ushered in "a new era in which various machines and technical procedures gradually replaced the physician's unaided senses and the stethoscope as the primary tools of cardiac diagnosis." [ (source)](https://pubmed.ncbi.nlm.nih.gov/8184849/#:~:text=The%20invention%20of%20the%20electrocardiograph,arrhythmias%20and%20acute%20myocardial%20infarction.). Without doubt, the identification of heart anomalies through ECG's is an essential activity, and the use of machine learning models to aid physicians in this task would prove quite useful.

## Business Problem

Doctors are in short supply worldwide.  Some areas, such as in central Africa and western and central Asia, have less than 1 medical doctor per 1,000 people (2018 data from the World Bank.)  [Medic Mobile](https://medic.org/) has contracted my company to research and develop a viable model that can identify and tag anomalous electrocardiographs from a dataset that contains normal and abnormal ECGs.  Medic Mobile is a nonprofit organization that strives to improve health care for those living in hard-to-reach communities.  Medic Mobile now impacts 14 countries in Africa and Asia, having trained and equipped 24,463 health workers.

## The Dataset

This dataset, "ECG5000" is a 20-hour long ECG originating from Physionet. It consists of 5,000 heartbeats randomly selected from a single patient with congestive heart failure, recorded over a 20 hour period.  The set contains 2,919 normal heartbeats, labeled as “1”, and 2,079 abnormal heartbeats, labeled as “0”.  Each heartbeat has been made equal length, and consists of 140 datapoints.

The data was split to remove the labels from the data and create a target data file with just the labels.  Borth files were train-test split, and the data file was scaled between 0 and 1 as tensor arrays.  The training set was divided into normal and abnormal sets so the model could be trained on just the normal heartbeats.

## Modeling
The problem at hand will be solved with a neural network using an autoencoder.  The graph below illustrates how this works, but it is easiest to think of it in the same way your phone camera works – your phone takes an image, compresses it for storage purposes, and recreates it from the compressed version when you look at it on your phone screen.  An autoencoder takes data, compresses it, and then attempts to reassemble it as closely as possible to the original.

![](https://user-images.githubusercontent.com/89176309/156230964-139eea97-a9c8-4da3-b39d-7e05ef0fbd56.png)

For our model, the autoencoder will be trained to recognize only normal ECGs.  The difference between each original and “reconstructed” ECG will measure and recorded as the amount of error.  We’ll take the errors and add a threshold amount – anything greater than that total amount will be identified as an abnormal ECG when we show the model our mixed set of normal and abnormal ECGs.



A baseline model, using Principal Component Analysis in the Autoencoder, was instatiated and run.  The model was simple, having only 2 layers in and out, and a bottleneck of 2 dimensions.  Because it was a PCA model, the relationships caculated between dimensions were linear, and error was calculated using mean squared error (MSE).  This model was run for 15 epochs.  The process for each model was as follows:

Comparison of Train vs. Validation Loss

Visualization of Input vs. Reconstruction of 10 Random Normal and Abnormal ECGs.

Calculation of a threshold

Making predictions

Scoring (Accuracy, Precision, Recall, F1)

Confusion Matrix

### Sample graphs of normal and abnormal heartbeats with reconstructions and error

![image](https://user-images.githubusercontent.com/89176309/156687955-d2cac850-1890-4214-9707-2057cf1def29.png)
![image](https://user-images.githubusercontent.com/89176309/156688083-a69cca95-1b2b-4748-ab8c-0a8bec1ed2dc.png)

## Evaluation
The precision metric was the primary metric I used, because it tells the percentage of the time I was predicting a normal heartbeat and getting it right.  It made more sense to lean into this metric, since we would rather tell someone they had a heart condition and find out they didn't than tell them they have a healthy heart when they do not.  (Truly, we'd rather get every single ECG right, but our models are not that good, yet!)  For correctly differentiating between normal and abnormal heartbeats, and also having the fewest number of false positives, the best model was the autoencoder, tuned with keras tuner.  The code for the keras tuner was adaptedd from [analyticvidhya.com](https://www.analyticsvidhya.com/blog/2021/05/anomaly-detection-using-autoencoders-a-walk-through-in-python/)  The model gave a precision score of ~99%, meaning that of the all the people predicted of having a normal heartbeat, the model was correct ~99% of the time.  The model had an accuracy score of ~95%, which means it accurately evaluated all of the ECGs 95% of the time.  

I also looked at the graphs for the incorrect predictions we made (example below). For the 3 false positives, the graphs show a fairly faithful reproduction with, by appearance, a low error rate.  Obviously, something that couldn't be detected by training the model was wrong with these ECGs.  For the false negatives, for the most part, it appears as though the model just did a poor job of recognizing these ECGs, as the reconstructions are not faithful to the originals.

![sample false positive](https://user-images.githubusercontent.com/89176309/156607544-885f74b7-bebc-44ee-bb07-ae6f34af813b.png)
![sample false negative](https://user-images.githubusercontent.com/89176309/156607584-c9aa07e8-bfdf-4fb4-8b53-3308083b690b.png)

Finally, we compared our models as shown in the graph below - as you can see, 3 of the 4 models all had good scores; the best model won out by doing best with false positives and was also first place in false negatives.

![scores](https://user-images.githubusercontent.com/89176309/156624305-ba539f16-f8a2-4ced-8aa6-57e9d358ce1e.png)

## Recommendations and Next Steps
First, it is important to emphasize that nothing replaces an expert eye and mind on complex health issues.  These recommendations are meant to supplement medical professionals, not to replace their expertise.  With that said, I suggest these items as next steps:

1. Verify the modeling used here with a much larger dataset including heartbeats from multiple individuals.  Collecting a large database of normal heartbeats would be best initial move forward.
2. Clarify your vision with your medical professionals.  Approach this as a method to support medical staff, not to replace the ultimate need for doctors to read ECGs.
3. This model could easily be implemented in a browser, but a hardware consultant should be retained to develop the necessary hardware and or software to input the data into a browser or app.  
4. Once the data input problems are solved, training your staff to use the model would be quite simple.

## Repository Structure
```

├── autoencoder
|   ├──tuning_autoencoder6
|      ├──Folders containing Keras Tuner modeling info   
|
├── data
|   ├── ecg.csv
|   ├── scores.csv 

├── Images 
|   ├── anomalous_ecg.png
|   ├── Architecture-of-an-undercomplete-autoencoder-with-a-single-encoding-layer-and-a-single.png
|   ├── best_model_abnorm_in_out.png
|   ├── best_model_cm.png
|   ├── best_model_norm_in_out.png
|   ├── normal_ecg.png
|   ├── pca_model_1_abnorm_in_out.png
|   ├── pca_model_1_cm.png
|   ├── pca_model_1_norm_in_out.png
|   ├── pca_model_2_abnorm_in_out.png
|   ├── pca_model_2_cm.png
|   ├── pca_model_2_norm_in_out.png
|   ├── sample false negative.png
|   ├── sample false negatives.png
|   ├── sample false positive.png
|   ├── sample false positives.png
|   ├── scores.png
|   ├── seq_model_1_abnorm_in_out.png
|   ├── seq_model_1_cm.png
|   ├── seq_model_1_norm_in_out.png

├── notebooks
|   ├── Streamlit.ipynb
|   ├── work_notebook_1.ipynb
|   ├── work_notebook_2.ipynb
|   ├── jeff.ipynb
    
├── .gitignore

├── first_model_notebook.ipynb

├── project_notebook.ipynb

├── presentation.pdf

├── requirements.txt

├── environment.yml

├── README.md
```

## For more information
Find the full [Jupyter notebook](https://github.com/jeffbeech/ECG_Anomaly_Detection/blob/main/project_notebook.ipynb) and the [presentation](https://github.com/jeffbeech/ECG_Anomaly_Detection/blob/main/Presentation.pdf).
