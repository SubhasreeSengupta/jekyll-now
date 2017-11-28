---
layout: post
title: Musical Source Separation
---

Below we present a detailed overview of our approach for source separation of a musical piece. This part is divided into the following sections: First we describe the pre-processing steps needed to feed the data into the model.  Next we describe the model architecture and the details of the approach. Finally, we mention some evaluation metrics for our method and the performance of our model.
We build upon the paper (1) and it forms the main backbone of our approach. However, we improve upon the existing model by using LSTMs layers in the model architecture. We further modify the loss such that it also accounts for increasing the difference between the sources. 

## Preprecessing

The figure below shows the spectrogram of an audio file we used in our dataset.
Spectograms are a common way of visualizing audio signals. The x-axis represents time, y-axis represents frequency of sounds at that time. The different intensities of colors are used to express a 3rd dimension - amplitude corresponding to a particular frequency.

The very first preprocessing step is to convert our audio files from dual channel to mono channel, which is accomplished by taking the average of the two channels.  The figure below shows the spectrogram we obtain:

![_config.yml]({{ site.baseurl }}/images/Initialial_img.PNG)

After the initial preprocessing, we also max-normalize the signal data by time and downsample the data.

We use STFT transform function to create a time-frequency representation of our data. Briefly, the STFT transform is used to compute the sinusoidal frequency and phase content of local sections of a signal as it changes over time. The idea here is to firstly divide the input spectra into equally sized smaller intervals. Then a Fourier transform is applied to each one of them, the resulting spectrogram is visualized below:
![_config.yml]({{ site.baseurl }}/images/STFT transform.PNG)


## Model Description

Visually the architecture we use is:

![_config.yml]({{ site.baseurl }}/images/source_sep_model_updated.PNG)

The intuition behind using an LSTM is two-fold. First, it can capture the neighbouring features in the input spectra. Second, it has been shown to be more stable than regular RNNs. Since, the pre-processing above, transforms the data in a sequence of time frames that further validates the use of RNNs, to capture the effect of time-sequencing.

We apply 3 LSTMs layers, with number of hidden units = 1000. These are hyerparameters in our model and are user chosen. We tested with other values of hyperparameters but the above configuration gave the best results. 

The two dense layers are used to identify each source out of the mixed spectra. The activation function used for the dense layers is Relu. 

The application of the masking layer is to smooth out the output for each source obtained from each dense layer. Mathematically, this is expressed as : 

![_config.yml]({{ site.baseurl }}/images/Masking_eq.PNG)

 z_t represents the spectrogram which is the input and y_1 and y_2 are the predictions for the output spectrogram. The y_hat values are the final separated source values, the masking helps in extracting each component out by masking out the values for the other source.
 
 The loss function correspondings to applying MSE+discriminative training. Mathematically, this is expressed as:
 
 ![_config.yml]({{ site.baseurl }}/images/source_sep_loss_func.PNG)
 
 The 1st and the third terms represent the MSE error between the predicted and the actual output and the 2nd and the 4th term are the discrimative terms to increase the difference between the predicted sources with respect to the actual sources.
 
 We set gamma to be **0.5**, which is again a model choice.
 
 The main highlight of this framework is the fact we not only train w.r.t to the parameters of the model but also with respect the masking layer. This is very practically useful, since our final objective is to improve the quality of the masked output and not the individual predictions for each source.
 
 The actual output (represented above as y_1,y_2) represent the two channels in the input dual channel audio file.
 
Finally, an inserve stft transform is applied to convert the masked spectrogram which a time-frequency magnitude representation back to audio signals, which we store in .wav file and which forms the input which we feed into the transcription model. 

Below we show a visual captured from the training of the network:
![_config.yml]({{ site.baseurl }}/images/source_sep_loss_curve.PNG)

We also experimented with different model architectures, with different hyperparameters such as number of iterations, number of LSTM layers, but did not see much variations in the training process from our current specified architecture.

The model achieves separating the outputs into two parts, separating by each instrument is adds several layers of complexity in terms of preprocessing, identifying ground truth labels, defining the loss function etc,  and hence is a direction for future work.


## Dataset
We use the MIR-1K dataset (MIR stands for Music information retrieval). 
It comprises of a variety of songs, a total of 1000 musical clips. This dataset is annotated with several musical features such as pitch, timbre, etc. We use this dataset since it is a popular dataset used in source separation literature as it captures various aspects of music and hence forms a very powerful dataset to be used for training for source separation. We needed a dataset which was rich and diverse in vocal and instrument audio signals and hence we used this dataset. 
Since our target transcription is for piano only we use datasets, which have vocal and only piano as the background instruments.


## Results
Below we visualization of the two source separated components:
1. Original spectrogram 

![_config.yml]({{ site.baseurl }}/images/original_spec.PNG)

2. Only-voice spectrogram

![_config.yml]({{ site.baseurl }}/images/voice_spec.PNG)

3. Only-music spectrogram

![_config.yml]({{ site.baseurl }}/images/music_spec.PNG)






## References
