---
layout: post
title: Automatic Music Transcription for Polyphonic Music for Multiple Instruments
---

# Project Objective
Music is an integral part of human life. It has its own charm and magic. Yet, interestingly, there is complex mathematical structure embedded in music. Many researchers and scientists have been working on different aspects of music, like learning the positive psychological effects of music, identifying the underlying musical style in a musical piece, just to name a few. Deep Learning has been successful in areas such as computer vision, natural language processing. It has performed better than humans in tasks like image classification, object recognition and many more. Given that there exists a mathematical framework embedded in music, attempts have been made by researchers to use the power of deep learning in this domain for a variety of tasks such as predicting song similarity, composer identification, song recommendation and even music generation.

In our project, we would like to address the problem of “Automatic Music Transcription for Polyphonic Music involving Multiple Instruments”. Automatic Music Transcription (AMT) is an open problem in Music Information Retrieval(MIR). AMT aims to generate a symbolic representation, a near-score like transcription, given a polyphonic musical piece. This is a difficult problem for the following reasons:
- In polyphonic music, there exists a complex interaction and overlap of harmonies arising from different acoustic signals in the melody.
- Seperating the sources of music in a given piece with multiple instruments is itself a very challanging task. So AMT fails to match human performance. 

We wish to use deep learning to address these challenges and succesfully achieve the mentioned aim.

In order to transcribe each instrument in a musical piece we first addressed the challenge of musical source separation. Next, we design a "predominant instrument classifier", the goal of which is to identify the predominant instrument in each musical piece. Thus, we comet o know the instrument in the given music piece. Once we know this, we give the corresponding wav file as input to the end-to-end CONVNET based transcription model trained for the identified predominant instrument. As a result, we will be able to transcribe the given music file, which is the ultimate goal of our project.

Looking into the last part, we need to train model according to the instruents. This requires datasets for each instrument for training, also, it the training is resouce intensive. Given the time and resource constraints, we currently focus on transcription of Piano music. But the pipeline proposed can be adapted to give results for multiple instruments. 

# Project Pipeline

![_config.yml]({{ site.baseurl }}/images/Project_pipeline.PNG)


In the sections below we describe each of the 3 main steps of the pipeline:
1. Separation of the sources in the given music file
2. Identifying the predominant instrument in each of these files
3. Do transcription for each file based on the labels obtained for them 


# Musical Source Segregation:
In order to make an end to end approach, we need to focus on filtering each musical source out of any given music piece. This is an ongoing area of research and is extremely difficult given:
- We need to have ground truth labels for each source.
+ We need to tune the loss function in such a way that the it accounts for increasing the difference among each instrument as the time it is recongizing it. This process is also called discriminative training. 

The motivation of this approach is primarily to be able to segregate each and every musical source in a musical piece. However in order to do again we will need a massive corpora that has various musical features for the source separation to be trained accurately. Hence, for the purposes for this project we adddress separating two musical sources out of a musical piece. Specifically, we focus on separating vocal and instrument audio signals from a musical piece. 
We build upon the approach presented in (1). We modify the model architecture, by using an LSTM based approach instead of the RNN based approach in the paper, which gives us better results. Additionally, we design the loss function such that it accounts for increasing the difference between the two sources.

We use the MIR-1K dataset for training the model. The output of the trained model are two separated files, corresponding to the vocal and instrument components separately. The file containing the musical components corresponding to the instruments is used as input to evaluate the next stages of the project pipeline. 

Below, is a plot of the 1st 500 iterations of training the network:

![_config.yml]({{ site.baseurl }}/images/source_sep_loss_curve.PNG)

A direction of future work to adapt/modify the model architecture such that it can not only segragate vocals and instruments but also can separted between instruments as well. 

Please find the detailed description of this step (including preprocessing, model, results) [here](https://subhasreesengupta.github.io/source-separation/)

 
# Predominant instrument classification:
In order to address the problem of transcription of music files involving multiple instruments, it is higly necessary to separate the sources which is complicated task in itself. Once we have the separated files, we need to identify the predominant source for each. this is the second step of the pipeline. Here we give a .wav file as an input to the model. TH model has been trained to identify 11 instruments. We get label of the predominant instrument present in the music file.

We were able to get 60% accuracy on identification of predominant instrument. 
The images of the loss functions are here.

The graph below shows the decrese in test loss and validation loss.
![_config.yml]({{ site.baseurl }}/images/decrease_loss.png)
![_config.yml]({{ site.baseurl }}/images/val_loss_decrease.png)

The graph below shows the improvement in test and validation accuracies.
![_config.yml]({{ site.baseurl }}/images/acc_increase_curve.png)
![_config.yml]({{ site.baseurl }}/images/val_acc_improvement.png)

[Here](https://subhasreesengupta.github.io/predominant-instrument/) is the link that describes all the model and complete details of this step.

# Automatic transcription

<<1 para briefing and visuzalization>>


The architecture, dataset, and details of other steps are [here](https://subhasreesengupta.github.io/end-to-end-approach/)

