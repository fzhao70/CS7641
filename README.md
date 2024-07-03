# CS7641 Project : Machine Learning on $PM_{2.5}$ pattern variation

## Introduction/Background:
Air pollution, especially suspended particulate matter (PM) pollution, has been significantly mitigated in China over the past 10 years through continuous government emission regulation efforts. However, the specific achievements have varied from city to city due to the complex influences of emission reduction, atmospheric chemistry regimes, regional characteristics, and meteorological patterns.

A dominant fraction of PM2.5 particles are secondarily formed in the atmosphere, with volatile organic compounds (VOCs) such as toluene and isoprene as the primary precursors. VOCs are predominantly of biogenic origin (BVOCs) at a global scale, while anthropogenic sources (AVOCs) can also dominate in densely populated cities. The formation of PM2.5 involves a multitude of complex atmospheric chemical reactions, with oxidants species and nitrogen oxides playing a central role.

## Problem Definition 
In this project, we collected hourly in situ measurements of fine particulate matter (PM2.5) mass concentration in 317 cities across China from 2015 to 2023 from China National Environmental Monitoring Center (CNEMC, https://quotsoft.net/air/). We would compile an extensive dataset, including BVOC emissions  inventories from CAMS global emission inventories(https://ads.atmosphere.copernicus.eu/), AVOC emissions from the Multi-resolution Emission Inventory model for Climate and air pollution research (http://meicmodel.org.cn/), gas pollutant concentrations from CNEMC, and meteorological fields from ECMWF Reanalysis v5 (https://www.ecmwf.int/en/forecasts/dataset/ecmwf-reanalysis-v5) data. These features represent the central factors that impact the PM2.5 pollution, i.e., precursor concentration, atmospheric chemistry regime, meteorological transfer.

The main objectives of this project are:

1. Analyze the primary factors contributing to the reduction in PM2.5 concentrations in China over the past ten years and evaluate the effectiveness of air pollutant emission reduction regulation.
2. Investigate the seasonal variation patterns of PM2.5 concentrations in different cities and identify potential pattern change with time.
3. Identify the regional patterns by clustering different cities regarding of PM2.5 concentration trend and seasonal variation patterns.

## Methods
1. data collection and preprocessing
We first processed the hourly PM2.5 concentration data to daily average and only used days with at least 17 hours of data. We further derived the monthly and annual mean from the daily averages.
2. Unsupervised method: K-means clustering \
   Here we used L2 norm to conduct clustering, i.e., the Euclidean distance was used. We didn't use Dynamic Time Warping (DTW) although DTW is a similarity measure between time series. This is because our temperal profiles are synchronous which doesn't allow for stretching and translation.
   2.1 Annual mean trend \
   We firstly clustered the 9-year annual mean trends for 317 cities using K-mean clustering. The annual mean PM2.5 concentrations are normalized for each city by
   $$conc_{norm} = \frac{conc-conc_{min}}{conc_{max}-conc_{min}} $$\
   2.2 Seasonal trend based on monthly trend \
   We further clustered the seasonal trends for 317 cities using K-mean clustering. The seasonal trend component of the 9-year PM2.5 temperal profiles for each city was derived following https://www.geeksforgeeks.org/seasonality-detection-in-time-series-data/. 
   In brief, the total monthly temperal variations were expressed as the summation of long-term trend, seasonal trend and residuals. The long-term trend was fitted using a polynomial function.
3. Supervised method: 
We will also use features or factors including VOCs emission, chemistry indicator and meteorological parameters to train a prediction model to predict the PM2.5 temporal profiles and seasonal variation patterns. For the prediction model, we will use both tree-based method and artificial neural network. For the tree-based method, we will choose to use the gradient boosted tree method. For the artificial neural network, we will used the long-term memory structure. These methods are known to be effective on the time series prediction. 
Here is the list of the methods and functions we used/plan to use in this project:

|Method	                    |   Libraries	 |   Function               |
| --------------------      | -------------- | -----------------------  |
|K-mean	                    |   scikit-learn |	sklearn.cluster.KMeans  |
|Gradient Boost Tree Method	|   XGBoost	     |  XGBoost.XGBRegressor    |
|Long Short Term Memory	    |   torch	     |  torch.nn.LSTM           |

## Results and Discussion

Davies-Bouldin Index would be used for clustering evaluation. In the prediction, we will use the common regression metrics provided in the scikit-learn, which will have mean absolute error, mean squared error and coefficient of determination.

We aim to illustrate the factors dominate PM2.5 trends of China in the past 10 years. Regional differnces in season variation and impacting factors are expected.

## References

1.	Z. Ma, R. Liu, Y. Liu, and J. Bi, "Effects of air pollution control policies on PM2.5; pollution improvement in China from 2005 to 2017: a satellite-based perspective," Atmospheric Chemistry and Physics, vol. 19, no. 10, pp. 6861-6877, 2019, doi: 10.5194/acp-19-6861-2019.

2.	S. Guo et al., "Elucidating severe urban haze formation in China," Proc Natl Acad Sci U S A, vol. 111, no. 49, pp. 17373-8, Dec 9 2014, doi: 10.1073/pnas.1419604111.

3.	S. Zhao, D. Yin, Y. Yu, S. Kang, D. Qin, and L. Dong, "PM(2.5) and O(3) pollution during 2015-2019 over 367 Chinese cities: Spatiotemporal variations, meteorological and topographical impacts," Environ Pollut, vol. 264, p. 114694, Sep 2020, doi: 10.1016/j.envpol.2020.114694.

4.	R. J. Huang et al., "High secondary aerosol contribution to particulate pollution during haze events in China," Nature, vol. 514, no. 7521, pp. 218-22, Oct 9 2014, doi: 10.1038/nature13774.

5.	M. Shrivastava et al., "Recent advances in understanding secondary organic aerosol: Implications for global climate forcing," Reviews of Geophysics, vol. 55, no. 2, pp. 509-559, 2017, doi: 10.1002/2016rg000540.

6.	Z. Wu et al., "Aerosol Liquid Water Driven by Anthropogenic Inorganic Salts: Implying Its Key Role in Haze Formation over the North China Plain," Environmental Science & Technology Letters, vol. 5, no. 3, pp. 160-166, 2018, doi: 10.1021/acs.estlett.8b00021.


### Timeline


| Task                                     |   Contributor           |  6/1 - 6/10 | 6/10 - 6/20 |  6/20 - 6/30 |  6/30 - 7/10 |
| Find the Topic                           |   All                   |  &#x2611;     |             |              |              |
| Find the source of the data              |   Shengjun, Fanghe      |  &#x2611;     |             |              |              |
| Implement the data preprocess            |   Shengjun, Fanghe      |             |   &#x2611;    |   &#x2611;     |              |
| Implement Machine Learning Method K-Mean |   Shengjun, Fanghe      |             |   &#x2611;    |              |              |
| Implement Machine Learning Method GBTM   |   Shengjun, Fanghe      |             |  &#x2611;     |   &#x2611;     |              |
| Implement Machine Learning Method LSTM   |   Shengjun, Fanghe      |             |             |     &#x2611;   |              |
| Evaluation of Macine Learning Model      |   Shengjun, Fanghe      |             |             |    &#x2611;    |   &#x2611;     |
| Visualizations                           |   Bin                   |             |             |   &#x2611;     |   &#x2611;     |
| Write the report                         |   Bin                   |             |    &#x2611;   |   &#x2611;     |    &#x2611;    |

### Proposal Contributions

|Name	                    |   Contribution                                             |
| --------------------      | --------------                                             | 
|Bin Bai	                |   Introduction/Background & Problem Definition & Reference |	
|Fanghe Zhao	            |   Method &  Results and Discussion                         |
|Shengjun Xi	            |   Method &  Results and Discussion                         | 
### Midterm Contributions

|Name	                    |   Contribution                                             |
| --------------------      | --------------                                             | 
|Bin Bai	                |   Method and Result Discussions & Visualization & Report Writting |	
|Fanghe Zhao	            |   Data Peprocessing & Model Train & Method and Result Discussions                    |
|Shengjun Xi	            |   Data Peprocessing & Method and Result Discussions                       | 
