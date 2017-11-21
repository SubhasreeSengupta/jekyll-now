---
layout: post
title: Automatic music transcription for polyphonic music instruments.
---
## Project Objective
Our project mainly aims at automatic music transcription. Specifically we focus on transcription of Piano music. Since we wanted to design a fully functional tool, that when given any audio file as input can then transcribe the piano music automatically. In order to make an end to end approach, we also need to focus on filtering the piano music out of any given music piece. This part is analogous to source separation in music, which is also an ongoing area of research and is extremely difficult, as you need to have labels corresponding to each instrument and also tune the loss function in such a way that the loss function also accounts for increasing the difference among each instrument at the same time as recongizing it. We investigated approaches that can separate out vocals, drums and bass instruments out of a musical mixture. Parallely, we also visualized this source segregation objective as identifying the predominant instrument in a mixture. Although we focus on piano transcription, the approach can be adapted for any instrument as long as we have a single instrument we want to transcribe for which the predominant instrument identification task is extremely crucial. 

The the following sections, we describe and visualize several aspects of our project.

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



