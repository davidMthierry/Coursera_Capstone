# IBM Data Science Professional Certificate Capstone Project 
This project aims to utilize all Data Science Concepts learned in the IBM Data Science Professional Course. We define a Business Problem, the data that will be utilized and using that data, we are able to analyze it using Machine Learning tools. In this project, we will go through all the processes in a step by step manner from problem designing, data preparation to final analysis and finally will provide a conclusion that can be leveraged by the business stakeholders to make their decisions.

# Table of Contents
- Introduction
- Target Audience
- Data Overview
- Methodology
- Discussion
- Conclusion

# Introduction
My report is for those who are planning to start a new restaurant. I'll try to provide as many suggestions on what all factors need to be considered while starting a new venture.

# Target Audience
Entrepreneurs who are passionate about opening a italian restaurant in a metropolitan city would be very interested in this project. The project is also for business owners and stakeholders who want to expand their businesses and wonder how data science could be applied to the questions at hand. 

#  Data Overview
The data that will be required will be a combination of CSV files that have been prepared for the purposes of the analysis from multiple sources which will provide the list of neighbourhoods in Toronto (via Wikipedia), the Geographical location of the neighbourhoods (via Geocoder package) and Venue data pertaining to Italian restaurants (via Foursquare). The Venue data will help find which neighbourhood is best suitable to open an Italian restaurant.

### Data Aquisition

#### Source 1: Wikipedia Table
The Wikipedia site provided almost all the information about the neighbourhoods. It included the postal code, borough and the name of the neighbourhoods present in Toronto. Since the data is not in a format that is suitable for analysis, scraping of the data was done. Wikipedia was fairly easy to scrape as it has a nice underlying structure. 

The top table is the table as it appeared on wikipedia, and the bottom table is the table as it appeared after scraping and using a DataFrame to contain the data. 

![Wikipedia Data](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/wikipedia_data.png) 

#### Source 2: Geographical Location Data
The second source of data provided us with the Geographical coordinates of the neighbourhoods with the respective Postal Codes. The file was in CSV format, so we could load it into a Pandas DataFrame.

![Geopraphical Data](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/geographical_data.png)

#### Source 3: Venue Data using Foursquare

Using examples from previous coursera instructional notebooks, we can adapt code to work for us. First we call the api using the displayed code and then load it into use using a Pandas DataFrame as well.

![Foursquare Data Code](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/foursquare_api_code.png)
![Foursquare Data](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/foursquare_data.png)

# Methodology
#### 4.1 - Data Cleansing
After all the data was collected and put into DataFrames, cleansing and merging of the data was required to start the process of analysis. When getting the data from Wikipedia, there were Boroughs that were not assigned to any neighbourhood therefore, the following assumptions were made:

- Only the cells that have an assigned a borough will be processed. Borough’s that were not assigned get ignored.
- More than one neighbourhood can exist in one postal code area. For example, in the table on the Wikipedia page, you will notice that M5A is listed twice and has two neighbourhoods: Harbourfront and Regent Park. These two rows will be combined into one row with the neighbourhoods separated with a comma.
- If a cell has a borough but a Not assigned neighbourhood, then the neighbourhood will be the same as the borough.

After the implementation of the following assumptions, the rows were grouped based on the borough as shown below.

![Methodology](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/methodology.png)
![TorontoVenue](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/toronto_venues.png)


#### 4.2 Data Exploration
Now after cleansing the data, the next step was to analyze it. We then created a map using Folium and colour-coded each Neighborhood depending on what Borough it was located in.

![Manhattan](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/manhattan.png)

Next, we used the Foursquare API to get a list of all the Venues in Toronto which included Parks, Schools, Café Shops, Asian Restaurants etc. Getting this data was crucial to analyzing the number of Italian Restaurants all over Toronto. There was a total of 45 Italian Restaurants in Toronto. We then merged the Foursquare Venue data with the Neighborhood data which then gave us the nearest Venue for each of the Neighborhoods.

![TorontoVenue](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/toronto_venues.png)

#### 4.3 Machine Learning
Then to analyze the data we performed a technique in which Categorical Data is transformed into Numerical Data for Machine Learning algorithms. This technique is called One hot encoding. For each of the neighbourhoods, individual venues were turned into the frequency at how many of those Venues were located in each neighbourhood.

![onehot](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/onehot_df.png)

Next, let's group rows by neighborhood and by taking the mean of the frequency of occurrence of each category

![grouped](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/to_grouped.png)

Now we will cluster the neighborhoods.

We will use k-means clustering. But first we will find the best K using the Elbow Point method.

![optimalk](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/optimum_k.png)

We see that the optimum K value is 4 so we will have a resulting of 4 clusters. 

![kmeanscode](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/kmeans_code.png)

So from here we run our K-Means algorithm with four clusters. After, we append the cluster results to the DataFrame as shown below. 

![to_merged](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/to_merged.png)

Lets check how many Italian Restaurant are there. After runing a short line of code, we see that there are a total of 40 locations with Italian Restaurants in Toronto.

We will create a new dataframe with the Neighborhood and Italian Restaurants

#### 4.4 Data Analysis
We have a total of 4 clusters (0,1,2,3). Before we analyze them one by one let's check the total amount of neighbourhoods in each cluster.

![npercluster](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/n_per_cluster.png)

From the bar graph that was made using Matplotlib, we can compare the number of Neighborhoods per Cluster. We see that Cluster 1 has 18 neighborhoods while cluster 2 has 10 neighborhoods (second most). Cluster 4 has 8 neighbourhoods and cluster 3 has only 6.

![rpercluster](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/r_per_cluster.png)

Then we compared the average Italian Restaurants per cluster. From the bar graph that was made using Matplotlib, we can compare the the average Italian Restaurants per cluster. We see that Cluster 1 has the least italian restaurants while cluster 2 has the most. Cluster 3 has around half the number of italian restaurants as cluster 4.

####  Cluster Analysis
This information is crucial as we can see that even though there is only 1 Italian restaurant in Cluster 1, it has the highest number of neighborhoods (0.1304) while Cluster 2 has the least neighborhoods but has the highest average of Italian Restaurants (0.0009). Also, from the map, we can see that neighborhoods in Cluster 2 are the most sparsely populated. Now let us analyze the clusters one by one.

#### Cluster 1: Red
Cluster 1 was mainly in the East Toronto Area. First Canadian Place, Underground city, Richmond, Adelaide, King were among some neighborhoods that were in that cluster. Cluster 1 had 173 unique Venue locations and out of those only 1 were Italian Restaurants. Cluster 1 had the lowest average of Italian Restaurants equating to 0.0.

![C1](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/cluster1.png)

#### Cluster 2: Yellow

There was a total of 140 neighbourhoods, 63 different venues and 9 Italian Restaurants. Therefore,the average amount of Italian Restaurants that were near the venues in Cluster 2 is the highest being 0.07. In the map and from our numbers, we can see that nodes of Cluster 2 were dispersed mostly evenly throughout Toronto making it one of the most sparsely populated clusters.

![C2](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/cluster2.png)

#### Cluster 3: Blue 
Cluster 3 had the second to lowest average of Italian Restaurants. Cluster 3 was mainly located in the Downtown area but also had some neighbourhoods in West Toronto, East Toronto and in North York. Neighbourhoods such as Ryerson, Toronto Dominion Center, Garden District, Queen’s Park and many more were included in this cluster. There was a total of 162 unique venues and out of those 20 were Italian Restaurants.

![C3](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/cluster3.png)

#### Cluster 4: Green 
Cluster 4 venues were located in the Downtown, West, East Toronto areas. Neighbourhoods such as Central Bay Street, St. James Town, Cabbagetownwere some of the neighbourhoods that made up this cluster. There were a total of 91 unique Venues in Cluster 4 with 10 Italian Restaurants. This made up the second-highest average of Italian Restaurants in that cluster which was approximately 0.047.

![C4](https://github.com/davidMthierry/Coursera_Capstone/blob/master/imgs/cluster4.png)

# Discussion
Most of the Italian Restaurants are in cluster 2 represented by the yellow clusters. The Neighborhoods located in the East Toronto area that have the highest average of Italian Restaurants are The Danforth West and Riverdale. Even though there is a huge number of Neighborhoods in cluster 1, there is little to no Italian Restaurant. We see that in the Downtown Toronto area (cluster 3) has the second last average of Italian Restaurants. Looking at the nearby venues, the optimum place to put a new Italian Restaurant in cluster 1 as there are many Neighborhoods in the area but little to no Italian Restaurants, therefore, eliminating any competition. The second-best Neighborhoods that have a great opportunity would be in areas such as which is in Cluster 4. Having 90 neighbourhoods in the area with 10 Italian Restaurants gives a good opportunity for opening a new restaurant, relative to cluster 3 where there are more neighborhoods and more italian restaurants.

Some of the drawbacks of this analysis are — the clustering is completely based on data obtained from the Foursquare API. Also, the analysis does not take into consideration of the Italian population across neighbourhoods as this can play a huge factor while choosing which place to open a new Italian restaurant.

This concludes the optimal findings for this project and recommends the entrepreneur to open an authentic Italian restaurant in these locations such as Cluster 1 and can expect little to no competition.

# Conclusion
In conclusion, to end off this project, we had an opportunity on a business problem, and it was tackled in a way that it was similar to how a genuine data scientist would do. We utilized numerous Python libraries to fetch the information, control the content and break down and visualize those datasets. We have utilized Foursquare API to investigate the settings in neighbourhoods of Toronto, get a great measure of data from Wikipedia which we scraped with the Beautifulsoup Web scraping Library. We also visualized utilizing different plots present in seaborn and Matplotlib libraries. Similarly, we applied AI strategy to anticipate the error given the information and utilized Folium to picture it on a map.

Places that have room for improvement or certain drawbacks give us that this project can be additionally improved with the assistance of more information and distinctive Machine Learning strategies. Additionally, we can utilize this venture to investigate any situation, for example, opening an alternate cuisine or opening of a Movie Theater and so forth. Ideally, this task acts as an initial direction to tackle more complex real-life problems using data science.