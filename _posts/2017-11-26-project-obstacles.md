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

<br>To avoid the issues due to dependencies, we started using Ubuntu on local machine also.

# Obstacles faced during the project

## Overall issues

Realized that preprocessing takes a huge time.
Our project is divided in three components and each the papers we used as reference 





