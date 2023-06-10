# MOF-Capstone
Power generation is one of the leading greenhouse gas emission sources and a key driver of climate change. Metal-organic frameworks (MOFs) are nanoporous materials that are great candidates for carbon capture and sequestration (CCS) processes due to their highly tuneable chemical and structural properties. My goal in this project was to use the [ARC-MOF](https://zenodo.org/record/6908728#.ZB22pHbMI2w) database of calculated MOF properties to generate a predictive model that can help steer chemists in the lab towards the best possible carbon capture material. The final Light GBM model I developed provided the following recommendations:
* It is most important to target pore limiting and largest cavity diameters of 0-10 angstroms
* Unit cell volume should be minimized
* Probe accessible volume fraction should fall within 40-50%

## 1. [Data](https://github.com/zjbrown0689/MOF-Capstone/blob/main/notebooks/1_data_wrangling_ARC_mof.ipynb)
Five of the datasets provided in the ARC-MOF database were joined to combine topology, geometry, radial distribution function (RDF), process, and revised autocorrelation (RAC) features. Volumetric carbon dioxide working capacity was used as my target metric, and any other metric features were dropped.

## 2. Method
A wide range of regression models were trained on 80% of the data, of which XGBoost and Light GBM performed the best. After hyperparameter tuning both models Light GBM continued to outperform XGBoost, with a final root mean squared error of 22.83 when tested against the 20% test data sample. The feature importances of this model were used to develop the recommendations for MOF development.

## 3. [Feature Engineering](https://github.com/zjbrown0689/MOF-Capstone/blob/main/notebooks/3_preprocessing.ipynb)
After dropping any dependent features from the dataset such as gas adsorption values, selectivity, etc, the only categorical feature - topology - was dummied. The dataset was then split into 80% train and 20% test sets. A standard scaler was then trained on the train set and applied to both train and test. The same procedure was used with a most frequent simple imputer. Finally the dataset was restricted to the 450 most important features as determined using f-regression.

## 4. [Exploratory Data Analysis](https://github.com/zjbrown0689/MOF-Capstone/blob/main/notebooks/2_exploratory_data_analysis.ipynb)
Exploratory data analysis found multiple non-linear correlations with volumetric working capacity, including pore limiting diameter, largest cavity diameter, and accessible surface area. It was also identified that of the topologies present in 20 or more MOFs, bct had the highest average working capacity and the tightest distribution of values. 

## 5. [Model Results](https://github.com/zjbrown0689/MOF-Capstone/blob/main/notebooks/4_model_development.ipynb)
After testing a wide range of models using default hyperparameters, five were hyperparameter tuned on 10% of the data. Light GBM and XGBoost performed the best throughout the entire modeling process, with Light GBM maintaining a constant slight advantage over XGBoost. After a final round of hyperparameter tuning the resultant Light GBM regressor was trained on 80% of the dataset and had root mean squared error of 22.83 when tested on the 20% testing set. This represented an almost 18-fold improvement over a mean dummy regressor. The 20 most important features were then plotted to compare against EDA in effort to derive recommendations.

## 6. Future Work
Going forward I have two paths I'd like to explore: digging deeper, and collaborating. In terms of digging deeper, I'd like to find additional datasets that can be added to this work to help find if these models can be improved further, and possibly identify other MOF features which may help improve this guidance. I also think it would be very interesting to collaborate with MOF research groups to actually design and synthesize some MOFs based on these recommendations to put them to the test. There are many challenges to synthesizing MOFs, so I would like to see if we could actually design and then create a MOF that should theoretically perform well as a CCS adsorbent.

## 7. Credits
I'd like to first and foremost thank my Springboard mentor Upom Malik for helping me find this dataset and develop this project idea. Finding MOF datasets that could be aligned to develop a machine learning project was quite a challenge, and his help was invaluable. I'd also like to thank the [ARC-MOF authors](https://pubs.acs.org/doi/10.1021/acs.chemmater.2c02485) for their work developing this database. This project wouldn't be possible without their efforts.
