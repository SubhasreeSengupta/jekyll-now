---
layout: post
title: Obstacles faced during the project
---


# Project Setup

Here is the description of the basic setup that we used.

## Google Cloud instance: 
Set up a gpu instance and set up tensorflow other dependencies by installing from sources. This improves the performance of the GPU. We were able to achieve a speedup of about 20 times compared to the that on other compute instances.

## AWS:
We used **g2.2xlarge** instance, but that was not helpful while we trained and preprocessed large datasets. There was not much difference in performance.

To avoid the issues due to dependencies, we started using Ubuntu on local machine also.

Also, the files that were processed were very huge, so we faced memory issues, which forced us to use 52 GB RAm, 100 GB storage space, leading to more powerful and more costly GPU instance.

# Obstacles faced during the project

## Overall issues:

**Availability of Dataset**: It was not easy to access well annotated, clean corpus of data to train our models.
**Preprocessing** of the input is the major task of the project. We realized that preprocessing takes a lot of time.
Our project is divided in three components. The papers that we referenced for each component used varied datasets and different techniques for preprocesing. This proved to be challenging to connect the pipeline and evaluate the results. 

## Preprocessing step:

As the music domain was very new for us, we had to learn and understand different ways of preprocessing. Also, we tried to train the model on large dataset. We used MAPS dataset which has recordings of about 45 GB. This had files which were whole music pieces. We divided the music files in time frames. We processed around 6 million time frames. This took around 4-5 hours.

## Training:

We trained the models for all the 3 components on GPU instances. Each one had their own complexities. It around 10 hours for the first part (source separation), 7 hours for the middle block (predominant music identification), 12 hours for the third block (end-to-end transcription).

Again we did this multiple times trying on different datasets.

## Transcription:

Transcription of polyphonic music is a complex task due to the combinatorially large output space. As the output corresponds to high-dimensional vectors for each time frame for a varied length piece, this is computationally intensive. The inference of the trained results to map them to the notes at each time frame involves multiple post processing steps. We tried multiple ways to convert the probabilistic sigmoid output of the fully connected layer like threshold calculation to classify the notes for each time frame during training. 

