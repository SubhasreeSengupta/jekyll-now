
---
layout: post
title: Predominant Instrument Identification 
---

# Predominant Instrument Identification

The goal of our project is music transcription for multiple instruments. This suggests that we need to transcribe for each of the instruments involved. For eg, if we have a music file which is a combination of piano, trumpet, guitar, we need to transcribe for each of these. That is why we did the source separation, to obtain individual music pieces for each instrument. Next block is for predominant music identification.

Once we have the individual music files, we need to identify which instrument does it belong to, so that we can transcribe accordingly. So, in this step, we try to find out the predominant music iunstrument in the music file.
We check this for each file obtained as an output from the source separation part. we get the label of the predominant instrument as the output.

We use the analogy that even music can be represented as image and the n CNN can be used to learn the pattern and identify the music. For this we need to preprocess audio files and generate images out of that. 
Here are the steps that are involved for this task.

## Preprecessing
The steps involved in the  aduio file (.wav format) are as follows:
- The stereo input is converted to mono by taking the mean of the two channels.
+ the input is downsampled from 44k to 22k (helps in removing noises possibly included in the range above this)
- Normalize the audio file
+ Convert it to a time-frequency representation using STFT
- Convert this into mel-scale and thus we obtain sprctrogram which is given as input to the CNN

## Dataset
We use IRMAS dataset to train this CNN model. It has musical excerpts with annotations of the predominant instrument present and is intended to be used for the automatic identification of the predominant instrument in music.

The dataset is divided into training data and testing data.
Training data has 6705 audio files each is 3 sec excerpt.
The annotations are for 11 pitched instruments
The testing part has 2874 audio files with lengths between 5 sec and 20 sec. These testing files had one or more target labels.

## Model Description
We use a CNN model. The architecture is mentioned in the paper cited below. 
![_config.yml]({{ site.baseurl }}/images/predominant_inst_model.png)


## Training and setup
We trained the model for 10 epochs initially and we got 15% accuracy (which is as good as guessing the instrument). But we observed the accuracy improved constantly.
Then we realized the size of the image is larger compaerd to normal images, so we trained the model for 150 epochs with early stopping. The accuracy thus obtained is ~60%.

## Results
Here are the results obtained during the training of the model.
![_config.yml]({{ site.baseurl }}/images/after_epoch_90.png)
![_config.yml]({{ site.baseurl }}/images/after_epoch_140.png)


## References

For this sectione, we referred to the paper "Deep convolutional neural networks for predominant
instrument recognition in polyphonic music". We tried to implement this paper on our own. [Here](https://arxiv.org/pdf/1605.09507.pdf) is the link to the paper.
