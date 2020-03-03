# Speaker Gender Classification

<img src='./pngs/ad.png'>

## Record and classify your own voice

[TODO]

Please read section "Setup" and "Folder structure" first.

## Table of content

[TODO]

## Setup

- Create a new directory.
- Clone this project (as an independent folder) inside that directory.
- Clone [AudioMNIST](https://github.com/soerenab/AudioMNIST) (as an independent folder) inside that directory.
- Install Python packages: tqdm, numpy, scipy, librosa, scikit-image, pytorch, fastai.
- This project is written in jupyter notebooks.

## Folder structure

[TODO]

- maps
- mfc_dataset
- nbs
- pngs
- results
- scripts

## Dataset

[AudioMNIST](https://github.com/soerenab/AudioMNIST)

- Data is available under `AudioMNIST/data`.
- Recorded in Germany.
- 60 speakers (one directory per speaker).
- 50 instances of each digit per speaker.
- 30000 audio samples of spoken digits (0-9).
- I chose gender because there are at least 10 instances of each gender (see below).
- Caution: Since there are only 60 speakers, the training and validation sets overlap in terms of speakers (but not specific instances of speech). Future work should make sure that the training and validation sets do not overlap even in terms of speakers.
- Exploratory data analysis (see `nb/eda.ipynb`):

<img src='./pngs/eda.png'>

## Feature engineering

Here's the primary preprocessing function I used. For more information, see `nb/eda.ipynb`.

```python
def pipeline(signal):
    
    emphasized_signal = pre_emphasis(signal)
  
    lifted_mfcc = librosa.feature.mfcc(
        y=emphasized_signal.astype(float), 
        sr=sample_rate, 
        n_mfcc=12, 
        dct_type=2, 
        norm='ortho', 
        lifter=22,
        n_fft = int(sample_rate * 0.025),
        hop_length= int(sample_rate * 0.01),
        power=2,
        center=False,
        window='hanning',
        n_mels=40
    )

    return lifted_mfcc

```

## Train model

- Used the fastai library.
- Used transfer learning:
    - Trained the classifier of a ResNet-34 pretrained on ImageNet for 2 epochs.
    - Trained the entire model for 1 epoch.

## Evaluate model

- Error rate: 1.07%
- Confusion matrix: <img src='./pngs/confusion_matrix.png' width=200>
- F1 score: 0.973











































