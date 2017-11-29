---
layout: post
title: Automatic Music Transcription for Polyphonic Music For Multiple Instruments
---

# Project Objective
Music is an integral part of human life. It has its own charm and magic. Yet, interestingly, there is complex mathematical structure embedded in music. Many researchers and scientists have been working on different aspects of music, like learning the positive psychological effects of music, identifying the underlying musical style in a musical piece, just to name a few. Deep Learning has been successful in areas such as computer vision, natural language processing. It has performed better than humans in tasks like image classification, object recognition and many more. Given that there exists a mathematical framework embedded in music, attempts have been made by researchers to use the power of deep learning in this domain for a variety of tasks such as predicting song similarity, composer identification, song recommendation and even music generation.

In our project, we would like to address the problem of **“Automatic Music Transcription For Polyphonic Music Involving Multiple Instruments”**. Automatic Music Transcription (AMT) is an open problem in Music Information Retrieval(MIR). AMT aims to generate a symbolic representation, a near-score like transcription, given a polyphonic musical piece. This is a difficult problem for the following reasons:
- In polyphonic music, there exists a complex interaction and overlap of harmonies arising from different acoustic signals in the melody.
+ Separating the sources of music in a given piece with multiple instruments is itself a very challenging task. So AMT fails to match human performance. 

We wish to use deep learning to address these challenges and successfully achieve the mentioned aim.

# Project Pipeline

![_config.yml]({{ site.baseurl }}/images/Project_pipeline.PNG)


In the sections below we describe each of the 3 main steps of the pipeline:
1. Separation of the sources in the given music piece
2. Identifying the predominant instrument in each of these pieces
3. Transcribe the music for the corresponding instrument based on the predominant instrument identification output

# Musical Source Segregation
In order to make an end to end approach, we need to focus on filtering each musical source out of any given music piece. This is an ongoing area of research and is extremely difficult given:
- We need to have ground truth labels for each source.
+ We need to tune the loss function in such a way that it accounts for increasing the difference among each instrument at the time it is recognizing it. This process is also called _Discriminative Training_. 

The motivation of this approach is to primarily segregate a musical piece into every source present. However, in order to accurately train the model for source separation, we require a massive annotated corpus with varied musical features. Hence, for the purposes of this project, we address separating a given music piece into two musical sources. Specifically, we focus on separating vocal and instrument audio signals from a musical piece to obtain a better instrumental input for the downstream model. 

We build upon the approach presented in (1). We modified the model architecture, by using an **LSTM based approach** instead of the RNN based approach in the paper, which gives us better results. Additionally, we design the loss function such that it accounts for increasing the difference between the two sources and incentivize the segregation (discriminative learning).

We use the MIR-1K dataset for training the model. The output of the trained model are two separated files, corresponding to the vocal and instrument components. The file containing the musical components corresponding to the instruments and is used as input to evaluate the next stages of the project pipeline. 

Here we provide visualizations of the output of our approach:

1. Spectrogram for original audio file
   ![_config.yml]({{ site.baseurl }}/images/original_spec.PNG)
   
2. Spectrogram for Only-music audio file
  ![_config.yml]({{ site.baseurl }}/images/music_spec.PNG)

3. Spectrogram for Only-voice audio file
  ![_config.yml]({{ site.baseurl }}/images/voice_spec.PNG)


These visuals demonstrate the effectiveness of our approach. By visual inspection we can see a marked difference between the original spectrogram and the spectrogram corresponding to that of the only-voice audio file. The same can be seen for the spectrogram corresponding to that of the only-music audio file.  

A direction of future work to adapt/modify the model architecture such that it can not only segregate vocals and instruments but also can separate between instruments as well. 

Please find the detailed description of this step (including preprocessing, model, results) [here](https://subhasreesengupta.github.io/source-separation/)

 
# Predominant instrument classification
Once we have the segregated inputs based on the instruments, we need to identify the predominant source for each. This is the second step of the pipeline that is required to avoid loss of information in the first step and also to reassure the classification. In our setup, the model has been trained to identify 11 instruments. We give a .wav file as an input to the model and get the label of the predominant instrument present in the music file.

**We were able to get 60% test accuracy on the identification of predominant instrument.**

The images of the loss functions are here.

The graph below shows the decrese in test loss and validation loss.
![_config.yml]({{ site.baseurl }}/images/decrease_loss.png)
![_config.yml]({{ site.baseurl }}/images/val_loss_decrease.png)

The graph below shows the improvement in test and validation accuracies.
![_config.yml]({{ site.baseurl }}/images/acc_increase_curve.png)
![_config.yml]({{ site.baseurl }}/images/val_acc_improvement.png)

[Here](https://subhasreesengupta.github.io/predominant-instrument/) is the link that describes all the model and complete details of this step.

# Automatic Transcription for Polyphonic Music
As part of our final step, we transcribe the individual instruments to generate musical notes. The source segregated input files are fed into the models trained for the particular instrument identified as the predominant instrument in the previous step.

In our setup, we have trained a **ConvNet model** to transcribe the polyphonic piano music. The model takes the piano identified music as input and generates piano roll notation for the same. We evauated our model on the **MAPS** (MIDI Aligned Piano Sounds) dataset. The overall size of the database is about 40GB, i.e. about 65 hours of audio recordings.

The combinatorially large output space is one of the challenging problems to be tackled in this step. The variability in the pitch and the timbral properties of the instrument is captured by the ConvNet model and is used to identify the notes at each time frame.

We were able to preprocess, train and evaluate many approaches to obtain the transcription for the polyphonic music.

The architecture, dataset, and details of other steps are [here](https://subhasreesengupta.github.io/end-to-end-approach/)

# Conclusion

In this project, our main goal was to achieve _Automatic transcription for multiple instruments using deep learning approach_. We divided this task into three sub-tasks.   
The first was to separate out a musical mixture (real music) into each source. We focused on an approach to separate two musical sources, specifically vocals and background instruments. We presented an LSTM based model, with a  discriminative loss function specially designed to achieve this objective.   
Next we presented a ConvNET approach to identify the predominant instrument in the music piece obtained sfter source segregation. The purpose of this part was to aid in deciding which transcription model should be used for a musical piece based on the corresponding predominant instrument.  
Finally, we presented a ConvNET based approach for transcribing piano music.   
For each section, we train the models on different datasets, tune the hyperparameters, and try to achieve the best possible results for each part.  
In the next section, we discuss some of the future directions of this project, to transform it into the product orginally envisioned.

# Future Work

There are several directions of future work.  
One, would be to improve upon the source separation model to extend it to identify other sources as well. This would entail a different model architecture and a different loss function for each source (instrument).  
Another direction would be make a flexible transcription model, that can transcribe for any musical source.   
Both these extensions are highly complex and hence are beyond the scope of our current project objective.

