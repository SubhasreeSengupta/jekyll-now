---
layout: post
title: KEEN QUARTET'S PROJECT
---

## Preprocessing 

The figure below shows the spectrogram of an audio file we used in our dataset.
Spectograms are a common way of visualizing audio signals. The x-axis represents time, y-axis represents frequency of sounds at that time. The different intensities of colors are used to express a 3rd dimension - amplitude corresponding to a particular frequency.

The very first preprocessing step is to convert our audio files from dual channel to mono channel, which is accomplished by taking the average of the two channels. 


![_config.yml]({{ site.baseurl }}/images/Initialial_img.PNG)


After the initial preprocessing, we also max-normalize the signal data by time and downsample the data.

We use STFT transform function to create a time-frequency representation of our data. This transformation was used for our experiments on Source separation.

![_config.yml]({{ site.baseurl }}/images/STFT transform.PNG)

CQT transform is used in our experiments for automatic transcription.

![_config.yml]({{ site.baseurl }}/images/CQT_transform.PNG)


