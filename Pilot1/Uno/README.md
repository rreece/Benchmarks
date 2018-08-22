## Uno: Predicting Tumor Dose Response across Multiple Data Sources

#### Example output
Uno can be trained with a subset of dose response data sources. Here is an command line example of training with all 6 sources: CCLE, CTRP, gCSI, GDSC, NCI60 single drug response, ALMANAC drug pair response.

```
uno_baseline_keras2.py --train_sources all --cache cache/all --use_landmark_genes --preprocess_rnaseq source_scale --no_feature_source --no_response_source
Using TensorFlow backend.
Params: {'activation': 'relu', 'batch_size': 32, 'dense': [1000, 1000, 1000], 'dense_feature_layers': [1000, 1000, 1000], 'drop': 0, 'epochs': 10, 'learning_rate': None, 'loss':
'mse', 'optimizer': 'adam', 'residual': False, 'rng_seed': 2018, 'save': 'save/uno', 'scaling': 'std', 'feature_subsample': 0, 'validation_split': 0.2, 'solr_root': '', 'timeout'
: -1, 'train_sources': ['all'], 'test_sources': ['train'], 'cell_types': None, 'cell_features': ['rnaseq'], 'drug_features': ['descriptors', 'fingerprints'], 'cv': 1, 'max_val_lo
ss': 1.0, 'base_lr': None, 'reduce_lr': False, 'warmup_lr': False, 'batch_normalization': False, 'no_gen': False, 'config_file': '/raid/fangfang/Benchmarks/Pilot1/Uno/uno_default
_model.txt', 'verbose': False, 'logfile': None, 'train_bool': True, 'shuffle': True, 'alpha_dropout': False, 'gpus': [], 'experiment_id': 'EXP.000', 'run_id': 'RUN.000', 'by_cell
': None, 'by_drug': None, 'drug_median_response_min': -1, 'drug_median_response_max': 1, 'no_feature_source': True, 'no_response_source': True, 'use_landmark_genes': True, 'use_f
iltered_genes': False, 'preprocess_rnaseq': 'source_scale', 'cp': False, 'tb': False, 'partition_by': None, 'cache': 'cache/ALL', 'single': False, 'export_data': None, 'growth_bi
ns': 0, 'datatype': <class 'numpy.float32'>}
Cache parameter file does not exist: cache/ALL.params.json
Loading data from scratch ...
Loaded 27769716 single drug dose response measurements
Loaded 3686475 drug pair dose response measurements
Combined dose response data contains sources: ['CCLE' 'CTRP' 'gCSI' 'GDSC' 'NCI60' 'SCL' 'SCLC' 'ALMANAC.FG'
 'ALMANAC.FF' 'ALMANAC.1A']
Summary of combined dose response by source:
              Growth  Sample  Drug1  Drug2  MedianDose
Source
ALMANAC.1A    208605      60    102    102    7.000000
ALMANAC.FF   2062098      60     92     71    6.698970
ALMANAC.FG   1415772      60    100     29    6.522879
CCLE           93251     504     24      0    6.602060
CTRP         6171005     887    544      0    6.585027
GDSC         1894212    1075    249      0    6.505150
NCI60       18862308      59  52671      0    6.000000
SCL           301336      65    445      0    6.908485
SCLC          389510      70    526      0    6.908485
gCSI           58094     409     16      0    7.430334
Combined raw dose response data has 3070 unique samples and 53520 unique drugs
Limiting drugs to those with response min <= 1, max >= -1, span >= 0, median_min <= -1, median_max >= 1 ...
Selected 47005 drugs from 53520
Loaded combined RNAseq data: (15198, 943)
Loaded combined dragon7 drug descriptors: (53507, 5271)
Loaded combined dragon7 drug fingerprints: (53507, 2049)
Filtering drug response data...
  2375 molecular samples with feature and response data
  46837 selected drugs with feature and response data
Summary of filtered dose response by source:
              Growth  Sample  Drug1  Drug2  MedianDose
Source
ALMANAC.1A    206580      60    101    101    7.000000
ALMANAC.FF   2062098      60     92     71    6.698970
ALMANAC.FG   1293465      60     98     27    6.522879
CCLE           80213     474     22      0    6.602060
CTRP         3397103     812    311      0    6.585027
GDSC         1022204     672    213      0    6.505150
NCI60       17190561      59  46272      0    6.000000
gCSI           50822     357     16      0    7.430334
Grouped response data by drug_pair: 51763 groups
Input features shapes:
  dose1: (1,)
  dose2: (1,)
  cell.rnaseq: (942,)
  drug1.descriptors: (5270,)
  drug1.fingerprints: (2048,)
  drug2.descriptors: (5270,)
  drug2.fingerprints: (2048,)
Total input dimensions: 15580
Saved data to cache: cache/all.pkl
Combined model:
__________________________________________________________________________________________________
Layer (type)                    Output Shape         Param #     Connected to
==================================================================================================
input.cell.rnaseq (InputLayer)  (None, 942)          0
__________________________________________________________________________________________________
input.drug1.descriptors (InputL (None, 5270)         0
__________________________________________________________________________________________________
input.drug1.fingerprints (Input (None, 2048)         0
__________________________________________________________________________________________________
input.drug2.descriptors (InputL (None, 5270)         0
__________________________________________________________________________________________________
input.drug2.fingerprints (Input (None, 2048)         0
__________________________________________________________________________________________________
input.dose1 (InputLayer)        (None, 1)            0
__________________________________________________________________________________________________
input.dose2 (InputLayer)        (None, 1)            0
__________________________________________________________________________________________________
cell.rnaseq (Model)             (None, 1000)         2945000     input.cell.rnaseq[0][0]
__________________________________________________________________________________________________
drug.descriptors (Model)        (None, 1000)         7273000     input.drug1.descriptors[0][0]
                                                                 input.drug2.descriptors[0][0]
__________________________________________________________________________________________________
drug.fingerprints (Model)       (None, 1000)         4051000     input.drug1.fingerprints[0][0]
                                                                 input.drug2.fingerprints[0][0]
__________________________________________________________________________________________________
concatenate_1 (Concatenate)     (None, 5002)         0           input.dose1[0][0]
                                                                 input.dose2[0][0]
                                                                 cell.rnaseq[1][0]
                                                                 drug.descriptors[1][0]
                                                                 drug.fingerprints[1][0]
                                                                 drug.descriptors[2][0]
                                                                 drug.fingerprints[2][0]
__________________________________________________________________________________________________
dense_10 (Dense)                (None, 1000)         5003000     concatenate_1[0][0]
__________________________________________________________________________________________________
dense_11 (Dense)                (None, 1000)         1001000     dense_10[0][0]
__________________________________________________________________________________________________
dense_12 (Dense)                (None, 1000)         1001000     dense_11[0][0]
__________________________________________________________________________________________________
dense_13 (Dense)                (None, 1)            1001        dense_12[0][0]
==================================================================================================
Total params: 21,275,001
Trainable params: 21,275,001
Non-trainable params: 0
__________________________________________________________________________________________________
Between random pairs in y_val:
  mse: 0.6069
  mae: 0.5458
  r2: -0.9998
  corr: 0.0001
Data points per epoch: train = 20158325, val = 5144721
Steps per epoch: train = 629948, val = 160773
Epoch 1/10
  8078/629948 [..............................] - ETA: 50:20:54 - loss: 0.1955 - mae: 0.2982 - r2: 0.2964
```