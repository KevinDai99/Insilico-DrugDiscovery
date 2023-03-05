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

The Lipinski descriptors as seen above, is a rule of thumb for the druglikeness of a compound. 
Statistical Signficance present for all Lipinski Values. This is expected from active vs inactive compounds 
