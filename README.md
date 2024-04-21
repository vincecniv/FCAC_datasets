# FCAC_datasets

Motivation for constructing the datasets: 

To study the Few-shot Class-incremental Audio Classification (FCAC) problem, three datasets of LS-100 dataset, NSynth-100 dataset and FSC-89 dataset are constructed by choosing samples from audio corpora of the [Librispeech](https://www.openslr.org/12/) dataset, the [NSynth](https://magenta.tensorflow.org/datasets/nsynth) dataset and the [FSD-MIX-CLIPS](https://zenodo.org/record/5574135#.YWyINEbMIWo) dataset respectively.


## Statistics on the datasets
|                                                              |                    NS-100                     |                    FS-89                     | LS-100                                        |
| :----------------------------------------------------------: | :-------------------------------------------: | :------------------------------------------: | --------------------------------------------- |
|                        Type of audio                         |              Musical instruments              |                  Free sound                  | Speech                                        |
|                       Num. of classes                        | 100 (55 of base classes, 45 of novel classes) | 89 (59 of base classes, 30 of novel classes) | 100 (60 of base classes, 40 of novel classes) |
| Num. of training / validation / testing samples per base class |                200 / 100 / 100                |               800 / 200 / 200                | 500 / 150 / 100                               |
| Num. of training / validation / testing samples per novel class |               100 / none / 100                |               500 / none / 200               | 500 / 150 / 100                               |
|                    Duration of the sample                    |               All in 4 seconds                |               All in 1 second                | All in 2 seconds                              |
|                      Sampling frequency                      |                 All in 16K Hz                 |               All in 44.1K Hz                | All in 16K Hz                                 |

## Preparation of the NS-100 dataset


The NSynth dataset is an audio dataset containing 306,043 musical notes, each with a unique pitch, timbre, and envelope. 
Those musical notes are belonging to 1,006 musical instruments. 

Before constructing the NS-100 dataset, we first conduct some statistical analysis on the NSynth dataset, see [here](/Statistics_of_the_NSynth_dataset.md).

Based on the statistical results, we obtain the NS-100 dataset by the following steps:

1. Download [Train set](http://download.magenta.tensorflow.org/datasets/nsynth/nsynth-train.jsonwav.tar.gz), [Valid set](http://download.magenta.tensorflow.org/datasets/nsynth/nsynth-valid.jsonwav.tar.gz), and [test set](http://download.magenta.tensorflow.org/datasets/nsynth/nsynth-test.jsonwav.tar.gz) of the NSynth dataset to your local machine and unzip them.
You should get a structure of the directory as follows:
<pre>
Your dataset root (NSynth_audio_for_FCAC)
├── nsynth-train　 # Training set of the NSynth dataset
│    ├── audio
│    |    ├── bass_acoustic_000-024-025.wav
│    |    └── ....
│    └── examples.json  # meta file of the training set
│
├── nsynth-val  # Validation set of the NSynth dataset
│    ├── audio
│    |    ├── bass_electronic_018-022-025.wav
│    |    └── ....
│    └── examples.json
│
└── nsynth-test # Test set of the NSynth dataset
     ├── audio
     |    ├── bass_electronic_018-022-100.wav
     |    └── ....
     └── examples.json
</pre>
2. Download the meta files for FCAC from [here](./Nsynth_meta_for_FCAC) to your local machine and unzip them.
You should get a structure of the directory as follows:
<pre>
Your dataset root (NSynth_meta_for_FCAC)
├── nsynth-100-fs-meta
│    ├── nsynth-100-fs_train.csv # containing information of all training samples from the base and novel classes
│    ├── nsynth-100-fs_val.csv  #　containing information of all validation samples from the base classes
│    ├── nsynth-100-fs_test.csv　# containing information of all test samples from the old and novel classes
│    └── nsynth-100-fs_vocab.json 　# label vocabulary of the dataset
│    
├── nsynth-200-fs-meta
│    ├── nsynth-200-fs_train.csv #  
│    ├── nsynth-200-fs_val.csv
│    ├── nsynth-200-fs_test.csv
│    └── nsynth-200-fs_vocab.json
│    
├── nsynth-300-fs-meta
│    ├── nsynth-300-fs_train.csv #  
│    ├── nsynth-300-fs_val.csv
│    ├── nsynth-300-fs_test.csv
│    └── nsynth-300-fs_vocab.json
│       
└── nsynth-400-fs-meta
     ├── nsynth-400-fs_train.csv #  
     ├── nsynth-400-fs_val.csv
     ├── nsynth-400-fs_test.csv
     └── nsynth-400-fs_vocab.json

</pre>

3. Run the following script to load the NSynth-100 dataset:
```
python Load_nsynth_data_for_FCAC.py --metapath path to NSynth_audio_for_FCAC folder --audiopath path to NSynth_meta_for_FCAC folder --num_class 100 --base_class 55

```

## Preparation of the FS-89 dataset

1. Since the FS-89 dataset is extracted from the FSD-MIX-CLIPS dataset, we need to prepare the FSD-MIX-CLIPS dataset first. See the instructions in
[here](./Preparation_of_the_FSD-MIX-CLIPS_dataset/README.md).

2. Download the meta file of FS-89 dataset from [here](./FSC-89-meta), You should get a structure of the directory as follows:

<pre>
FS-89-meta   
   ├── setup1  # 
   |     ├── Fsc89-setup1-fsci_train.csv # -  
   |     ├── Fsc89-setup1-fsci_val.csv  # -  
   |     └── Fsc89-setup1-fsci_test.csv # -  
   |
   └── setup2 # -  
         ├── Fsc89-setup2-fsci_train.csv # -  
         ├── Fsc89-setup2-fsci_val.csv  # -  
         └── Fsc89-setup2-fsci_test.csv # -  

</pre>

3. Run the following script to load the FSC-89 dataset:

```
python load_fsc_89_data_for_FCAC.py --metapath path to FSC-89-meta folder \
--datapath path to FSD-MIX-CLIPS_data folder --data_type audio --setup setup1
```

## Preparation of the LS-100 dataset



LibriSpeech is a corpus of approximately 1000 hours of 16kHz read English speech, prepared by Vassil Panayotov with the assistance of Daniel Povey. The data is derived from read audiobooks from the LibriVox project, and has been carefully segmented and aligned. We find that the subset `train-clean-100` of Librispeech is enough for our study, so we constructed the LS-100 dataset using partial samples from the Librispeech as the source materials. To be specific, we first concatenate all the speakers' speech clips into a long speech, and then select the 100 speakers with the longest duration to cut their voices into two second speech. You can download the Librispeech from [here](https://www.openslr.org/12/).

1. Download [dataset](https://www.openslr.org/resources/12/train-clean-100.tar.gz) and extract the files.

2. Transfer the format of audio files. Move the script `normalize-resample.sh` to the root dirctory of extracted folder, and run the command `bash normalize-resample.sh`.

3. Construct LS-100 dataset.

   ```
   python LS100/construct_LS100.py --data_dir DATA_DIR --duration_json data/librispeech/spk_total_duration.json --single_spk_dir SINGLE_SPK_DIR --num_select_spk 100 --spk_segment_dir SPK_SEGMENT_DIR --csv_path CSV_PATH --spk_mapping_path SPK_MAPPING_PATH
   ```

   
