# Pre-processing tools for the DCASE 2022 task 6-A

This repository contains the data pre-processing tools used in the baseline. These tools are already included in the [baseline repository](https://github.com/felixgontier/dcase-2022-baseline).

## Setup

First, clone this repository on your computer:

````shell script
$ git clone git@github.com:felixgontier/dcase-2022-preprocessing.git
````

This operation will create a `dcase-2022-preprocessing` directory at the current location, with the contents of this repository.

The data pre-processing tools rely on several Python packages, which can be installed by running:

````shell script
$ python3.7 -m venv env/ # Optionally create a virtual environment
$ pip install -r requirements_pip.txt
````

## Clotho dataset preparation

### Obtaining the data from Zenodo

The Clotho v2.1 dataset can be found on Zenodo: [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.4783391.svg)](https://doi.org/10.5281/zenodo.4783391)

The test set (without captions) is available separately: [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3865658.svg)](https://doi.org/10.5281/zenodo.3865658)

After downloading all `.7z` archives and `.csv` caption files from both repositories, audio files should be extracted in the `data` directory.

Specifically, the directory structure should be as follows from the baseline root directory:

    data/
     | - clotho_v2/
     |   | - development/
     |   |   | - *.wav
     |   | - validation/
     |   |   | - *.wav
     |   | - evaluation/
     |   |   | - *.wav
     |   | - test/
     |   |   | - *.wav
     |   | - clotho_captions_development.csv
     |   | - clotho_captions_validation.csv
     |   | - clotho_captions_evaluation.csv

### Data pre-processing

Pre-processing operations are implemented in `clotho_preprocessing.py`.

Dataset preparation is done by running the following command:

````shell script
$ python clotho_preprocessing.py --cfg dcb_data
````

The script outputs a `<file_name>_<caption_id>.npy` file for each ground truth caption of each audio file in the dataset. Each output file contains a Numpy record array with the following fields:

 * `file_name`: Name of the source audio file.
 * `vggish_embeddings`: VGGish embeddings extracted for 1s audio frames with 1s interval.
 * `caption`: The corresponding caption, with all punctuation removed and all lowercase.

Output directories follow the same structure as the inputs:

    data/
     | - clotho_v2_vggish/
     |   | - development/
     |   |   | - *.npy
     |   | - validation/
     |   |   | - *.npy
     |   | - evaluation/
     |   |   | - *.npy
     |   | - test/
     |   |   | - *.npy

### Pre-processing parameters

Data pre-processing relies on settings in the `data_settings/dcb_data.yaml` file.

    data:
      root_path: 'data'
      input_path: 'clotho_v2'
      output_path: 'clotho_v2_vggish'
      splits:
        - development
        - validation
        - evaluation
        - test

The settings are the following:

 * `root_path` (str): Path to the root data directory.
 * `input_path` (str): Sub-path of `root_path` with unprocessed data.
 * `output_path` (str): Sub-path of `root_path` where pre-processed data should be saved. If it does not exist, the directory will be created.
 * `splits` (list(str)): Data splits, each corresponding to a sub-directory of `input_path` and `output_path`.


