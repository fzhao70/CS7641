# CS7641-Project

## Introduction/Background:
Air pollution, especially suspended particulate matter (PM) pollution, has been significantly mitigated in China over the past 10 years through continuous government emission regulation efforts. However, the specific achievements have varied from city to city due to the complex influences of emission reduction, atmospheric chemistry regimes, regional characteristics, and meteorological patterns.

A dominant fraction of PM2.5 particles are secondarily formed in the atmosphere, with volatile organic compounds (VOCs) such as toluene and isoprene as the primary precursors. VOCs are predominantly of biogenic origin (BVOCs) at a global scale, while anthropogenic sources (AVOCs) can also dominate in densely populated cities. The formation of PM2.5 involves a multitude of complex atmospheric chemical reactions, with oxidants species and nitrogen oxides playing a central role.

## Problem Definition 
In this project, we collected hourly in situ measurements of fine particulate matter (PM2.5) mass concentration over 2030 sites in 193 cities across China from June 2014 to May 2024 from China National Environmental Monitoring Center (CNEMC, https://quotsoft.net/air/). We would compile an extensive dataset, including BVOC emissions  inventories from CAMS global emission inventories(https://ads.atmosphere.copernicus.eu/), AVOC emissions from the Multi-resolution Emission Inventory model for Climate and air pollution research (http://meicmodel.org.cn/), gas pollutant concentrations from CNEMC, and meteorological fields from ECMWF Reanalysis v5 (https://www.ecmwf.int/en/forecasts/dataset/ecmwf-reanalysis-v5) data. These features represent the central factors that impact the PM2.5 pollution, i.e., precursor concentration, atmospheric chemistry regime, meteorological transfer.


1. Analyze the primary factors contributing to the reduction in PM2.5 concentrations in China over the past ten years and evaluate the effectiveness of air pollutant emission reduction regulation.
2. Investigate the seasonal variation patterns of PM2.5 concentrations in different cities and identify potential pattern change with time.
3. Identify the regional patterns by clustering different cities regarding of PM2.5 concentration trend and seasonal variation patterns.

## Methods

We first processed the hourly PM2.5 concentration data to daily average and only used days with at least 17 hours of data. We further derived the monthly and seasonal mean from the daily averages. Then, we will use both unsupervised and supervised learning methods. We will first apply the K-mean clustering method on the PM2.5 time series to group the mode of the PM2.5 temporal profiles. We will use both L2 norm and the Dynamic Time Warping to conduct clustering. Dynamic Time Warping is a similarity measure between time series.
We will also use features or factors including VOCs emission, chemistry indicator and meteorological parameters to train a prediction model to predict the PM2.5 temporal profiles and seasonal variation patterns. For the prediction model, we will use both tree-based method and artificial neural network. For the tree-based method, we will choose to use the gradient boosted tree method. For the artificial neural network, we will used the long-term memory structure. These methods are known to be effective on the time series prediction. 
Here is the list of the methods and functions we plan to use in this project:

|Method	                    |   Libraries	 |   Function               |
| --------------------      | -------------- | -----------------------  |
|K-mean	                    |   scikit-learn |	sklearn.cluster.KMeans  |
|Gradient Boost Tree Method	|   XGBoost	     |  XGBoost.XGBRegressor    |
|Long Short Term Memory	    |   torch	     |  torch.nn.LSTM           |

## (Potential) Results and Discussion

## References

### Contribution Table
|Name	                    |   Contribution   |
| --------------------      | -------------- | 
|Bin Bai	                |   Introduction/Background & Problem Definition & Reference |	
|Fanghe Zhao	            |   Method &  Results and Discussion |
|Shengjun Xi	            |   Method &  Results and Discussion | 