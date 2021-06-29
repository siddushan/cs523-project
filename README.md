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
This project was interesting in multiple ways. First, the problem of no-reference image quality is unique and challenging. One part we would like to point out is the disparity of measurements the metrics provide. An opportunity for future research would be to compile more results from additional metrics such as DIIVINE. Another interesting note is that we, as humans, could pick out the "best" deblurred image across every method., this leads to some thought about the human eye and how well it can detect altered images. Another opportunity for future research might be a double-blind test to compare image quality metrics against human vision.  One difficulty we faced individually was discovering what metrics would be helpful for measurement since traditional metrics like PSNR and SSIM were not applicable.



## Python Dependencies

python3
magenta


## Citations
- [1](https://arxiv.org/pdf/1803.05428.pdf) Adam Roberts, Jesse Engel, Colin Raffel, Curtis Hawthorne, Douglas Eck (2019). A Hierarchical Latent Vector Model for Learning Long-Term Structure in Music
- [2](https://github.com/magenta/magenta/tree/master/magenta/models/music_vae) Magenta's MusicVAE base code



