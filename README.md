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
   \
   2.1 Annual mean trend \
   We firstly clustered the 9-year annual mean trends for 317 cities using K-mean clustering. The annual mean PM2.5 concentrations are normalized for each city by
   $$conc_{norm} = \frac{conc-conc_{min}}{conc_{max}-conc_{min}} $$\
   2.2 Seasonal trend based on monthly trend \
   We further clustered the seasonal trends for 317 cities using K-mean clustering. The seasonal trend component of the 9-year PM2.5 temperal profiles for each city was derived following https://www.geeksforgeeks.org/seasonality-detection-in-time-series-data/. 
   In brief, the total monthly temperal variations were expressed as the summation of long-term trend, seasonal trend and residuals. The long-term trend was fitted using a polynomial function. \
\
   The cluster numbers were determined based on the distortion elbow figure (distortion vs. cluster numbers), as shown in Fig. 1a and 1d. Ultimately, we generated three clusters for the annual mean trend and four clusters for the seasonal trend. We evaluated our clustering results using the Silhouette Coefficient (Fig. 1b and 1e) and the Davies-Bouldin Index (Fig. 1c and 1f). The Silhouette Coefficients were greater than 0.2 for both clustering scenarios. Considering the ratio of feature numbers to sample numbers (9 vs. 317) and the dispersion level of our data, our clustering results are acceptable.
4. Supervised method: 
We will also use features or factors including VOCs emission, chemistry indicator and meteorological parameters to train a prediction model to predict the PM2.5 temporal profiles and seasonal variation patterns. For the prediction model, we will use both tree-based method and artificial neural network. For the tree-based method, we will choose to use the gradient boosted tree method. For the artificial neural network, we will used the long-term memory structure. These methods are known to be effective on the time series prediction. 
Here is the list of the methods and functions we used/plan to use in this project:

|Method	                    |   Libraries	 |   Function               |
| --------------------      | -------------- | -----------------------  |
|K-mean	                    |   scikit-learn |	sklearn.cluster.KMeans  |
|Gradient Boost Tree Method	|   XGBoost	     |  XGBoost.XGBRegressor    |
|Long Short Term Memory	    |   torch	     |  torch.nn.LSTM           |

## Results and Discussion
1. K-mean clustering results \
1.1 annual trend \
We generated two main clusters from the annual PM2.5 trends of 317 cities, with a third cluster representing outliers that could not be represented by the two main clusters. The annual trends for each cluster center are shown in Fig. 2a-c, while Fig. 2d displays the spatial distribution of the clustering results for all 317 cities. For cities in Cluster 1, PM2.5 mass concentration declined continuously from 2014 to 2022, due to persistent emission reduction measures implemented by the Chinese government. However, there was a rebound in PM2.5 concentrations in 2023 for these cities. Cluster 1 includes most of the cities in the provinces of Heilongjiang, Jilin, Liaoning, Beijing, Hebei, Shandong, Henan, Hunan, Hubei, Zhejiang, Sichuan, Chongqing, Tibet, Ningxia, and Qinghai. \
\
In contrast, cities in Cluster 2 showed a decline in PM2.5 concentration only after 2017, with a similar rebound in 2023. The delayed decrease in PM2.5 concentration for Cluster 2 might be related to different chemistry regimes, pollutant emission structures and delayed implementation of emission reduction practices in provinces such as Shanxi, Jiangsu, Anhui, and many southern provinces. \
\
The remaining cities in Cluster 3 (outliers) did not show a clear decrease in PM2.5 concentration over the past nine years, particularly evident in many cities in Yunnan province. Overall, the annual trends for PM2.5 concentrations in 317 cities in China showed distinct spatial patterns from 2014 to 2023. Further investigation into the reasons for this phenomenon will be discussed in supervised model section. \
\
1.2 Seasonal trend \
Figure 3 shows the seasonal trends for each cluster center and the spatial distribution of each cluster. For Cluster 1, PM2.5 mass concentration decreased from January to August and increased after August. For Cluster 2, PM2.5 concentration peaked in March and reached its lowest levels from June to August. For Cluster 3, the highest PM2.5 concentration was found in January, while the lowest concentrations were in July and August. For Cluster 4, PM2.5 concentration peaked in December and January and reached its lowest in June. \
\
These four clusters exhibit distinct spatial patterns. Cluster 3 includes most of the cities in central China and Xinjiang. Cluster 4 encompasses cities in southern China, mainly in Hainan, Guangdong, Guangxi, Jiangxi, and Fujian provinces. Cluster 2 includes cities in Yunnan province and several coastal cities in Fujian province. Cluster 1 represents most of the cities in northeastern, northwestern China, and Jiangsu province. \
The different seasonal patterns reflect varying meteorological conditions, emission structures, and chemical regimes across different regions, which will be further addressed in the supervised model section. \
\
Figure Titles: \
Figure 1. Evaluation for K-mean clustering of PM2.5 concentration annual mean trend (a-c) and seasonal trend (d-f) \
Figure 2. PM2.5 concentration annual mean trend centers for each clusters (a-c) and spatial distribution of clustering results (d) \
Figure 3. PM2.5 concentration seasonal trend centers for each clusters (a-d) and spatial distribution of clustering results (e)


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
