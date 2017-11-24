---
layout: post
title: Automatic Music Transcription for Polyphonic Music for Multiple Instruments
---
# Project Objective
Music is an integral part of human life. It has its own charm and magic. Yet, interestingly, there is complex mathematical structure embedded in music. Many researchers and scientists have been working to on different aspects of music, like learning the positive effets of music, identifyting the pattern lying in the music, etc. Deep Learning has been successful in areas like computer vision, natural language processing. It has performed better than humans in tasks like image classification, object recognition and many more to name. Given the mathematical correlation of music, there have been attempts by researchers to use the power of deep learning in this domain. 

We would like to address the problem of "Automatic Music Transcription for Polyphonic Music involving Multiple Instruments". Automatic Music Transcription (AMT) is an open problem in Music Information Retrieval(MIR). AMT aims to generate a symbolic representation, a score like transcription, given a polyphonic music. This is a difficult problem for the following reasons: (1) for polyphonic music, number of notes are being played simultaneously. This causes a complex interation and overlap of harmonics in the acoustic signals. (2) Seperating the sources of music in a given piece with multiple instruments is extremely difficult. So AMT fails to match human performance. We wish to use deep learning to tackle these problems. 

In the the following sections, we describe and visualize several aspects of our project.

# Project Pipeline

Below we present a visual presentation of our project pipeline

![_config.yml]({{ site.baseurl }}/images/Project_pipeline.PNG)

# Our Approach
Our approach to transcribe each musical instrument in the polyphonic audio input involves three steps:
(1) Instrument Segregation: In order to make an end to end approach, we need to focus on filtering each instrument out of any given music piece. This part is analogous to source separation in music, which is also an ongoing area of research and is extremely difficult, as you need to have labels corresponding to each instrument and also tune the loss function in such a way that the loss function also accounts for increasing the difference among each instrument at the same time as recongizing it. We investigated approaches that can separate out vocals, drums and bass instruments out of a musical mixture. Parallely, we also visualized this source segregation objective as identifying the predominant instrument in a mixture. Although we focus on piano transcription, the approach can be adapted for any instrument as long as we have a single instrument we want to transcribe for which the predominant instrument identification task is extremely crucial.

As part of implementation, presented a polyphonic melody, we create a learning algorithm to seperate the instruments presented in it. To accomplish this, we use <<describe the model, dataset and other details in short here>>

(2) Predominant Musical Instrument Identification: We feed the segregated audio files to identify the predominant musical instrument in each segregation. This step is necessary to identify the exact instrument in the audio files after the source seperation is completed. We build a model << add the details of the model, dataset, output and other details>>
(3) Automatic Music Transcription: After identification of the predominant instrument, we feed the audio files to the specific learning algorithm trained to transcribe the musical instrument. This helps in note identification for each instrument leading to transcription. For our task, we have trained a model to transcribe the polyphonic piano music, <<add the details of the model here>>

In the following sections we will describe each of the steps in the pipeline.

# Source Separation
This part of the pipeline acts as more of a preprocessing step. This step is used to extract the audio signals corresponding to the different instruments we have in our musical piece. We focus on an approach that is best suited for extracting 2 audio signals from a musical mixture. Before we describe the model architecture we briefly mention some preprocessing necessary.

## Preprocessing 

The figure below shows the spectrogram of an audio file we used in our dataset.
Spectograms are a common way of visualizing audio signals. The x-axis represents time, y-axis represents frequency of sounds at that time. The different intensities of colors are used to express a 3rd dimension - amplitude corresponding to a particular frequency.

The very first preprocessing step is to convert our audio files from dual channel to mono channel, which is accomplished by taking the average of the two channels. 


![_config.yml]({{ site.baseurl }}/images/Initialial_img.PNG)


After the initial preprocessing, we also max-normalize the signal data by time and downsample the data.

We use STFT transform function to create a time-frequency representation of our data. 
![_config.yml]({{ site.baseurl }}/images/STFT transform.PNG)

## Model Description:
Our approach is based on the paper : TO-DO insert paper link. 
We use an LSTM based model architecture. Visually the architecture we use is:

![_config.yml]({{ site.baseurl }}/images/Source_sep_model.PNG)

The masking layer is a technique by which the probabilities from each dense layer for each source are used to apply a mask to the initial spectrogram to transform the spectrogram such that the source based on which the mask is applied is predominantly represented.

Finally, an inserve stft transform is applied to convert the spectrogram which a time-frequency magnitude representation back to audio signals, which we store in .wav file. 

Below we show a visual captured from the training of the network:
![_config.yml]({{ site.baseurl }}/images/source_sep_loss.PNG)




# Automatic transcription

## Preprocessing:
CQT transform is used in our experiments for automatic transcription.

![_config.yml]({{ site.baseurl }}/images/CQT_transform.PNG)


## Model Description

# Dataset
## Dataset for Source separation:
#TO-DO describe the Mir-1k dataset.


## Dataset for Automatic transcription:
Before we delve into the technical details of the project, we briefly describe the dataset we use. We used the MAPS (MIDI Aligned Piano Sounds) dataset, which is a freely accessable dataset focused on piano melodies as our dataset. It is composed by isolated notes, random-pitch chords, usual musical chords and pieces of music. The database provides a large amount of sounds obtained in various recording conditions. 
MAPS provides recordings with CD quality (16-bit, 44-kHz sampled stereo audio) and the related aligned MIDI files as ground truth labels. The overall size of the database is about 40GB, i.e. about 65 hours of audio recordings.

Inorder to use this we first apply some pre-processing steps which are as described below. Finally, the total number of frames in our dataset are ~5 million, which amounts to a huge corpora of audio signals for transcription.



