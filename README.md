# Music Generation Using Magenta
By: Junyu Liu, Sidd Shanmugam, Minglan Zheng

## Introduction
This project reproduces results stemming from [this](https://arxiv.org/pdf/1803.05428.pdf) paper written about hierarchical latent vector models for long-term structure in music. The slides used to present this project can be found [here](https://docs.google.com/presentation/d/1eq6TXIcUN9CCoQ9xSQ6d6CwnDIS0M2twH8SrhgaBlL8/edit?usp=sharing)

## Data preparation 
In order to convert your midi files into the necessary tfrecord, follow these steps

```
pip install magenta （only needs to do this once)
ml ffmpeg (you can add ffmpeg to modules to load)
INPUT_DIRECTORY=<the midi dir>
SEQUENCES_TFRECORD=<the output file>
python3 convert_dir_to_note_sequences.py   --input_dir=$INPUT_DIRECTORY   --output_file=$SEQUENCES_TFRECORD   --recursive
```

## Evaluation 
The results of the MusicVAE model are evaluated using reconstruction quality and cross entropy loss. 

## Results


## Discussion

This project was interesting to work on in many ways. Working on audio data to begin with presents a challenge. Unlike image or language data, music and audio are defined by many things in a wave form like amplitude and frequency. This requires special data preparation before we could even pass it into a model for training. Audio and instruments also have different timbres meaning different instruments that are playing at the same pitch still sound different. While this is easy for a human to realize, it is much harder to define to a model. Accounting for all these differences made this project very unique. It was also a special opportunity to work on the more creative side of deep learning by listening to music and attempting to generate something natural. While the results we saw here are very simple applied to single instruments at a time, future work could focus on looking at lyrical data combined with a NLP model or looking at full compositions containing multiple instruments at once.


## Python Dependencies

- python3
- magenta


## Citations
- [[1]](https://arxiv.org/pdf/1803.05428.pdf) Adam Roberts, Jesse Engel, Colin Raffel, Curtis Hawthorne, Douglas Eck (2019). A Hierarchical Latent Vector Model for Learning Long-Term Structure in Music
- [[2]](https://github.com/magenta/magenta/tree/master/magenta/models/music_vae) Magenta's MusicVAE base code



