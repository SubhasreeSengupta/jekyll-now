
In this project, our main goal was to achieve _Automatic transcription for multiple instruments using deep learning approach_. We divided this task into three sub-tasks.   
The first was to separate out a musical mixture (real music) into each source. We focused on an approach to separate two musical sources, specifically vocals and background instruments. We presented an LSTM based model, with a  discriminative loss function specially designed to achieve this objective.   
Next we presented a ConvNET approach to identify the predominant instrument in the music piece obtained sfter source segregation. The purpose of this part was to aid in deciding which transcription model should be used for a musical piece based on the corresponding predominant instrument.  
Finally, we presented a ConvNET based approach for transcribing piano music.   
For each section, we train the models on different datasets, tune the hyperparameters, and try to achieve the best possible results for each part.  
In the next section, we discuss some of the future directions of this project, to transform it into the product orginally envisioned.

## Future Work

There are several directions of future work.  
One, would be to improve upon the source separation model to extend it to identify other sources as well. This would entail a different model architecture and a different loss function for each source (instrument).  
Another direction would be make a flexible transcription model, that can transcribe for any musical source.   
Both, these extensions are highly complex and hence are beyond the scope of our current project objective.
