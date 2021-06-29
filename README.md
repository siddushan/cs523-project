# Music Generation Using Magenta
By: Junyu Liu, Sidd Shanmugam, Minglan Zheng

## Introduction
This project reproduces results stemming from [this](https://arxiv.org/pdf/1803.05428.pdf) paper written about hierarchical latent vector models for long-term structure in music. 

## Data conversion 
After ensuring you have the correct python dependencies.

```
pip install magenta （only needs to do this once)
ml ffmpeg (you can add ffmpeg to modules to load)
INPUT_DIRECTORY=<the midi dir>
SEQUENCES_TFRECORD=<the output file>
python3 convert_dir_to_note_sequences.py   --input_dir=$INPUT_DIRECTORY   --output_file=$SEQUENCES_TFRECORD   --recursive
```

## Evaluation 


## Results


## Discussion




## Python Dependencies

- python3
- magenta


## Citations
- [[1]](https://arxiv.org/pdf/1803.05428.pdf) Adam Roberts, Jesse Engel, Colin Raffel, Curtis Hawthorne, Douglas Eck (2019). A Hierarchical Latent Vector Model for Learning Long-Term Structure in Music
- [[2]](https://github.com/magenta/magenta/tree/master/magenta/models/music_vae) Magenta's MusicVAE base code



