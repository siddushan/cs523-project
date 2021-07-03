# Music Generation Using Magenta 
By: Junyu Liu, Sidd Shanmugam, Minglan Zheng

## Introduction
This project reproduces results stemming from [this](https://arxiv.org/pdf/1803.05428.pdf) paper written about hierarchical latent vector models for long-term structure in music. The paper resulted in the [Magenta Project](https://magenta.tensorflow.org/)'s VAE models which contains several models including [MusicVAE](https://github.com/magenta/magenta/tree/main/magenta/models/music_vae#musicvae-a-hierarchical-recurrent-variational-autoencoder-for-music) and [GrooVAE](https://github.com/magenta/magenta/tree/main/magenta/models/music_vae#groovae). 

The slides used to present our reproduction of this work can be found [here](https://docs.google.com/presentation/d/1eq6TXIcUN9CCoQ9xSQ6d6CwnDIS0M2twH8SrhgaBlL8/edit?usp=sharing)

## Documentation
* [Data preparation](#data-preparation)
* [Instructions and Results](#instructions-and-results)
  - [Training flat piano](#training-flat-piano)
  - [Training hierdec-piano](#training-hierdec-piano-trained-from-scratch)
  - [Evaluate hierdec-piano](#evaluate-hierdec-piano)
  - [Finetuning hierdec-piano](#finetuning-hierdec-piano)
* [Architecture](#architecture)
* [Evaluation](#evaluation)
* [Anlysis and Demo](#analysis-and-demo)
  - [Qualitative Analysis](#qualitative-analysis)
  - [Samples](#samples)
* [Final thoughts](#final-thoughts)
* [Citations](#citations)

## Data preparation 
Models that process audio data require the data to be in a certain format. MIDI files contain raw audio data with instructions about a note, its volume, its speed, and other details. We can convert MIDI files into a binary representation known as a TFRecord which is usable by TensorFlow.

In order to convert your MIDI files into the necessary TFRecord, follow these steps

```
pip install magenta

ml ffmpeg

INPUT_DIRECTORY=<the midi dir>

SEQUENCES_TFRECORD=<the output file>

python3 convert_dir_to_note_sequences.py \
--input_dir=$INPUT_DIRECTORY  \
--output_file=$SEQUENCES_TFRECORD \
--recursive
```

## Instructions and Results
The following commands can be used on SCC with appropriate paths to your data and using the correct config settings located in the original project [here](https://github.com/magenta/magenta/blob/main/magenta/models/music_vae/configs.py)

### Training Results: 
- Our checkpoint of training the heirarchical model from scratch is available [here](https://drive.google.com/drive/folders/1d6HYCq-27hnXMBwP27cFHxDAZHYAzsPp?usp=sharing). 

- Our checkpoint of training a flat baseline model from scratch is to be accessed here: [data](https://drive.google.com/file/d/1-yBass-8H7ISmAxqQxCXg8QqXCvoxi_I/view?usp=sharing), [index](https://drive.google.com/file/d/1CXNwWu-fK_4CbeJYEwbG7cecNIkztXfs/view?usp=sharing), [meta](https://drive.google.com/file/d/1fEW73QCNy-3ZQvAOUQIRf71HNnj1VcNa/view?usp=sharing).

As of July 1, 2021, the music_vae_train.py script seems to be iterating over only 10 batches of data in each training session. As a result, we had to manually restart training about every 15 minutes so the model gets trained on a different 10 batches of data.

### Training flat piano:
```
python3 music_vae_train.py \
--config='flat-mel_16bar' \
--run_dir='/run/in/this/directory' \
--mode=train \
--examples_path='/path/to/examples' \
--hparams=batch_size=32,learning_rate=0.0005
```
### Training hierdec-piano: (trained from scratch)
```
python3 music_vae_train.py \
--config='hierdec-mel_16bar' \
--run_dir='/run/in/this/directory' \
--mode=train \
--examples_path='/path/to/examples' \
--hparams=batch_size=64,learning_rate=0.0005
```
### Evaluate hierdec-piano:
```
python3 music_vae_train.py \
--config='hierdec-mel_16bar' \
--run_dir='/run/in/this/directory' \
--mode=eval \
--examples_path='/path/to/examples' \
--hparams=batch_size=64
```
### Finetuning hierdec-piano:
```
python3 music_vae_train.py \
--config='hierdec-mel_16bar' \
--run_dir='/run/in/this/directory' \
--mode=train \
--examples_path='/path/to/examples' \
--hparams=batch_size=64,learning_rate=0.0005 \
--checkoint_file=/save/checkpoint/here
```

## Architecture
<img src=https://github.com/siddushan/cs523-project/blob/main/architecture.png width="500" />

*This architecture diagram is taken directly from the [paper](https://arxiv.org/pdf/1803.05428.pdf) referenced above.* It uses a recurrent VAE which contains a sequential autoencoder and a hierarchical recurrent decoder. This architecture was compared with a baseline 'flat' decoder to show this hierarchical method leads to improved results especially on longer sequences. 


## Evaluation 
The results of the MusicVAE model are evaluated using reconstruction quality and cross entropy loss. For 2-bar models, the authors used scheduled sampling and for the 16-bar models they used the teacher forcing method.

Scheduled sampling is used to help prevent exposure bias which commonly occurs in sequence generation problems like this. The basic idea is to give the model prediction values as input instead of the ground truth in order to prevent dependency on the prior being learned.
Teacher forcing is the process of giving our model the ground truth when it makes a misclassification so that predictions in later time steps may be more accurate. This is analagous to a multi-step homework problem where part b depends on the correctness of part a. 


## Analysis and Demo
### Qualitative Analysis
#### --> Analysis I 
- One way to demonstrate that that the model is utilizing the latent code is to analyze the **reconstruction accuracy**, such that model is learning the features from the latent space. 
- We visualize two sets of midi files below, both has the **original sequence on the top and its reconstruction on the bottom**.
- The interesting thing is that the model maintains the overall structure and key of the piece, but changing some local characteristics.
<p float="left">
<img src=https://github.com/siddushan/cs523-project/blob/main/reconstruction_ex01.png width="350" />
<img src=https://github.com/siddushan/cs523-project/blob/main/reconstruction_ex02.png width="350" />
</p>

#### --> Analysis II
- Another way to verify that the model is utilizing the latent code is through latent space manipulations i.e. interpolations. In the “posterior collapse” case, the decoder will generates useful samples but they will be completely independent of the latent code, and interpolations output will not change as intended. 
- As shown on the botton image, the interpolated point **(third image)** manages to successfully mix attributes of both sequences in a middle register and still remain musical.

<img src=https://github.com/siddushan/cs523-project/blob/main/Interpolation.png width="400" />


### Samples
We generated demo samples using our trained model, please referred to the audio_outputs folder
## Final thoughts

This project was interesting to work on in many ways. Working on audio data to begin with presents a challenge. Unlike image or language data, music and audio are defined by many things in a wave form like amplitude and frequency. This requires special data preparation before we could even pass it into a model for training. Audio and instruments also have different timbres meaning different instruments that are playing at the same pitch still sound different. While this is easy for a human to realize, it is much harder to define to a model. Accounting for all these differences made this project very unique. It was also a special opportunity to work on the more creative side of deep learning by listening to music and attempting to generate something natural. While the results we saw here are very simple applied to single instruments at a time, future work could focus on looking at lyrical data combined with a NLP model or looking at full compositions containing multiple instruments at once.


## Python Dependencies

- python3
- magenta


## Citations
- [[1]](https://arxiv.org/pdf/1803.05428.pdf) Adam Roberts, Jesse Engel, Colin Raffel, Curtis Hawthorne, Douglas Eck (2019). A Hierarchical Latent Vector Model for Learning Long-Term Structure in Music
- [[2]](https://github.com/magenta/magenta/tree/master/magenta/models/music_vae) Magenta's MusicVAE base code
- [[3]](https://arxiv.org/pdf/1906.04331.pdf) Daniel Duckworth, Arvind Neelakantan, Ben Goodrich, Lukasz Kaiser, Samy Bengio (2019). Parallel Scheduled Sampling 



