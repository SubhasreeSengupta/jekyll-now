
---
layout: post
title: Source Separation
---

# Source Separation


## Preprecessing

The figure below shows the spectrogram of an audio file we used in our dataset.
Spectograms are a common way of visualizing audio signals. The x-axis represents time, y-axis represents frequency of sounds at that time. The different intensities of colors are used to express a 3rd dimension - amplitude corresponding to a particular frequency.

The very first preprocessing step is to convert our audio files from dual channel to mono channel, which is accomplished by taking the average of the two channels. 

![_config.yml]({{ site.baseurl }}/images/Initialial_img.PNG)

After the initial preprocessing, we also max-normalize the signal data by time and downsample the data.

We use STFT transform function to create a time-frequency representation of our data. 
![_config.yml]({{ site.baseurl }}/images/STFT transform.PNG)

## Dataset
We use the MIR-1K dataset (MIR stands for Music information retrieval). 
It comprises of a variety of songs, a total of 1000 musical clips. This dataset is annotated with several musical features such as pitch, timbre, etc. We use this dataset since it is a popular dataset used in source separation literature as it captures various aspects of music and hence forms a very powerful dataset to be used for training. 
We use to the .wav files (audio signals) to build and test our model.

## Model Description

Our approach is based on the paper : TO-DO insert paper link. 
We use an LSTM based model architecture. Visually the architecture we use is:

![_config.yml]({{ site.baseurl }}/images/Source_sep_model.PNG)

The masking layer is a technique by which the probabilities from each dense layer for each source are used to apply a mask to the initial spectrogram to transform the spectrogram such that the source based on which the mask is applied is predominantly represented.

Finally, an inserve stft transform is applied to convert the masked spectrogram which a time-frequency magnitude representation back to audio signals, which we store in .wav file. 

Below we show a visual captured from the training of the network:
![_config.yml]({{ site.baseurl }}/images/source_sep_loss.PNG)

We also experimented with different model architectures, with different hyperparameters such as number of iterations, number of LSTM layers, but did not see much variations in the training process from our current specified architecture.

The model achieves separating the outputs into two parts, separating by each instrument is adds several layers of complexity in terms of preprocessing, identifying ground truth labels, defining the loss function etc,  and hence is a direction for future work.


<<<Also include table with parameter values>>>

## Training and setup

## Results
(output visualization)

## References
