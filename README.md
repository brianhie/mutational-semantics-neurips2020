# Learning mutational semantics

This repository contains the analysis code, links to the data, and pretrained models for the paper "Learning mutational semantics" by Brian Hie, Ellen Zhong, Bryan Bryson, and Bonnie Berger, which will appear at NeurIPS 2020.

For a more biologically-oriented follow-up work, including analysis of SARS-CoV-2 viral sequences, see our paper ["Learning the language of viral evolution and escape"](https://www.biorxiv.org/content/10.1101/2020.07.08.193946v1).

### Data

You can download the [relevant datasets](http://cb.csail.mit.edu/cb/viral-mutation/data.tar.gz) (including training and validation data) using the commands
```bash
wget http://cb.csail.mit.edu/cb/viral-mutation/data.tar.gz
tar xvf data.tar.gz
```
within the same directory as this repository.

### Dependencies

The major Python package requirements and their tested versions are in [requirements.txt](requirements.txt).

Our experiments were run with Python version 3.7 on Ubuntu 18.04.

### Experiments

To run the experiments below, download the data (instructions above). Our experiments require a maximum of 400 GB of CPU RAM and 32 GB of GPU RAM (though often much less); _in silico_ escape model inference can take around 35 minutes for influenza HA and 90 minutes for HIV Env.

#### News headlines

Headline POS changes can be evaluated with the command
```bash
python bin/parse_headline_mods.py results/headlines/semantics_1024.log \
    > headline_pos.log 2>&1
```

Headline WordNet changes can be evaluated with the command
```bash
python bin/parse_semantic_headlines.py results/headlines/semantics_1024.log \
    > headline_wordnet.log 2>&1
```

Generating headline changes can be done with the command
```bash
python bin/headlines.py bilstm --checkpoint data/headlines.hdf5 --semantics \
    > semantics.log 2>&1 &
```

#### Influenza HA

Influenza HA semantic embedding UMAPs and log files with statistics can be generated with the command
```bash
python bin/flu.py bilstm --checkpoint models/flu.hdf5 --embed \
    > flu_embed.log 2>&1
```

Single-residue escape prediction using validation data from [Doud et al. (2018)](https://github.com/jbloomlab/HA_antibody_ease_of_escape) and [Lee et al. (2019)](https://github.com/jbloomlab/map_flu_serum_Perth2009_H3_HA) can be done with the command
```bash
python bin/flu.py bilstm --checkpoint models/flu.hdf5 --semantics \
    > flu_semantics.log 2>&1
```

Training a new model on flu HA sequences can be done with the command
```bash
python bin/flu.py bilstm --train --test \
    > flu_train.log 2>&1
```

#### HIV Env

HIV Env semantic embedding UMAPs and log files with statistics can be generated with the command
```bash
python bin/hiv.py bilstm --checkpoint models/hiv.hdf5 --embed \
    > hiv_embed.log 2>&1
```

Single-residue escape prediction using validation data from [Dingens et al. (2019)](https://github.com/jbloomlab/EnvsAntigenicAtlas) can be done with the command
```bash
python bin/hiv.py bilstm --checkpoint models/hiv.hdf5 --semantics \
    > hiv_semantics.log 2>&1
```

Training a new model on HIV Env sequences can be done with the command
```bash
python bin/hiv.py bilstm --train --test \
    > hiv_train.log 2>&1
```

### Questions

For questions about the pipeline and code, contact brianhie@mit.edu. We will do our best to provide support, address any issues, and keep improving this software. And do not hesitate to submit a pull request and contribute!
