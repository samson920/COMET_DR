# COMET: Clinical and Omics Multi-Modal Analysis Enhanced with Transfer Learning -- Diabetic Retinopathy
COMET is a machine learning framework that incorporates large, observational electronic health record (EHR) databases and transfer learning to improve the analysis of small datasets from omics studies. For more detail, please see our publication describing the development of COMET [here](https://www.nature.com/articles/s42256-024-00974-9)
## Overview
This repo contains the code used for the analyses and results presented in our manuscript focused on the analysis of diabetc retinopathy. The EHR data cannot be shared publicly due to Stanford University policies. However, the code in this repository can be used to analyze similarly structured data for researchers who have access to omics and EHR data.
## Installation and Setup
First, clone the GitHub repo:
```
git clone https://github.com/samson920/COMET_DR
```
Then, set up the environment:
```
conda env create -f environment.yml
conda activate COMET
```
The installation should take about 10 minutes.

## Code Details
The notebook contains the code used for our machine learning modeling. To apply the code to your own data, the following files would be needed:
### Omics Data Files
- ./data/processed/processed_proteomics.csv: This file should be tabular omics data with rows representing patients and columns representing proteins (or measurements of other analytes). It should be of dimension n_patients, num_omics_analytes_measured.
### Baseline Data Files (i.e. patients who have both EHR and omics data)
- ./data/processed/RNN_data_omics_cohort_disease_modeling.npy: This file should contain the EHR input to the RNN for the patients with both EHR and omics data. The EHR data in this file are embedded using a word2vec model trained on only the patients with both EHR and omics data. The file should be of dimension n_patients, max_days, embedding_dim. 
- ./data/processed/RNN_data_outcomes_omics_cohort_disease_status.npy: This file should contain the binary disease label for the patients in the omics cohort. It should be of dimension n_patients.
- ./data/processed/RNN_data_lengths_omics_cohort_disease_status.npy: This file should contain the lengths of the EHR sequences for the patients in the omics cohort (used for padding in the RNN). It should be of dimension n_patients.
- ./data/processed/sampleID_indices_omics_cohort_disease_modeling.csv: This file should contain the patient ID and corresponding index of that patient's data in the other files. It is primarily used to ensure all data are correctly aligned. It should be dimension n_patients, 2.
### Baseline Data Files (i.e. patients who have both EHR and omics data), processed with pre-trained word2vec model
The following files are identical to those above, except the EHR codes are embedded using the word2vec model trained with ALL available EHR data. These files are used to fine-tune the pre-trained model and take advantage of the fact that we can also learn a more powerful word2vec model by pre-training with a large amount of EHR data.
- ./data/processed/RNN_data_omics_cohort_disease_modeling_PT_word2vec_model.npy
- ./data/processed/RNN_data_outcomes_omics_cohort_disease_status_PT_word2vec_model.npy
- ./data/processed/RNN_data_lengths_omics_cohort_disease_status_PT_word2vec_model.npy
- ./data/processed/sampleID_indices_omics_cohort_disease_modeling_PT_word2vec_model.csv
### Pretraining Data Files (i.e. patients who have only EHR data)
The following files are identical to those above, except it should contain data from patients with only EHR data (generally orders of magnitude more than the omics patients) as these data are used for pre-training.
- ./data/processed/RNN_data_full_EHR_cohort_disease_classification.npy
- ./data/processed/RNN_data_outcomes_full_EHR_cohort_disease_classification.npy
- ./data/processed/RNN_data_lengths_full_EHR_cohort_disease_classification.npy
- ./data/processed/sampleID_indices_full_cohort_disease_classification.csv
