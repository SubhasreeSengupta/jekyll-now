
---
layout: post
title: End-to-end transcription for Piano Music 
---

# End-to-end transcription for Piano Music
In the End to End transcription section, we are building a convolutional neural network model that takes a audio file as an input and provides the transcription for the Piano music. 

## Preprocessing
We transform the input audio to a time-frequency representation which is then input to the model. Here we experiment with the constant Q transform (CQT) as the input representation with 36 bins and a hop size of 512 samples. A padding with a window size of 7 is also added to the input files.

The output files are converted to piano roll representation using pretty_midi python library.

## Dataset
We evauated our model on the MIDI Aligned Piano Sounds(MAPS) dataset. The overall size of the database is about 40GB, i.e. about 65 hours of audio recordings. Each audio wav file consists of corresponding MIDI file which is used in extracting the piano roll music information for the music piece.

The MAPS dataset is divided into 9 categories based on the recording conditions, attributes of the audio files recorded and instruments used. 
Type of Sounds: They consist of isolated notes and monophonic excerpts, random chords, usual chords and full pieces of music. 
The training set consisted of 6 categories of the synthesized piano music.
Recording conditions: software default, church, concert hall, studio, jazz club, "Ambient", "Close"
Instrument: Software synthesized(The Grand 2, Akoustik Piano, The Black Grand) or Real instrument (Yamaha Disklavier Mark III) 

We tried two approaches to use this dataset to train our model. In both approaches, we used 6 software synthesized music categories for training,  1 software synthesized music category for validation, and 2 real instrument recordings (Yamaha Disklavier) for testing.
Approach 1 - Use all of the dataset: along with isolated notes, monophonic excerpts, random chords and usual chords
Approach 2 - Use only full pieces in each type of instrument and recording conditions:  There are 9 categories of recordings corresponding to different piano types and recording conditions, with 30 recordings per category. Therefore the dataset consists of 210 synthesised recordings and 60 real recordings.
Approach 3 - Use only Isolated notes and monophonic excerpts.

The Approach 1 was computationally more expensive and also was not efficient.  

## Model Description
<<image>>

The ConvNet model consist of two convolutional-dropout-maxpool layers followed by a flatten layer and two dense-dropout layers. We use hyperbolic tan as the activation functions at the conv layers and sigmoid at the dense layers. The dropout value is set to 0.5 across the layers.

An Early Stopping callback with a patience of 5 was added to stop the training on no change in validation loss and Checkpoint call backs were added to store the checkpoints at each epoch. We use binary_crossentropy loss function and SGD optimizer with an initial learning rate of 0.01 and a linear decay in learning rate.

## Training and setup

We preprocessed and trained the MAPS dataset in the GCP GPU instance(configuration: 1 Nvidia Tesla K80 GPU, 8 vCPUs, 52 GB RAM, 100 GB Memory). A high epoch value of 1000 was set to verify if there is any improvements in the accuracies.

Approach 1: The entire dataset trained for 7 epochs before early stopping. 

Approach 2: In this approach, the different categories of full musical pieces were trained per folder. The training stopped at 63 epochs, 20 epochs, 7 epochs, 7 epochs, 7 epochs for the training folders.

Approach 3:

## Results
(output visualization)

## References

• An End-to-End Neural Network for Polyphonic Piano Music Transcription, Siddharth Sigtia, Emmanouil Benetos, and Simon Dixon

MAPS Dataset:

• V. Emiya, R. Badeau and B. David, Multipitch estimation of piano sounds using a new probabilistic spec tral smoothness principle, IEEE Transactions on Audio, Speech and Language Processing, (to be published);

• V. Emiya, Transcription automatique de la musique de piano, Thèse de doctorat, Telecom Paris-Tech, 2008 (in French).
