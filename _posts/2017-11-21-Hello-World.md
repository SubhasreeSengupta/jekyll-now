---
layout: post
title: Automatic Music Transcription for Polyphonic Music for Multiple Instruments
---

 

# Project Objective
Music is an integral part of human life. It has its own charm and magic. Yet, interestingly, there is complex mathematical structure embedded in music. Many researchers and scientists have been working to on different aspects of music, like learning the positive psycholocgial effects of music, identifyting the underlying musical style in a musical piece, just to name a few. Deep Learning has been successful in areas such as computer vision, natural language processing. It has performed better than humans in tasks like image classification, object recognition and many more. Given there exists a mathematical framework embedded in music, there have been attempts by researchers to use the power of deep learning in this domain to achieve a variety of tasks such as predicting song similarity, composer identification and even music generation.

In our project, we would like to address the problem of "Automatic Music Transcription for Polyphonic Music involving Multiple Instruments". Automatic Music Transcription (AMT) is an open problem in Music Information Retrieval(MIR). AMT aims to generate a symbolic representation, a near-score like transcription, given a polyphonic musical piece. This is a difficult problem for the following reasons: (1) In polyphonic music, there exists a complex interation and overlap of harmonies arising from different acoustic signals in the melody. (2) Seperating the sources of music in a given piece with multiple instruments is extremely difficult. So AMT fails to match human performance. We wish to use deep learning to address these challenges and then framework to succesfully achieve the mentioned aim . 


In order to transcribe each instrument in a musical piece we first addressed the challenge of instrument separation, the output of which are separate wav files corresponding to the instruments we separted out. Next to further validate the separation step, we design a "predominant instrument classifier", the goal of which is to identify the predominant instrument in each musical piece. This step compliments the transcription process and acts as a validation to the output of the separation step. Finally we build a deep learning model for automatic transcription, which is the ultimate goal of our project.  

In the the following sections, we describe and visualize several aspects of our project.

# Project Pipeline

Below we present a visual presentation of our project pipeline

![_config.yml]({{ site.baseurl }}/images/Project_pipeline.PNG)


In each of the sections below we describe each of the 3 main steps of the pipeline.
(1) We first describe the instrument separation task.
(2) Then we motivate the need for the predominant instrument identification task and describe our approach.
(3) We finally connect (1),(2) to address the transcription process and describe our method.


# Instrument Segregation:
In order to make an end to end approach, we need to focus on filtering each instrument out of any given music piece. This part is analogous to source separation in music, which is also an ongoing area of research and is extremely difficult, as you need to have a way to get ground truth labels for each source(instrument) and also tune the loss function in such a way that the loss function also accounts for increasing the difference among each instrument at the same time as recongizing it. Below we describe our approach. Before we describe the model architecture we briefly mention some preprocessing steps.

### Preprocessing 

The figure below shows the spectrogram of an audio file we used in our dataset.
Spectograms are a common way of visualizing audio signals. The x-axis represents time, y-axis represents frequency of sounds at that time. The different intensities of colors are used to express a 3rd dimension - amplitude corresponding to a particular frequency.

The very first preprocessing step is to convert our audio files from dual channel to mono channel, which is accomplished by taking the average of the two channels. 


![_config.yml]({{ site.baseurl }}/images/Initialial_img.PNG)


After the initial preprocessing, we also max-normalize the signal data by time and downsample the data.

We use STFT transform function to create a time-frequency representation of our data. 
![_config.yml]({{ site.baseurl }}/images/STFT transform.PNG)

### Model Description:
Our approach is based on the paper : TO-DO insert paper link. 
We use an LSTM based model architecture. Visually the architecture we use is:

![_config.yml]({{ site.baseurl }}/images/Source_sep_model.PNG)

The masking layer is a technique by which the probabilities from each dense layer for each source are used to apply a mask to the initial spectrogram to transform the spectrogram such that the source based on which the mask is applied is predominantly represented.

Finally, an inserve stft transform is applied to convert the masked spectrogram which a time-frequency magnitude representation back to audio signals, which we store in .wav file. 

Below we show a visual captured from the training of the network:
![_config.yml]({{ site.baseurl }}/images/source_sep_loss.PNG)

We also experimented with different model architectures, with different hyperparameters such as number of iterations, number of LSTM layers, but did not see much variations in the training process from our current specified architecture.

The model achieves separating the outputs into two parts, separating by each instrument is adds several layers of complexity in terms of preprocessing, identifying ground truth labels, defining the loss function etc,  and hence is a direction for future work.


### Dataset:
 We use the MIR-1K dataset (MIR stands for Music information retrieval). It comprises of a variety of songs, a total of 1000 musical clips. This dataset is annotated with several musical features such as pitch, timbre, etc. We use this dataset since it is a popular dataset used in source separation literature as it captures various aspects of music and hence forms a very powerful dataset to be used for training. We use to the .wav files (audio signals) to build and test our model.
 
#### TO DO: Describe evaluation.
 
# Predominant instrument classification:


# Automatic transcription

### Preprocessing:
CQT transform is used in our experiments for automatic transcription.

![_config.yml]({{ site.baseurl }}/images/CQT_transform.PNG)


### Model Description

### Dataset for Automatic transcription:
Before we delve into the technical details of the project, we briefly describe the dataset we use. We used the MAPS (MIDI Aligned Piano Sounds) dataset, which is a freely accessable dataset focused on piano melodies as our dataset. It is composed by isolated notes, random-pitch chords, usual musical chords and pieces of music. The database provides a large amount of sounds obtained in various recording conditions. 
MAPS provides recordings with CD quality (16-bit, 44-kHz sampled stereo audio) and the related aligned MIDI files as ground truth labels. The overall size of the database is about 40GB, i.e. about 65 hours of audio recordings.

Inorder to use this we first apply some pre-processing steps which are as described below. Finally, the total number of frames in our dataset are ~5 million, which amounts to a huge corpora of audio signals for transcription.



