## Fully Connected & Graphical Neural Networks to Classify Fmri Movie Data 
> Application of Fully Connected Neural Networks (FCNs) & Graphical Convolutional Neural Networks (GCNs) using pytorch to fmri movie data.

- [Overview](#overview)
- [Introduction](#Introduction)
  * [Data](#data)
  * [Objectives](#Objectives)
- [Results](#Results)
  * FCN > GCN across all tests
  * Network FCNs - Visual is best
- [Code](#Code)
  * [1_FCNs_vs_GCNs_fmri_classification](#1_FCNs_vs_GCNs_fmri_classification)
  * [2_Network_Models_FCN](#2_Network_Models_FCN)
  * [util_funcs.py](#util_funcs.py)
  * [models_fcn.py](#models_fcn.py)
  * [models_fcn_gcn.py](#models_fcn_gcn.py)
- [Using_the_Resource](#Using_the_Resource)
  * [Installation](#Installation)
  * [Pre-requisites](#Pre-requisites)
- [Slides](#Slides)
- [References](#References)

_This work was carried out with Prof Rhodri Cusack of the_ [Cusack lab](https://www.cusacklab.org/) _at the_ [Trinity Centre of Neuroscience](https://www.tcd.ie/Neuroscience/), [Trinity College Dublin](https://www.tcd.ie/)

## Overview

FCNs and GCNs (1st order, 5th order, 8th order), developed using pytorch, were used to classify time blocks of fmri data across subjects.
The fmri data came from the [Cam-CAN study - The Cambridge Centre for Ageing and Neuroscience](https://www.cam-can.org/). It was recorded while subjects watched an excerpt from a Hitchcock movie. The fmri data was split into equaly sized blocks of timepoints. The FCN and GCN models were then used in a Machine Learning manner to classify each timepoint as coming from each of the 26 blocks.  It transpired that the FCNs yielded a better predictive performance than the GCNs. FCNs were therefore used in the second part of the study for further analysis. This involved parcellating the fmri data into 7 key networks based on the [Yeo 7 Network Parcellation](https://github.com/ThomasYeoLab/CBIG/tree/master/stable_projects/brain_parcellation/Schaefer2018_LocalGlobal), specifically this [Network Parcellation File](https://github.com/ThomasYeoLab/CBIG/blob/master/stable_projects/brain_parcellation/Schaefer2018_LocalGlobal/Parcellations/MNI/Schaefer2018_400Parcels_7Networks_order.txt) and determining the classification power of each network separately. 



*Yeo 7 Network Parcellation*

<img src="https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/experimenting/yeo_parcellation.PNG" width="500" />


## Introduction 
A central goal in neuroscience is to understand the mechanisms in the brain responsible for cognitive functions. A recent approach known as “brain decoding ”, involves inferring certain experimental points in time using pattern classification of brain activity across participants. Here a multidomain brain decoder was proposed that automatically learns the spatiotemporal dynamics of brain response within a short time window using a deep learning approach. 

### Data
The decoding model was evaluated on the fmri of 644 participants from the Cam-CAN study.

<img src="https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/experimenting/camcan_study.png" width="200" />


The fmri data was recorded while the participants watched a 7 minute extract from the Hitchcock film - _Bang! You're Dead_. THe fmri data was of dimension 644 subjects x 193 timepoints x 400 Regions of Interest (ROIs). A number of time windows for fmri classification were used including 6, 8 and 16 blocks. Note that the repetition time (TR) of the fmri data, whci is the time it takes to scan all slices, is equal to 2.47 secs in this instance. 

<img src="https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/experimenting/hitchcock_movie.jpg" width="150" />

### Objectives 
As mentioned above, the main focus of the analyses was;

#### 1. Comparison of FCNs vs GCNs
The classification performance of the FCNs was compared to that of the GCNs to determine which was the optimal classifier in the context of decoding fmri data. The fmri data was split into equaly sized blocks of timepoints, for example, the 192 timepoints would be split into 26 x 8 blocks for a block duration of 8. THe FCN and GCN models were then used in a Machine Learning manner to classify each timepoint as coming from each of the 26 blocks.  
As shown below, the best results were obtained using the FCN model, achieving a mean accuracy of 90.7%, and so the FCN was used for further analysis as below. 


#### 2. Network Model parcellation
This invloved parcellating the fmri data into distinctive cognitive networks, specifically 400 parcel parcellation matched to [Yeo 7 Network Parcellation](https://github.com/ThomasYeoLab/CBIG/tree/master/stable_projects/brain_parcellation/Schaefer2018_LocalGlobal) from this [Network Parcellation File](https://github.com/ThomasYeoLab/CBIG/blob/master/stable_projects/brain_parcellation/Schaefer2018_LocalGlobal/Parcellations/MNI/Schaefer2018_400Parcels_7Networks_order.txt). The 7 networks of cognitive function included the 
Visual, Somatomotor, Dorsal Attention, Ventral Attention, Limbic, Default and Control Networks. The FCN model was applied to the fmri data of each parcellated network separately to determine the indiviudal predictive power of each of the 7 parcellated networks across all individuals fmri.

## Results 

### FCN > GCN across all tests

The following accuracy results where obtained for the FCN and GCN models when used to classify the fmri data across timepoints. The FCN model performed significantly better then the GCN models. It obtained a mean accuracy of 90.7% compared to 78.5% for the 5th order GCN model, 75.1% for the 8th order GCN model and 54.1% for the 1st order GCN model. Thus for the future analyses the FCN model was used. It's architecture was also a lot more straightforward, a further advantage of the model. 

_FCN Model training_

<img src="https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/experimenting/model_training.PNG" width="500" />

_FCN vs GCN Model Results_

<img src="https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/1_FCNs_vs_GCNs_fmri_classification/miscellaneous_results/model_results.PNG" width="700" />


------

### Network FCNs - Visual is best
The Visual Network had the highest classification accuracy across subjects. That is, the model was able to best detect an underlying temporal trend across the fmri data pertaining to the Visual network. It had a classification accuracy of 71.4% as shown in the table below. This was followed by the Somatomotor Network which has a test accuracy of 62.0%. In the first plot, the model accuracy for each of the different time blocks is shown, to get an idea of how the classifiers were performing across the duration of the movie. 


<img src="https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/2_Network_Models_FCN/Results/network_model_results_table.PNG" width="400" />

<img src="https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/2_Network_Models_FCN/Results/network-model-plot1.png" width="600" />

The model training was repeated for 10 runs so that error bars of the standard deviation, as well as the mean, could be displayed as below to get a sense of the consistency of each of the network model results. 

<img src="https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/2_Network_Models_FCN/Results/network-model-stats.png" width="600" />

## Code

### ``` 1_FCNs_vs_GCNs_fmri_classification```
- The [1_FCNs_vs_GCNs_fmri_classification](https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/tree/master/1_FCNs_vs_GCNs_fmri_classification) folder contains the code for evaluating the FCNs and GCNs on the fmri data. The models are trained on various block durations and their performance metrics compared. The main results were obtained in the following jupyter notebook;

#### ```1_fmri_gcns_main.ipynb```
- The models FCNs and GCNS (1st, 5th and 8th order) were trained and tested in this jupyter notebook.  The models were run for block durations of 6, 8 and 16 to both filtered and filtered + normalised fmri data. The model performance metrics such as the model accuracy were also determined here. The notebook made use of the additional class methods and fucntions from ```util_funcs.py``` and the models specified in ```models_fcns_gcns.py``` 

### ``` 2_Network_Models_FCN``` 
- The [2_Network_Models_FCN](https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/tree/master/2_Network_Models_FCN) folder contains the code for parcellating the fmri data into distinctive cognitive networks. The FCN model was then applied to the fmri data of each parcellated network separately. This was achieved with the following scripts;

#### ```1_Networks_Data.py```
- The [1_Networks_Data.py](https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/2_Network_Models_FCN/1_Networks_Data.py) script parcellates the fmri data into distinctive networks in the script

 #### ```2_Network_Models.py```

- The [2_Network_Models.py](https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/2_Network_Models_FCN/2_Network_models.py) script contains the class ```Network_Model()``` which involved training the model based on the parcellated data of each of the 7 networks including

  - The method ```create_network_data(self)``` which
    - Adds a column ```network``` to ```df_network``` which specifies the full name of the metric inferred from an abbreviation in the [Yeo parcellation file](https://github.com/ThomasYeoLab/CBIG/blob/master/stable_projects/brain_parcellation/Schaefer2018_LocalGlobal/Parcellations/MNI/Schaefer2018_400Parcels_7Networks_order.txt)
   - The method ```get_df_results_networks(self)``` which
      - Runs the FCN model using each network data indiviually

#### ```util_funcs.py```
The [util_funcs.py](https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/util_funcs.py) script contains various utility functions related to loading the fmri data 
- ```get_fmri_data(root_pth, task_type)```
    - Loads the fmri data from the root path, where ```task_type = (Movie, Rest)```, pertaining to the fmri movie data or resting state data
- The class ```Fmri_dataset(fmri_dataset, block_duration)``` which;
    - Has as input the fmri_dataset (either the train or test set) and the specified block duration which it uses to split the fmri into blocks
    - THe class includes the method ```split_fmri_blocks()``` which; 
    - Splits the fmri data into blocks of size block_duration, resulting in num_blocks blocks i.e num_blocks = total_time/block_duration. For example if block duration = 8, then number of blocks = 192/8 = 24 blocks 
    - Thus the fmri is returned split into equal sized blocks of size num_blocks x block_duration x ROIs
- ```get_rsfmri_adj_matrix(root_pth)```
    - Gets the resting state data & returns the  adjacency matrix required for the graphical neural network models
  

#### Models
#### ```models_fcn.py```
- The [models_fcn.py](https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/models_fcn.py) script contains the code for the Fully Connected Neural Network. 
    - The fmri data is averaged across it's time dimension before being inputed to the FCN, so that it goes from dimension;
        - Number of subjects x ROIs x Timepoints to dimension Number of subjects x ROIs 
    -  The FCN model has input layer created using the pytorch method ```nn.Linear``` of;
        - input size ```input_dim``` equal to the number of ROIs (400)
        - output size ```hidden_dim``` chosen as 128 in this instance
        - A ```ReLU``` activation layer is then added
        - Followed by a ```Dropout``` layer with ```self.dropout = 0.2``` 
    -  The pytorch method ```nn.Sequential``` is then used to create the hidden and final layer. The first hidden layer has
        - Input of size ```hidden_dim```
        - Output of size ```hidden_dim/4```
        - A ```ReLU``` activation layer is added
        - Followed by a ```Dropout``` layer with ```dropout = 0.5``` 
    - The final output layer then has
        - Input size ```hidden_dim/4```
        - Output size ```self.output_dim``` which is equal to the number of blocks in the fmri that need to be classified 

####  ```model_fcn_gcn.py```
- The [model_fcn_gcn.py](https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri/blob/master/1_FCNs_vs_GCNs_fmri_classification/models_fcn_gcn.py) script contains the code for the Fully Connected Neural Network and Graphical Neural Network models (In the folder ``` 2_Network_Models_FCN```)
    - The class ``` ChebNet(block_duration, filters, n_labels, gcn_layers = num_layers, dropout=0.25,gcn_flag=True)``` was used to create the GCN models. Note that 
        - ```filters``` = 32
        - ```gcn_layers``` = ```num_layers``` = 2
        - ```nlabels``` = Number of blocks (e.g 24 if block duration = 8, then 192/8 = 24)

## Using_the_Resource

#### Installation 
Open a terminal and type:

```
git clone https://github.com/hanmacrad2/FCNs_GCNs_classify_fmri.git
```

#### Prequisites
- The package requirements are written in the file ```requirements.text```
- These can me installed by typing the following in your terminal;

```
pip install -r requirements.txt 
```
- Note for ```pytorch```
  - python version 3.8 for torch install 
  - install torch with specific cpu and do same for scatter 

  - ```pip install torch==1.7.0+cu110 torchvision==0.8.1+cu110 torchaudio==0.7.0 -f https://download.pytorch.org/whl/torch_stable.html```

## Slides
Check out the presentation slides [here](https://drive.google.com/file/d/1KYRaSnbKZZZFt4FwQQW4UDNskGfAFH8f/view?usp=sharing)

## References 

Yeo, B. T., Krienen, F. M., Sepulcre, J., Sabuncu, M. R., Lashkari, D., Hollinshead, M., ... & Buckner, R. L. (2011). The organization of the human cerebral cortex estimated by intrinsic functional connectivity. Journal of neurophysiology.

Zhang, Y., Tetrel, L., Thirion, B., & Bellec, P. (2021). Functional annotation of human cognitive states using deep graph convolution. NeuroImage, 231, 117847.

Zhang, Y., & Bellec, P. (2019). Functional annotation of human cognitive states using graph convolution networks.


