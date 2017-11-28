---
layout: post
title: Musical Source Separation
---

Below we present a detailed overview of our approach for source separation of a musical piece. This part is divided into the following sections: First we describe the pre-processing steps needed to feed the data into the model.  Next we describe the model architecture and the details of the approach. Finally, we mention some evaluation metrics for our method and the performance of our model.
We build upon the paper (1) and it forms the main backbone of our approach. However, we improve upon the existing model by using LSTMs layers in the model architecture. We further modify the loss such that it also accounts for increasing the difference between the sources. 

## Preprecessing

The figure below shows the spectrogram of an audio file we used in our dataset.
Spectograms are a common way of visualizing audio signals. The x-axis represents time, y-axis represents frequency of sounds at that time. The different intensities of colors are used to express a 3rd dimension - amplitude corresponding to a particular frequency.

The very first preprocessing step is to convert our audio files from dual channel to mono channel, which is accomplished by taking the average of the two channels.  The figure below shows the spectrogram we obtain:

![_config.yml]({{ site.baseurl }}/images/Initialial_img.PNG)

After the initial preprocessing, we also max-normalize the signal data by time and downsample the data.

We use STFT transform function to create a time-frequency representation of our data. Briefly, the STFT transform is used to compute the sinusoidal frequency and phase content of local sections of a signal as it changes over time. The idea here is to firstly divide the input spectra into equally sized smaller intervals. Then a Fourier transform is applied to each one of them, the resulting spectrogram is visualized below:
![_config.yml]({{ site.baseurl }}/images/STFT transform.PNG)


## Model Description

Visually the architecture we use is:

![_config.yml]({{ site.baseurl }}/images/source_sep_model_updated.PNG)

The masking layer is a technique by which the probabilities from each dense layer for each source are used to apply a mask to the initial spectrogram to transform the spectrogram such that the source based on which the mask is applied is predominantly represented.

Finally, an inserve stft transform is applied to convert the masked spectrogram which a time-frequency magnitude representation back to audio signals, which we store in .wav file. 

Below we show a visual captured from the training of the network:
![_config.yml]({{ site.baseurl }}/images/source_sep_loss_curve.PNG)

We also experimented with different model architectures, with different hyperparameters such as number of iterations, number of LSTM layers, but did not see much variations in the training process from our current specified architecture.

The model achieves separating the outputs into two parts, separating by each instrument is adds several layers of complexity in terms of preprocessing, identifying ground truth labels, defining the loss function etc,  and hence is a direction for future work.


## Dataset
We use the MIR-1K dataset (MIR stands for Music information retrieval). 
It comprises of a variety of songs, a total of 1000 musical clips. This dataset is annotated with several musical features such as pitch, timbre, etc. We use this dataset since it is a popular dataset used in source separation literature as it captures various aspects of music and hence forms a very powerful dataset to be used for training. 
We use to the .wav files (audio signals) to build and test our model.



## Training and setup

## Results
(output visualization)

## References
