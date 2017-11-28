---
layout: post
title: Musical Source Separation
---

Below we present a detailed overview of our approach for source separation of a musical piece. This part is divided into the following sections: First, we describe the pre-processing steps needed to feed the data into the model.  Next, we describe the model architecture and the details of the approach. Finally, we mention some evaluation metrics for our method and the performance of our model.
We build upon the paper (1) and it forms the main backbone of our approach. However, we improve upon the existing model by using LSTMs layers in the model architecture. We further modify the loss such that it also accounts for increasing the difference between the sources. 

## Preprecessing

The figure below shows the spectrogram of an audio file we used in our dataset.
Spectograms are a common way of visualizing audio signals. The x-axis represents time, y-axis represents frequency of sounds at that time. The different intensities of colors are used to express the 3rd dimension - amplitude corresponding to a particular frequency.

The very first preprocessing step is to convert our audio files from dual channel to mono channel, which is obtained by taking the average of the two channels. The figure below shows the spectrogram we obtain:

![_config.yml]({{ site.baseurl }}/images/Initialial_img.PNG)

After the initial preprocessing, we also downsample the signal data and then max-normalize the signal data by time.

We use STFT transform function to create a time-frequency representation of our data. Briefly, the STFT transformation is used to compute the sinusoidal frequency and phase content of local sections of a signal as it changes over time. The idea here is to firstly divide the input spectra into equally sized smaller intervals. Then a Fourier transform is applied to each one of them, thus resulting spectrogram is visualized below:
![_config.yml]({{ site.baseurl }}/images/STFT transform.PNG)


## Model Description

Visually the architecture we use is:

![_config.yml]({{ site.baseurl }}/images/source_sep_model_updated.PNG)

The intuition behind using an LSTM is two-fold.
 1. It can capture the neighbouring features in the input spectra. 
 2. It has been shown to be more stable than regular RNNs. Since the pre-processing above transforms the data in a sequence of time frames, that further validates the use of LSTMs, to capture the effect of time-sequencing.

We apply 3 LSTMs layers, with number of hidden units = 1000. The number of layers and numbers of hidden units are the hyperparameters for our model. We obtained the best results with these values of hyperparameters after fine tuning them.

The LSTM layers are followed by two dense layers. These are used to identify each source out of the mixed spectra. The activation function used for the dense layers is **ReLU**. 

We then have a masking layer which helps to smoothen the output for each source obtained from each dense layer. Mathematically, this is expressed as: 

![_config.yml]({{ site.baseurl }}/images/Masking_eq.PNG)

 Here z_t represents the input spectrogram and yhat_1t and yhat_2t are the predictions for the output spectrogram from the neural network. The y^tilde values are the final separated source values, the masking helps in extracting each component out by masking out the values for the other source.
 
 The loss function corresponds to applying MSE+discriminative training. Mathematically, this is expressed as:
 
 ![_config.yml]({{ site.baseurl }}/images/source_sep_loss_func.PNG)
 
 The 1st and the 3rd terms represent the MSE error between the predicted and the actual output and the 2nd and the 4th term are the discrimative learning terms to increase the difference between the predicted sources with respect to the actual sources.
 
 We set **gamma** to be **0.5**, which is again a model choice.
 
 The main highlight of this framework is the fact that we not only train w.r.t to the parameters of the model but also w.r.t. the masking layer. This is practically very useful, since our final objective is to improve the quality of the masked output and not the individual predictions for each source.
 
 The actual output against which we train(represented above as y_1t, y_2t), represents the two channels in the input dual channel audio file.
 
Finally, an inverse STFT transform is applied to convert the masked spectrogram which a time-frequency magnitude representation back to audio signals, which we store in .wav file. This acts as input for the transcription model. 

Shown below is a visual captured from the training of the network:
![_config.yml]({{ site.baseurl }}/images/source_sep_loss_curve.PNG)

We also experimented with different model architectures, with different hyperparameters such as number of iterations, number of LSTM layers, but did not see much variations in the training process from our current specified architecture.

The model achieves separating the music file into two parts: voice and instrument. Separating the input into each instrument adds several layers of complexity in terms of preprocessing, identifying ground truth labels, defining the loss function, etc. and hence is a direction for future work.

## Dataset
We use the MIR-1K dataset (MIR-Music information retrieval). 
It comprises of a variety of songs, a total of 1000 musical clips (hence 1K in the name of dataset). This dataset is annotated with several musical features such as pitch, timbre, etc. We use this dataset since it is a popular dataset used in source separation literature. It captures various aspects of music and hence forms a very powerful dataset to be used for training for source separation. We needed a dataset which was rich and diverse in vocal and instrument audio signals and hence we used this dataset.   
Since our target transcription is for piano only, we use datasets for evaluation which have vocal and only piano as the background instrument.


## Results
Below we visualization of the two source separated components:
1. Original spectrogram 

![_config.yml]({{ site.baseurl }}/images/original_spec.PNG)

2. Only-voice spectrogram

![_config.yml]({{ site.baseurl }}/images/voice_spec.PNG)

3. Only-music spectrogram

![_config.yml]({{ site.baseurl }}/images/music_spec.PNG)

## References
