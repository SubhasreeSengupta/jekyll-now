---
layout: post
title: End-to-end transcription for Piano Music 
---

In the End-to-End transcription section, we are building a Convolutional Neural Network (CNN) model that takes an audio file as an input and provides the transcription for the Piano music. We train and evaluate the model on a dataset of polyphonic piano music. The use of CNN for transcription of polyphonic music is still an active area of research. 

## Preprocessing
We transform the input audio file (MIDI file) to a time-frequency representation which is then given as input to the model. Here, we experiment with the constant-Q transform (CQT) as the input representation with 36 bins and a hop size of 512 samples. A padding with a window size of 7 is also added to the input files.


The output files are converted to piano roll representation using **pretty_midi** python library.

## Dataset
- We evauated our model on the **MAPS** (MIDI Aligned Piano Sounds) dataset. The overall size of the database is about 40GB, i.e. about 65 hours of audio recordings. Each audio wav file consists of corresponding MIDI file which is used in extracting the piano roll music information for the music piece.

+ The MAPS dataset is divided into 9 categories based on the recording conditions, attributes of the audio files recorded and instruments used. 
- Type of Sounds: They consist of isolated notes and monophonic excerpts, random chords, usual chords and full pieces of music. 
+ The training set consisted of 6 categories of the synthesized piano music.
- Recording conditions: software default, church, concert hall, studio, jazz club, "Ambient", "Close"
+ Instrument: Software synthesized(The Grand 2, Akoustik Piano, The Black Grand) or Real instrument (Yamaha Disklavier Mark III) 

We tried two different approaches to use this dataset to train our model. In both approaches, we used 6 software synthesized music categories for training:  
1. Software synthesized music category for validation
2. Real instrument recordings (Yamaha Disklavier) for testing.

**Approach 1:** Use the whole dataset 
Along with isolated notes, monophonic excerpts, random chords and usual chords
**Approach 2:** Use only full pieces in each type of instrument and recording conditions
There are 9 categories of recordings corresponding to different piano types and recording conditions, with 30 recordings per category. Therefore the dataset consists of 210 synthesised recordings and 60 real recordings.

The Approach 1 was computationally more expensive and also was not efficient.  

## Model Description
![_config.yml]({{ site.baseurl }}/images/model_end_to_end.png)

The ConvNet model consist of two convolutional-dropout-maxpool layers followed by a flatten layer and two dense-dropout layers. We use **hyperbolic tan** as the activation functions at the conv layers and sigmoid at the dense layers.   The dropout value is set to 0.5 across the layers.

An Early Stopping callback with a patience of 5 was added to stop the training if there is no change in validation loss and checkpoint callbacks were added to store the checkpoints at each epoch. We use **binary_crossentropy** loss function and **SGD** optimizer with an initial learning rate of 0.01 and a linear decay in learning rate.

## Training and setup

We preprocessed and trained the MAPS dataset using the GCP GPU instance (configuration: 1 Nvidia Tesla K80 GPU, 8 vCPUs, 52 GB RAM, 100 GB Memory). A high epoch value of 1000 was set to verify if there is any improvements in the accuracies.

**Approach 1:** The entire dataset trained for 7 epochs before early stopping. 

**Approach 2:** In this approach, the different categories of full musical pieces were trained per folder. The training stopped at 63 epochs, 20 epochs, 7 epochs, 7 epochs, 7 epochs for the training folders. 

## Metrics

We used frame based metrics to assess the performance of the system. We compare the predicted values with the groud-truth and obtain the overall Fmeasure, precision, recall at each time frame. These values are then averaged over for all the frames in the piece and across the pieces.

## Results
Precision: 5.74  
Recall: 39.76  
f1 score: 0.1  
loss: 0.1368  
validation accuracy: 96.5  

## Observations

We were able to succesfully complete the preprocessing and training of the model. We faced challenges in our post processing to infer the results predicted by the model and capture the information required for capturing the transcription.

The issues faced involved finding the most likely notes per time frame and each time frame. The post processing method resulted in converting the output generated to all zeros and hence a very low precision values.

Also, we observed a very high validation and training accuracy and we suspect a severe overfitting in the model. Various attempts to train the model with different dataset combinations, hyperparameter values and pre processing methods could not improve the model's generalization.

## Possible Improvements

Fix the issues related to post processing and predict the piano transcription for the given input pieces.

## References

1. Sigtia, Siddharth, Emmanouil Benetos, and Simon Dixon. "An end-to-end neural network for polyphonic piano music transcription." IEEE/ACM Transactions on Audio, Speech and Language Processing (TASLP) 24.5 (2016): 927-939.  

2. MAPS Dataset:
Multi-pitch estimation of piano sounds using a new probabilistic spectral smoothness principle, V. Emiya, R. Badeau, B. David, IEEE Transactions on Audio, Speech and Language Processing, 2010.

[BACK HOME](https://subhasreesengupta.github.io/project-intro/)
