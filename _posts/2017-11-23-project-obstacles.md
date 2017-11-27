---
layout: post
title: Obstacles faced during the project
---


# Project Setup

Here is the description of the basic setup that we used.

## Google Cloud instance: 
Set up a gpu instance and set up tensorflow other dependencies by installing from sources. This improves the performance of the GPU. We were able to achieve a speedup of about 20 times compared to the that on the machine.
It took around 10 hours to set this as we were doing it for the first time.

## AWS:
We used g2.2xlarge, but that gave us terrible results and not much difference in performance. So we discontinued using it.

To avoid the issues due to dependencies, we started using Ubuntu on local machine also.

Also, the files that were processed were very huge, so we faced memory issues, which forced us to use 52 GB RAm, 100 GB storage space, leading to more powerful and more costly GPU instance.

# Obstacles faced during the project

## Overall issues:

Realized that preprocessing takes a huge time.
Our project is divided in three components. The papers that we referenced for each component used different datasets, different techniques for preprocesing, which was very problematic if we try to pipeline the individual blocks.

## Preprocessing step:

As the music domain was very new for us, we had to learn and understand different ways of preprocessing. Also, we tried to train the model on large dataset. We used MAPS dataset which has recordings of about 45 GB. This had files which were whole music pieces. We divided the music files in time frames. We processed around 6 million time frames. This took around 4-5 hours.

## Training:

We trained the models for all the 3 components. Each one had their own complexities. It around 10 hours for the first part (source separation), 7 hours for the middle block (predominant music identification), 12 hours for the third block (end-to-end approach).

Again we did this multiple times trying on different datasets.

## Transcription:

Transcription seems to be an easy step as all we do is one-to-one mapping.
But when we have polyphonic music generating transcription based on the learning, it becomes more complicated to infer the results.
