# Crop Yield Prediction and Estimation using Time series remote sensing data.

# Objective:
We aim to build an ML model that will predict the yield of a crop using time series analysis of remote sensing data.

# Motivation behind doing this project:
According to a Harvard review, Food demand is expected to increase anywhere between 59% to 98% by 2050.
This will shape agricultural markets in ways we have not seen before. Farmers worldwide will need to increase crop production, either by increasing the amount of agricultural land to grow crops or by enhancing productivity on existing agricultural lands through fertilizer and irrigation and adopting new methods like precision farming.
However, the ecological and social trade-offs of clearing more land for agriculture are often high, particularly in the tropics. And right now, crop yields — the amount of crops harvested per unit of land cultivated — are growing too slowly to meet the forecasted demand for food.

There is a strong academic consensus that climate change-driven water scarcity, rising global temperatures, and extreme weather will have severe long-term effects on crop yields. These are expected to impact many major agricultural regions, especially those close to the Equator.

# How this project will help?
A farmer will be aware of the tentative amount of production he will obtain from his field.
This will help the policymakers of the state to determine the budget. If the production of a crop observes a declining trend then they can plan to implement the schemes at an early stage. This in return will save the state from shortage of a product.
The required import and export of a product can also be determined.
This will also help in monitoring the growth of healthy crops. 
Suggestive methods will help the farmer to increase the yield

# Layout:Part1: (for prediction of yield)
![alt text](https://github.com/mahimatendulkar/Crop-Yield-Prediction-and-Estimation-using-Time-series-remote-sensing-data./blob/master/layout1.png)





















# Layout: (Part2) (for suggesting methods for increasing yield)​



# Datasets available:

Below link will generate the following data fields(related to Goa state): 
Name of Crop, Year, Season, Area(hectares), Production(Tonnes), Yield(Tonnes/Hectare)
           https://aps.dac.gov.in/APY/Public_Report1.aspx

Google Earth Engine: Provides the datasets for the weather, Landsat and sentinel.
           https://developers.google.com/earth-engine/dat
           https://earthexplorer.usgs.gov/

IMD for weather and climate
           http://www.imd.gov.in/Welcome%20To%20IMD/Welcome.php




# Related studies:

Temperature extremes: Effect on plant growth and development 
By Jerry L. Hatfield and John H Prueger.​
(Journal of Sciencedirect)​

1. Warmer temperature with climate change causes an exponential decrease of the final yield.​

2. If we divide the whole plantation stages into two - reproductive phase (pollination) and growth phase, it’s found that the reproductive stage is one of the most sensitive stages. Temperature extreme at this stage will strongly affect the yield. ​

4. There is about 80 - 90 percent reduction in productivity on an increase of temperature for maize plants.​
​
5.  Effect of temperature on the plants increases on deficit or excess of water given to the crops.​
Thus, we found that deciding the date for planting crops such that the reproductive phase of plant will suffer less weather(temperature/precipitation) changes will increase the final output (aka yield) of the crops.​

 




# Data Extraction for Preprocessing and Data Analysis ​

 
(Fig1)
We can create Color gradient map giving a score to each color.​
Barren Land will be generally shown by brown color and fully grown land as dark green color.​
After selecting the region divide it into n*n grid and assign a score to it according to color gradient scale( Fig 1). 
The collected scores can then be arranged and stored in time series graph for a specific crop for multiple harvest ( 1 harvest  ~ 4 months )​
This is further divided into multiple stages according to the plant stages ( Pollination , Sowing, growth, harvesting stages , reproduction stage). (Fig 2)

  
(Fig 2)

(Fig 3)

We can then plot graph for each of the different types of Satellite images ( Temperature, crop-growth, C02, Precipitation, humidity, etc. )​
As for each species, there is also a graph for temperature ( min, optimum, max) in terms of growth at each stage ( Spl. For reproduction and growth stage) (Fig 3)
The above temperature graph is also useful for yield prediction.
After getting graph, we can find the relationship between each component and find out about how they change with time.

Note
Here, we considered whether, windspeed, precipitation etc are independent variable whereas remaining parameter such as crop growth, density are taken as dependent variables .​
It should be taken into consideration as we don’t have irrigation data ( water supply to plant by pump etc ). This problem is solved by setting threshold for each species of plant, and consider the farmers taking optimal decisions for now.​
Plant disease for now is not taken into consideration ​



Technology Stack used: Earth engine code editor (java script), Landsat 7 and Landsat 8 Imagery.















# Discussion:

# Test 1

Objective: we want to classify each pixel of the map based on three classes. These classes are selected based on the spectral output of each band (water, vegetation, and bare/constructed area). 


Image details:Above is the Landsat image. Points are marked manually
Purple point- Region of Interest
Red points- Urban region
Green points-Vegetation
Blue points- water

Approach: In this,(for feature collection) we have selected a set of points and given classes. We then trained the classifier taking 6 values for each (image) data from LANDSAT/LC8_L1T_TOA image collection between 2015-01-01 to 2015-12-31.   


Image Details: Red region is urban area, blue is water and green is vegetation

After learning weights for each class, we run the classifier to classify each pixel of the given region. This process can also be used for Specific crop detection at a specific time. 

Test 2

Objective: Detection of Crop using polygon rather than points as in test 1.

Approach:  We have drawn polygons to form 3 regions as in below image.

we have selected 3 regions as follows:
bare land, vegetation land and the water body (green quadrilateral patch is water, orange patch is bare land and brown quadrilateral patch is vegetation)

We are using Landsat 8 satellites having 11 spectral band observation capability with an average of 30 m resolution.  Each of the spectral bands consists of electromagnetic waves of a different wavelength. (Below table gives the range of wavelength for each band)






In above figure, we are extracting values of [b2, b3, b4, b5, b6, b7] .  We have selected b2 to b7 because we required visible spectra from blue to near infra-red (NIR). For each of the selected region, we were extracting the average band's value of that region. Thus, for each of them we will give values in a 6*1 matrix.   

After calculating the average value of all selected region, we plot by taking wavelength (w.r.t to the band) in the x-axis and average value as the y-axis. We found that for vegetation land, the mean values go higher at NIR region as compared to the water region. We also found water showing greater mean value at the blue visible spectrum compared to vegetation.   
We confirmed that vegetation lands emit more of the Green and NIR bands. 
We then colored each part of the map based on the observed spectrum. We allotted green for the vegetation, red for bare land and blue for water.  
We confirmed that this process can be used for computing vegetation of land using the NDVI Image collection of Landsat 7.   
This process can also be used to select a certain crop out of the collection of crops.  
Note: This process can be only be used for a variety of crops having significant Manhattan distance. 

Test 3

Objective: Predicting Yield using the NDVI spectral value

Approach:
The image data for the time duration of 2015-2017 was taken and a plot of Time V/s NDVI was made. For each year, it shows a peak NDVI.  
Normalized Difference Vegetation Index (NDVI) quantifies vegetation by measuring the difference between near-infrared (which vegetation strongly reflects) and red light (which vegetation absorbs).  
NDVI always ranges from -1 to +1. But there isn’t a distinct boundary for each type of land cover. 
NDVI is given by the following formula:


Above is a plot of time vs NDVI (for a particular region).

Peak NDVI indicates the maximum biomass (harvesting season) in the set region.  
Note1: dips in the curve are due to the cloud cover at the time when the image was taken. These can be avoided by smoothing of the curve. 
We can combine both the tasks and derive an average NDVI value of selected farm region. Based on a set of NDVI peak values and corresponding yield data of each peak value, we can form a regression model to predict the yield.
Note2: the yield of the crop field is only generated by measuring the spectrum of that region. We have not considered other factors such as Heat Signatures, Weather data, etc.  

Test 4:

Objective: Classifier to classify between urban region, vegetation, fields and water bodies.(here we are classifying between vegetation and fields as well).

Approach: The points where manually marked 

Image details: yellow points-water bodies, dark blue points- urban area, light blue points- vegetation, purple poins- fields.

The data was separated into train and test data.
Classifier was modeled.
Specific color was assigned to specific region.
Image was displayed.


Image details: Red region is the urban/non vegetation area, blue region is waterbody, yellow are fields and dark green region is Natural vegetation 

Test 5

Objective: Use of Clustering to Mask multiple region depending on their respective bands value.


Algorithm used:
Sample region was selected using 4 vertices.
Training data set was created.
Cluster was instantiated and trained.
Input was clustered using trained cluster.
Cluster was displayed with random colors.

The purpose of test 5 is to use semi supervised method to complete the same task as all the previous test are trying to archive. This method does not require human intervention to mask regions( ex. Baran, vegetation, water, industrial etc). This methods does so by assigning labels based on there bands values and there relatedness with each other.

In this method, we have used K mean unsupervised learning to assign label. Pixels having similar band values will be assigned to the same class.

Image details: After implementing clustering.(random colors where used)

We applied k mean unsupervised algorithm to the selected region. It has been seen that the k-mean cluster algorithm performs well in the region and shows significant results. On fine tuning the K-means clustering algorithm, the pipeline can be made to work perfectly and further analysis can be done.



Submitted by:
 Mahima Tendulkar
         (B.tech ECE final year student at NIT Goa) 
Aakash Kumar
(B.tech CSE final year student at NIT Goa)
