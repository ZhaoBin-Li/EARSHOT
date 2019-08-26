# EARS User Guide

## Before starting

* Download input file `Pattern_Dict.IM_Spectrogram.OM_SRV.AN_10.Size_10000.WL_10.pickle` [here](https://drive.google.com/file/d/1pujVHSPtwXWZiQeutFJwxdsz1mz0Lddi/view)

* Download diphone wav files for hidden analysis [here](https://drive.google.com/file/d/1poWuCQ1_09jBSaIZJbj5KvBLl4HejdH3/view)

* All the commands below are done at the terminal (command console for Windows).

* Python 3.x and several python libraries must be installed in your environment by following command:
    ```
    GPGPU using:
    pip install tensorflow-gpu, librosa, matplotlib

    CPU only:
    pip install tensorflow, librosa, matplotlib
    ```

## Simulation execution
### Command

```
python EARShot.py [parameters]
```

### Parameters

* `-ht LSTM|GRU|SCRN|BPTT`
    * Determines the type of hidden layer. You can enter either LSTM, GRU, SCRN, or BPTT.
    * This parameter is required.

* `-hu <int>`
    * Determines the size of the hidden layer. You can enter a positive integer.
    * This parameter is required.

* `-tt <int>`
    * Set the frequency of the test during learning. You can enter a positive integer.
    * This parameter is required.

* `-se <int>`
    * Set the model's start epoch. This parameter and the 'mf' parameter must be set when loading a previously learned model.
    * The default value is 0.

* `-me <int>`
    * Set the ending epoch of the model. The default is 20000.

* `-em P|T|M`
    * Set pattern exclusion method. You can choose between P (pattern based), T (talker based), or M (mix based).
    * If set to P, 1/10 of each talker pattern will not be trained.
    * When set to T, all patterns of one talker are excluded from the learning. The talker can be set via the 'et' parameter.
    * When set to M, patterns are excluded as a mixture of the two methods.
    * If not set, all patterns will be learned.

* `-et <talker>`
    * Set which talker pattern is excluded from the learning.
    * Applies if 'em' parameter is T or M, otherwise this parameter is ignored.

* `-ei`
    * If you enter this parameter, all exclusion settings above will be ignored.
    * This is the parameter used to over-training all patterns after normal training.
    * It is recommended that you do not assign the 'em' parameter if you want to learn all patterns from the beginning.

* `-idx <int>`
    * Attach an index tag to each result.
    * This value does not affect the performance of the model.

### Execution examples

```
python EARShot.py -ht LSTM -hu 512 -tt 1000 -se 0 -me 4000 -em M -et Fred -idx 0
python EARShot.py -ht LSTM -hu 512 -tt 500 -se 2000 -me 2500 -em M -et Bruce -ei -idx 0
```

## Result analysis

### Command

```
python Result_Analysis.py [parameters]
```

### Parameters

* `-d <path>`
    * Results directory to run the analysis on.
    * This parameter is required.
    * Ex. `./Results/IDX_0/HM_LSTM.H_512.EM_M.ET_AGNES.IDX_0`

* `-a <float>`
    * Criterion in reaction time calculation of absolute method.
    * The default is 0.7.

* `-r <float>`
    * Criterion in reaction time calculation of relative method.
    * The default is 0.05.

* `-tw <int>`
    * Width criterion in reaction time calculation of time dependent method used in the paper.
    * The default is 10.

* `-th <float>`
    * Height criterion in reaction time calculation of time dependent method used in the paper.
    * The default is 0.05.

### Execution examples

```
python Result_Analysis.py -d ./Results/IDX_0/HM_LSTM.H_512.EM_M.ET_AGNES.IDX_0
python Result_Analysis.py -d ./Results/IDX_2/HM_LSTM.H_512.EM_M.ET_BRUCE.IDX_2 -tw 5 -th 0.1
```

## Hidden analysis

### Command

```
python Hidden_Analysis.py [parameters]
```

### Parameters

* `-d <path>`
    * Results directory to run the analysis on.
    * This parameter is required.
    * Ex. `./Results/IDX_0/HM_LSTM.H_512.EM_M.ET_AGNES.IDX_0`

* `-e <int>`
    * The epoch to run the analysis on.
    * This parameter is required.

* `-v <path>`
    * diphone wav directory to be used for hidden analysis
    * This parameter is required.
    * Ex. `./Diphone_Wav`

### Execution examples

```
python Hidden_Analysis.py -d ./Results/IDX_0/HM_LSTM.H_512.EM_M.ET_AGNES.IDX_0 -e 2000 -v ./Diphone_wav
```

## RSA analysis

### Command

```
python RSA_Analysis.py [parameters]
```

### Parameters

* `-d <path>`
    * Results directory to run the analysis on.
    * This parameter is required.
    * Ex. `./Results/IDX_0/HM_LSTM.H_512.EM_M.ET_AGNES.IDX_0`

* `-e <int>`
    * The epoch to run the analysis on.
    * To proceed with the RSA analysis, you must first perform a hidden analysis on the configured epoch.
    * This parameter is required.

* `-c <float>`
    * Criterion of the PSI and FSI map to be used.

* `-pn <int>`
    * Number of permutation tests
    * The default is 1000000.

### Execution examples

```
python RSA_Analysis.py -d ./Results/IDX_0/HM_LSTM.H_512.EM_M.ET_AGNES.IDX_0 -e 2000 -c 0.0 -pn 10000
```


## Generate figures by R script

### Accuracy and cosine similarity flow

#### Method

1. Use `./R_Script/Acc_and_CS_Flow(Fig.3).R`

2. Modify the parameters in line 116 - 124

3. Run. The result will be in `base_Dir`.

#### Result example
<img src='./Example_Figures/Fig.3.B.png' width=50% height=50% /><img src='./Example_Figures/Fig.3.A.png' width=50% height=50% />

### PSI and FSI

#### Method

1. Use `./R_Script/PSI_and_FSI(Fig.4).R`

2. Modify the parameters in line 36 - 42

3. Run. The result will be in each talker's hidden analysis directory.

#### Result example
![]('./Example_Figures/Fig.4.B.png')
![]('./Example_Figures/Fig.4.A.png')

### RSA permutation test

#### Method

1. Use `./R_Script/RSA_Permutation_Test(Fig.5).R`

2. Modify the parameters in line 7 - 13

3. Run. The result will be in each talker's hidden analysis directory.

#### Result example
![]('./Example_Figures/Fig.S1.2.png')

### Phoneme and feature flow

#### Method

1. Use `./R_Script/Phoneme_and_Feature_Flow(Fig.6).R`

2. Modify the parameters in line 10 - 16

3. Run. The result will be in each talker's hidden analysis directory.

#### Result example
<img src='./Example_Figures/Fig.6.png' width=50% height=50% /><img src='./Example_Figures/Fig.6.Feature.png' width=50% height=50% />

### Accuracy flow integration by talker

This is different 'Accuracy and cosine similarity flow'. This analysis checks the accuracy of the talker's data in all simulations.

#### Method

1. Use `./R_Script/Acc_Flow_Integration_by_Talker(Fig.S2.1).R`

2. Modify the parameters in line 53 - 61

3. Run. The result will be in `base_Dir`.

#### Result example
![]('./Example_Figures/Fig.S2.1.png')

### Phoneme and feature flow tile

#### Method

1. Use `./R_Script/Phoneme_and_Feature_Flow_All_Tile(Fig.S2.2-3).R`

2. Modify the parameters in line 22 - 28

3. Run. The result will be in each talker's hidden analysis directory.

#### Result example
![]('./Example_Figures/Fig.S2.2.png')

### Phoneme and feature flow compare

#### Method

1. Use `./R_Script/Phoneme_and_Feature_Flow_Compare(Fig.S2.4).R`

2. Modify the parameters in line 10 - 16

3. Run. The result will be in each talker's hidden analysis directory.

#### Result example
![]('./Example_Figures/Fig.S2.4.png')
