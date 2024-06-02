# ChirpChecker: Predicting Criters Names Based On Insect Sounds
<p align="center">
<img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExaG1namU2dXozaDE3cHFrYjY5YWp1NndkaGMwbWNzMGVrOWR1ZmpsdiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/LkSmv0KEK9H3nmNm0c/giphy.webp" />
</p>

## Table of Contents

1. [Introduction](#introduction)
2. [Data Collection & Exploratory Data Analysis](#data-collection--exploratory-data-analysis)
3. [Modeling Approach](#modeling-approach)
4. [Conclusion and Future Directions](#conclusion-and-future-directions)
5. [Credits for FauxRecordings](#credits-for-fauxrecordings)

## Introduction
Welcome to the Insect Sound Classification Models repository! This project aims to build several classification models that differentiate between the three dominant groups of insect sounds: crickets, katydids, and cicadas. By analyzing sound files, our models coarsely categorize these recordings into their respective groups, providing an efficient way to identify the source of various insect sounds.

Insect sound classification has numerous applications, from ecological research to pest control. With these models, you can automatically identify insect sounds, aiding in studies of insect behavior, population monitoring, and more.

This repository contains all the necessary code, and instructions to replicate our results and use the models for your own purposes. Whether you are a researcher, developer, or simply curious about insect sounds, this project provides valuable tools for sound classification.


## Data Collection & Exploratory Data Analysis
For this project, we were given access to a substantial dataset comprising 13,462 labeled insect audio files through the Macaulay Library of Natural Sounds at Cornell University. This dataset includes approximately 160 GB of audio recordings, labeled to identify the sounds of crickets, katydids, and cicadas.

Please note that the dataset was provided to us with specific usage restrictions and cannot be shared with the public. We respect the terms under which this data was made available to us and therefore cannot redistribute the audio files.

For more information about the dataset and access to similar audio files, we encourage you to visit the [Macaulay Library of Natural Sounds](https://www.macaulaylibrary.org/?_ga=2.113818318.857612229.1717218032-636893029.1717218032&_gl=1%2Af8c6mr%2A_gcl_au%2AODU1MzgwODM4LjE3MTcyMTgwMzI.%2A_ga%2ANjM2ODkzMDI5LjE3MTcyMTgwMzI.%2A_ga_QR4NVXZ8BM%2AMTcxNzIxODAzMS4xLjEuMTcxNzIxODA2MS4zMC4wLjA.&doing_wp_cron=1717218075.6205980777740478515625) at Cornell University. The Macaulay Library is a scientific archive for research, education, and conservation, offering an extensive collection of wildlife audio and video recordings.

### Data information:
The recordings began with an introduction, typically a person talking, followed by segments containing insect noises. These recordings varied in length. To clean the dataset, we split off the first 20 seconds of each segment after the introduction, reducing the dataset to 12,602 recordings. We then reviewed these recordings and deleted any files that still contained talking. After this thorough cleaning process, we arrived at a final dataset of 5,899 recordings. The cleaned dataset includes 3,949 recordings of crickets, 1,895 recordings of katydids, and 55 recordings of cicadas. This data cleaning ensured that our dataset was high-quality and suitable for training and evaluating our classification models.

- [MLNS_Insects_Fams_05212024.csv](https://github.com/andrewcmerwin/ChirpChecker/blob/main/MLNS_Insects_Fams_05212024.csv): This file contains metadata on all of the audio samples we acquired. Particularly important are the columns: 'cat_num', used to identify the sample number,'fam_or_subfam', one of 15 possible values, and 'critter_name', one of the three broader values 'cicada', 'cricket', 'katydid'.

- [voice_files.csv](https://github.com/andrewcmerwin/ChirpChecker/blob/main/voice_files.csv): This file contains the 'cat_num' for those samples which had voice as detected by VAD in Data_Prep_for_Feat_Extraction.ipynb

- [no_voice_files.csv](https://github.com/andrewcmerwin/ChirpChecker/blob/main/no_voice_files.csv): This file contains the 'cat_num' for those samples which did not have voice as detected by VAD in Data_Prep_for_Feat_Extraction.ipynb

- [Data_Prep_for_Feat_Extraction](https://github.com/andrewcmerwin/ChirpChecker/blob/main/Data_Prep_for_Feat_Extraction.ipynb): Run this file to prep acquired .wav files (from the Macaulay Library of Natural Sounds or other sources) for feature extraction

- [Feature_Extraction](https://github.com/andrewcmerwin/ChirpChecker/blob/main/Feature_Extraction.ipynb): Run this file to obtain 160 MFCC values and 4 frequency parameters for each prepped .wav file. We use the [librosa package](https://librosa.org/) to extract the MFCC features from each track.

These features could then be input to a variety of traditional machine learning models like Partial least squares regression, KNN, SVC rbf and CNN. 

## Modeling Approach
The following models (Partial least squares regression, KNN, SVC rbf) run and are trained using [MLNS_Final_Train.csv](https://github.com/andrewcmerwin/ChirpChecker/blob/main/MLNS_Final_Train.csv) and tested on [MLNS_Final_Test.csv](https://github.com/andrewcmerwin/ChirpChecker/blob/main/MLNS_Final_Test.csv). MLNS_Final_Train.csv and MLNS_Final_Test.csv contain the final test-train split with columns 'cat_num', 'fam_or_subfam', 'critter_name', as well as all whole-recording features.

- [Partial_Least_Squares_Regression](https://github.com/andrewcmerwin/ChirpChecker/blob/main/Partial_Least_Squares_Regression.ipynb): Use this notebook to train a partial least squares regression on the preprocessed data. Low r<sup>2</sup> values show this method is not suitable.

- [KNN](https://github.com/andrewcmerwin/ChirpChecker/blob/main/KNN.ipynb): Run this notebook for training and testing of k-nearest neighbors classifier.

- [SVC_rbf](https://github.com/andrewcmerwin/ChirpChecker/blob/main/SVC_rbf.ipynb): Run this notebook for training and testing of radial basis function support vector classifier.

Accuracy results and more details can be found in the notebooks.

- [SVD.ipynb](https://github.com/andrewcmerwin/ChirpChecker/blob/main/SVD.ipynb) contains a singular value decomposition of the data in MLNS_Final_Train.csv


- [Convolutional_Neural_Network](https://github.com/andrewcmerwin/ChirpChecker/blob/main/Convolutional_Neural_Network.ipynb): Run this notebook for the model training and visualization for CNN. It loads from [FeedingData.ipynb](https://github.com/andrewcmerwin/ChirpChecker/blob/main/FeedingData.ipynb) multiple methods for feature extraction and contains a few other feature extraction methods itself. It allows for training of 1D and 2D CNN,  plotting training history and confusion matrices, and saving and loading of data at various stages.

## Conclusion and Future Directions

### Model Performance

| Model | Accuracy Critters Names| 
| - | :-: | 
| Partial Least Squares Regression | <50% | 
| KNN | 89% | 
| rbf SVC | 91% | 
| CNN | 90%[^*] | 

[^*]: Dataset used for CNN was a subset of the dataset used in other models

### Looking Ahead
In our future work, we would like to expand our dataset by collecting additional cicada samples and increasing the number of samples across all insect groups. It would be beneficial to compare model performance with clean, high-quality samples to assess the impact of background noise. We aim to develop and integrate noise reduction techniques and analyze their effectiveness. Implementing real-time sound classification capabilities and testing model performance in live conditions would also be valuable. Additionally, we hope to broaden our scope to include more insect groups and other environmental sounds, and create a user-friendly interface with visualization tools for easy model deployment and result interpretation.


## Credits for FauxRecordings
FauxRecoding.wav
Daniel Parker, XC907104. Accessible at [www.xeno-canto.org/907104](https://xeno-canto.org/907104).

FauxRecoding2.wav
K.-G. Heller, XC875015. Accessible at [www.xeno-canto.org/875015](https://xeno-canto.org/875015).

FauxRecoding3.wav
[https://quicksounds.com/library/sounds/cicada](https://quicksounds.com/library/sounds/cicada)

FauxRecording4.wav
Just ACM

FauxRecording5.wav
K.-G. Heller, XC875061. Accessible at [www.xeno-canto.org/875061](https://xeno-canto.org/875061).