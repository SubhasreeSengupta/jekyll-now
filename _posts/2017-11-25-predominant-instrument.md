---
layout: post
title: Predominant Instrument Identification 
---

The goal of our project is music transcription for multiple instruments. This suggests that we need to transcribe for each of the instruments involved. For eg, if we have a music file which is a combination of piano, cello, trumpet, guitar, we need to transcribe for each of these. Hence we complete the source segregation to obtain approximate individual music pieces for each instrument. Next block is for predominant music instrument identification.

In our setup, we use the analogy that even music can be represented as image and a **Convolutional Neural Network (CNN)** can be used to learn the pattern in the music. The timbral properties can be captured and are needed to identify the predominant instrument in the music piece. Our CNN model takes the spectrogram images generated from after preprocessing the audio files.

Here are the steps that are involved for this task.

## Preprocessing
The steps involved to process the audio files (.wav format) are as follows:
- The stereo input is converted to mono by taking the mean of the two channels.
+ The input is downsampled from 44k to 22k (helps in removing noises possibly included in the range above this)
- Normalize the audio file
+ Convert it to a time-frequency representation using STFT
- Convert this into mel-scale and thus we obtain spectrogram which is given as input to the CNN

The images were organized in folders according to the instrument, whereas we needed labels with instruments. So we need more processing of these images.
- Convert into a grayscale image. This refrains the model to learn unnecessary information. It is supposed to learn the patterns in the image and color does not give any information about the audio files.
+ Reshape the mel-spectogram to time x Number of filters x frequency (1 x 43 x 128)
_ Assign labels to each image based on the folder they belong
+ divide the dataset into test, train, validation.


## Dataset
We use **IRMAS** dataset to train our CNN model. It has musical excerpts with annotations of the predominant instrument present and is intended to be used for the automatic identification of the predominant instrument in music.   

The dataset is divided into training data and testing data. Training data has 6705 audio files with each file of 3 sec excerpt.
The annotations are for 11 pitched instruments. The testing data has 2874 audio files with lengths between 5 sec and 20 sec. These testing files have one or more target labels.

## Model Description
![_config.yml]({{ site.baseurl }}/images/predominant_inst_model.png)

The above figure describes the architecture that we use.

#### Addition of the batch normalization layer helped us in improving the accuracy.

## Training and Setup
- We have trained the CNN model on the GCP GPU instance (configuration: 1 Nvidia Tesla K80 GPU, 8 vCPUs, 52 GB RAM, 100 GB Memory)..
+ The validation accuracy steadily increased with the increase in the number of epochs trained. We initially observed a 15% accuracy after training the model for 10 epochs (which is as good as guessing the instrument) and the accuracy improved constantly later on with more training. 
- We realized the size of the image is larger compared to normal images. Hence we trained the model for 150 epochs with early stopping and a patience of 5. The training early stopped after 70 epochs with a test accuracy of ~60%.
+ We used Tensorboard for visualization of the loss curves, accuracy curves. As part of training setup we saved checkpoints after every epoch, which helps us to resume in case of failure and for early stopping.


## Results
Here are the results obtained during the training of the model.
![_config.yml]({{ site.baseurl }}/images/after_epoch_90.png)
![_config.yml]({{ site.baseurl }}/images/after_epoch_140.png)


## References

1. Han, Yoonchang, et al. "Deep convolutional neural networks for predominant instrument recognition in polyphonic music." IEEE/ACM Transactions on Audio, Speech and Language Processing (TASLP) 25.1 (2017): 208-221.

2. IRMAS Dataset: 

Bosch, J. J., Janer, J., Fuhrmann, F., & Herrera, P. “A Comparison of Sound Segregation Techniques for Predominant Instrument Recognition in Musical Audio Signals”, in Proc. ISMIR (pp. 559-564), 2012.

