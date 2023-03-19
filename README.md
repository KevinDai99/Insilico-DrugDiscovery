# Insilico-DrugDiscovery

## Introduction

Traditional drug discovery and development is cost and time intensive, with an average of 10-15 years of R&D, and an estimated cost of $58.8 billion USD[1]. To give a perspective, of 10 000 screened chemical compounds, only 250 will move to clinical testing [2]. Further, out of drugs that pass Phase I of clinical trials, only around 11.83% are approved for market [3]. The consequence is that drug discovery has ceased for some therapeutical areas such as antibiotics [4]. The high cost and failure rates of the traditional path have prompted the need for computational machine learning to aid drug discovery. In this project I will use a machine learning model for discovery and will start with extraction, collection, and cleaning of data from the chEMBL database!

The chEMBL database contains curated bioactivity data of more than 2 million compounds. It is compiled from more than 76,000 documents, 1.2 million assays, spans 13,000 targets, 1,800 cell types and 33,000 indications34. Suffice to say, the volume of the chEMBL dataset makes working in Excel difficult. Nevertheless, the first step is to install the ChEMBL web service package so that the bioactivity data can be retrieved. 

In this project, I will developing a model for predicting whether a molecule is a good candidate for targeting Acute Myeloid Leukemia (AML). AML is a type of blood cancer in which the bone marrow makes a large number of adnormal blood cells. Mutations in the FMS-like tyrosine kinase 3 (FTL3) gene are detected in approximately 30% of AML pateints, making it a promising therapeutic target[5]. The project is divided in three parts:

1) Query bioactivity data for FTL3
2) Cleaning and exploring bioactivity data
    * Merging Lipininski Descriptors
    * Conducting statistical 

3) Developing a machine learning model to evaluate drug candidates

## Exploring Data Results

In the data exploration and cleaning step - bioactiviy class was assigned - and statistical test were done to confirm that active vs in-active compounds can be used for ML. 

![image](https://user-images.githubusercontent.com/89043234/222947169-a5f586ac-50cc-4789-be34-5d079bf292bb.png)

|   | Descriptor    | results   | pvalue        | interpretation                     |
|---|---------------|-----------|---------------|------------------------------------|
| 0 | pIC50         | 33.402186 | 1.937665e-152 | Different distribution (reject H0) |
| 1 | MW            | 7.251568  | 9.960885e-13  | Different distribution (reject H0) |
| 2 | LogP          | 3.647737  | 2.821961e-04  | Different distribution (reject H0) |
| 3 | NumHDonors    | 3.379502  | 7.623462e-04  | Different distribution (reject H0) |
| 4 | NumHAcceptors | 5.194867  | 2.618861e-07  | Different distribution (reject H0) |

**Figure 1.  Limpinski descriptor shows significant difference between active and inactive compounds**

The Lipinski descriptors as seen above, is a rule of thumb for the druglikeness of a compound. 
Statistical Signficance present for all Lipinski Values. This is expected from active vs inactive compounds 

## Methodology

1) Padel was used calculate the molecular fingerprints from the SMILES
2) PCA analysis was used to reduce the number of features as seen below - with a cutoff at 95% cumulative variance. 

![image](https://user-images.githubusercontent.com/89043234/223281694-e144b2ca-f976-4ced-8c08-a594baf61038.png)

## Results 

### Mutiple Linear Regression 

![image](https://user-images.githubusercontent.com/89043234/223282259-7dd24ca6-0414-431e-9b1b-289bab457d40.png)

**Figure 2. Mutiple Linear Regression Model**

After training and testing, mutiple linear regression model did not predict actual pIC50 well, with an r2 score of 0.26. This is expected as the relationship between molecular fingerprints and pIC50 is non-linear. 

### Random Forest Tree - Ensemble Learning 

![image](https://user-images.githubusercontent.com/89043234/223282328-b889e6ea-7f01-4493-81d8-cb4e46187c99.png)

**Figure 3. Random Forest Tree Model**

The random forest tree algorithim predicted actual pIC50 better than mutiple linear regression with a r2 score of 0.41 after hyper-optimization. The MSE was 0.83 compared to the 0.14 MSE when predicting on x_train. This suggest that the model is not overfitted and is able to generalize to unseen data. 

However, the r2 score of 0.41 may be improved by using other models such neural networks or collecting more data. 

## Neural Network (MPL Regressor)

| Neural Network | Predicted R^2 Value on Test | Predicted R^2 Value on Train |
|----------------|-----------------------------|------------------------------|
| One Layer      | 0.35                        | 0.72                         |
| Two Layer      | 0.41                        | 0.81                         |
| Three Layer    | 0.55                        | 0.86                         |

**Figure 4. A Neural Network with three layers performs better than linear regression and random forest, but is overfitted**

Based on Figure 4 Neural Network with three layesr performs better than random forest tree and linear regression. However, the difference between predicted R^2 on test and train suggest that the neural network model is not able to generalize to new data, and warrants L2 regularization.

![image](https://user-images.githubusercontent.com/89043234/226152060-2a430c6d-b8e6-439a-8443-fc9d09e4684c.png)

**Figure 5. An Alpha value of 2.00 reduces overfitting relative to other values**

Figure 5. illustrates that an alpha value of 2.00 has the lowest the difference between R^2 on test and train of 0.54, and 0.83 respectively. 

![image](https://user-images.githubusercontent.com/89043234/226152002-3d95330c-7c9b-47ba-ba9e-faa9d2b9e467.png)

**Figure 6. Neural Network Model after Regularizations**

## Conclusion

In conclusion, out of the three models tested, the random forest tree algorithim may perform the best on novel data, as even after regularization, the neural network is likely overfitted. The random forest tree is also able to explain how it reached it prediction, whereas the neural network is a black-box model. Methods to increase the accuracy of the random forest model should be pursued, whether that be gaining more data, or certain molecular fingerprints be left out. 



