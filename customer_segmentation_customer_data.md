---
title: "Customer Segmentation using Principle Component Analysis and K-means Clustering"
output: 
  html_document:
    toc: true
    highlight: tango
    theme: paper
    keep_md: true
    df_print: paged
  html_notebook:
    highlight: tango
    theme: paper
date: "2023-07-26"
tags:
  - customer segmentation
  - marketing recommendations
  - unsupervised learning
authors:
  - Andres Cojuangco
updated_at: "2023-08-03"
---
### Project Objectives

<b> Apply customer segmentation on customer retail data to: </b>

- Develop customer profiles
- Understand each profile's spending behavior 
- Find out each profile's preferred purchasing channel
- Provide recommendations to the marketing team and analytics team

---

### Data Description
The data set is provided by Dr. Omar Romero-Hernandez, a researcher and professor at U.C. Berkeley's Haas School of Business and the Hult International Business School. Unfortunately, there is no information provided on how the data was collected. The link to it can be found here [reference card](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis?datasetId=1546318&sortBy=voteCount&language=R).       
The data set contains 31 columns and 2240 rows, and can be thematically divided into 4 sections: 

* **Customer Attributes (9 columns)**,
  + *ID:* Customer's unique identifier (int)
  + *Year_Birth:* Customer's birth year (int)
  + *Education:* Customer's education level (obj)
  + *Marital_Status:* Customer's marital status  (obj)
  + *Income:* Customer's yearly household income (int)
  + *Kidhome:* Number of children in customer's household (int)
  + *Teenhome:* Number of teenagers in customer's household (int)
  + *Dt_Customer:* Date of customer's enrollment with the company (obj)
  + *Recency:* Number of days since customer's last purchase (int)
  
* **Customer Behavior (12 columns)**:
  + *MntWines:* Amount spent on wine in last 2 years (int)
  + *MntFruits:* Amount spent on fruits in last 2 years (int)
  + *MntMeatProducts:* Amount spent on meat in last 2 years (int)
  + *MntFishProducts:* Amount spent on fish in last 2 years (int)
  + *MntSweetProducts:* Amount spent on sweets in last 2 years (int)
  + *MntGoldProds:* Amount spent on gold in last 2 years (int)
  + *NumWebPurchases:* Number of purchases made through the company's web site (int)
  + *NumCatalogPurchases:* Number of purchases made using a catalogue (int)
  + *NumStorePurchases:* Number of purchases made directly in stores (int)
  + *NumWebVisitsMonth:* Number of visits to company's web site in the last month (int)
  + *NumDealsPurchases:* Number of purchases made with a discount (int)    
  + *Complain* 1 if customer accepted the offer in the last campaign, 0 otherwise
  
* **Customer Response to Marketing Activations (6 columns)**:
  + *AcceptedCmp1:* 1 if customer accepted the offer in the 1st campaign, 0 otherwise (int)
  + *AcceptedCmp2:* 1 if customer accepted the offer in the 2nd campaign, 0 otherwise (int)
  + *AcceptedCmp3:* 1 if customer accepted the offer in the 3rd campaign, 0 otherwise (int)
  + *AcceptedCmp4:* 1 if customer accepted the offer in the 4th campaign, 0 otherwise (int)
  + *AcceptedCmp5:* 1 if customer accepted the offer in the 5th campaign, 0 otherwise (int)
  + *Response:* 1 if customer accepted the offer in the last campaign, 0 otherwise (int)     

* **Channels (4 columns)**:
  + *NumWebPurchases*: Number of purchases made through the company’s web site (int)
  + *NumCatalogPurchases*: Number of purchases made using a catalogue (int)
  + *NumStorePurchases*: Number of purchases made directly in stores (int)
  + *NumWebVisitsMonth*: Number of visits to company’s web site in the last month (int)
  




<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["ID"],"name":[1],"type":["int"],"align":["right"]},{"label":["Year_Birth"],"name":[2],"type":["int"],"align":["right"]},{"label":["Education"],"name":[3],"type":["chr"],"align":["left"]},{"label":["Marital_Status"],"name":[4],"type":["chr"],"align":["left"]},{"label":["Income"],"name":[5],"type":["int"],"align":["right"]},{"label":["Kidhome"],"name":[6],"type":["int"],"align":["right"]},{"label":["Teenhome"],"name":[7],"type":["int"],"align":["right"]},{"label":["Dt_Customer"],"name":[8],"type":["chr"],"align":["left"]},{"label":["Recency"],"name":[9],"type":["int"],"align":["right"]},{"label":["MntWines"],"name":[10],"type":["int"],"align":["right"]},{"label":["MntFruits"],"name":[11],"type":["int"],"align":["right"]},{"label":["MntMeatProducts"],"name":[12],"type":["int"],"align":["right"]},{"label":["MntFishProducts"],"name":[13],"type":["int"],"align":["right"]},{"label":["MntSweetProducts"],"name":[14],"type":["int"],"align":["right"]},{"label":["MntGoldProds"],"name":[15],"type":["int"],"align":["right"]},{"label":["NumDealsPurchases"],"name":[16],"type":["int"],"align":["right"]},{"label":["NumWebPurchases"],"name":[17],"type":["int"],"align":["right"]},{"label":["NumCatalogPurchases"],"name":[18],"type":["int"],"align":["right"]},{"label":["NumStorePurchases"],"name":[19],"type":["int"],"align":["right"]},{"label":["NumWebVisitsMonth"],"name":[20],"type":["int"],"align":["right"]},{"label":["AcceptedCmp3"],"name":[21],"type":["int"],"align":["right"]},{"label":["AcceptedCmp4"],"name":[22],"type":["int"],"align":["right"]},{"label":["AcceptedCmp5"],"name":[23],"type":["int"],"align":["right"]},{"label":["AcceptedCmp1"],"name":[24],"type":["int"],"align":["right"]},{"label":["AcceptedCmp2"],"name":[25],"type":["int"],"align":["right"]},{"label":["Complain"],"name":[26],"type":["int"],"align":["right"]},{"label":["Z_CostContact"],"name":[27],"type":["int"],"align":["right"]},{"label":["Z_Revenue"],"name":[28],"type":["int"],"align":["right"]},{"label":["Response"],"name":[29],"type":["int"],"align":["right"]}],"data":[{"1":"5524","2":"1957","3":"Graduation","4":"Single","5":"58138","6":"0","7":"0","8":"04-09-2012","9":"58","10":"635","11":"88","12":"546","13":"172","14":"88","15":"88","16":"3","17":"8","18":"10","19":"4","20":"7","21":"0","22":"0","23":"0","24":"0","25":"0","26":"0","27":"3","28":"11","29":"1","_rn_":"1"},{"1":"2174","2":"1954","3":"Graduation","4":"Single","5":"46344","6":"1","7":"1","8":"08-03-2014","9":"38","10":"11","11":"1","12":"6","13":"2","14":"1","15":"6","16":"2","17":"1","18":"1","19":"2","20":"5","21":"0","22":"0","23":"0","24":"0","25":"0","26":"0","27":"3","28":"11","29":"0","_rn_":"2"},{"1":"4141","2":"1965","3":"Graduation","4":"Together","5":"71613","6":"0","7":"0","8":"21-08-2013","9":"26","10":"426","11":"49","12":"127","13":"111","14":"21","15":"42","16":"1","17":"8","18":"2","19":"10","20":"4","21":"0","22":"0","23":"0","24":"0","25":"0","26":"0","27":"3","28":"11","29":"0","_rn_":"3"},{"1":"6182","2":"1984","3":"Graduation","4":"Together","5":"26646","6":"1","7":"0","8":"10-02-2014","9":"26","10":"11","11":"4","12":"20","13":"10","14":"3","15":"5","16":"2","17":"2","18":"0","19":"4","20":"6","21":"0","22":"0","23":"0","24":"0","25":"0","26":"0","27":"3","28":"11","29":"0","_rn_":"4"},{"1":"5324","2":"1981","3":"PhD","4":"Married","5":"58293","6":"1","7":"0","8":"19-01-2014","9":"94","10":"173","11":"43","12":"118","13":"46","14":"27","15":"15","16":"5","17":"5","18":"3","19":"6","20":"5","21":"0","22":"0","23":"0","24":"0","25":"0","26":"0","27":"3","28":"11","29":"0","_rn_":"5"},{"1":"7446","2":"1967","3":"Master","4":"Together","5":"62513","6":"0","7":"1","8":"09-09-2013","9":"16","10":"520","11":"42","12":"98","13":"0","14":"42","15":"14","16":"2","17":"6","18":"4","19":"10","20":"6","21":"0","22":"0","23":"0","24":"0","25":"0","26":"0","27":"3","28":"11","29":"0","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

## Data Preparation and Exploratory Data Analysis

This section of the code aims to use the dplyr package to clean and recode data for Principle Component Analysis (PCA) and K-means clustering and deal with missing values. To do this, univariate analysis on each feature was conducted to identify variables that are not needed, as well as to identify and remove outliers (an important step for k-means clustering). The analysis includes exploring the responses and spread of each feature with boxplots, bar graphs, and histograms. Then, any features with ordinal/nominal data were turned into factors. 

### Exploring Missing Data 

![](Figs/missing_data-1.png)<!-- -->![](Figs/missing_data-2.png)<!-- -->

Since the missing data in the Income variable comprises only 1.1% of the data, we can omit these rows using list wise deletion. Moreover, the purpose of Z_CostContact and Z_Revenue remain unclear so these columns will be omitted.



### Exploring ID


---------------------------
 total_rows   total_unique 
------------ --------------
    2216          2216     
---------------------------

Table: Checking unique values in ID

Since the number of unique IDs are equal to the number of total rows, there are no repeats of IDs in the data. Thus, each row represents one unique customer, meaning the IDs will not be needed for the clustering.



### Exploring Age from Year_Birth



![](Figs/unnamed-chunk-3-1.png)<!-- -->

There are some people who are way too old to be true data so we will remove outliers. 



![](Figs/histogram_age-1.png)<!-- -->

Age seems evenly distributed.

### Exploring Education

![](Figs/explore_education-1.png)<!-- -->




We will convert the variable Education into a factor of three levels: "1", which is a receiver of basic education, "2", which is an undergraduate degree-holder, and "3", which is a higher education degree-holder. Convertinng them into numeric factors will make it more useable during PCA and k-means clustering.

### Exploring Marital Status

![](Figs/explore_marital-1.png)<!-- -->



We will filter out unusual answers in variable Marital_Status that were likely answered as a joke. We then collapsed similar responses into a factor with two levels: "1", which indicates one is not legally married, and "2", which indicates one is married.

![](Figs/factor_marital-1.png)<!-- -->

### Exploring Income

![](Figs/explore_income-1.png)<!-- -->

There are extreme outliers that need to be removed.



![](Figs/histograsm_income-1.png)<!-- -->

Income seems evenly distributed.

### Exploring Recency

![](Figs/unnamed-chunk-8-1.png)<!-- -->

Recency seems evenly distriibuted.

### Combining Kidhome and Teenhome



![](Figs/explore_kids-1.png)<!-- -->

The families range from 0 to 3 kids. Now, we will remove original features like Kidhome and Teenhome now that they are combined into Kids.



### Exploring Total Expenses, Total Frequency, and Total Accepted Campaigns 



![](Figs/unnamed-chunk-11-1.png)<!-- -->

Total expenses and Accepted campaigns appear to be skewed to the right while total frequency seems evenly distributed.

### Exploring Dt_customer

[1] "Earliest join date is 2012-07-29 17:00:00"
[1] "Latest join date is 2014-06-28 17:00:00"

![](Figs/explore_dt_customer-1.png)<!-- -->

The dates of enrollment seem evenly distributed. Although it seems to be suggesting enrollment or registration to a program or newsletter that we know nothing about. Therefore, it will be removed.



###  Exploring Complain

-----------------
 Complain    n   
---------- ------
    0       2178 

    1        20  
-----------------

Table: Counts of Complains

Since only 20 people have complained in the last two years, this feature won't be useful.



### Exploring Response


-----------------
 Response    n   
---------- ------
    0       1868 

    1       330  
-----------------

Table: Counts of Response to Last Campaign

Since we do not know the nature of the last campaign, we can remove this feature as well.



The resulting data frame after cleaning looks like this.
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Age"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["Education"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Marital_Status"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Income"],"name":[4],"type":["int"],"align":["right"]},{"label":["Recency"],"name":[5],"type":["int"],"align":["right"]},{"label":["MntWines"],"name":[6],"type":["int"],"align":["right"]},{"label":["MntFruits"],"name":[7],"type":["int"],"align":["right"]},{"label":["MntMeatProducts"],"name":[8],"type":["int"],"align":["right"]},{"label":["MntFishProducts"],"name":[9],"type":["int"],"align":["right"]},{"label":["MntSweetProducts"],"name":[10],"type":["int"],"align":["right"]},{"label":["MntGoldProds"],"name":[11],"type":["int"],"align":["right"]},{"label":["NumDealsPurchases"],"name":[12],"type":["int"],"align":["right"]},{"label":["NumWebPurchases"],"name":[13],"type":["int"],"align":["right"]},{"label":["NumCatalogPurchases"],"name":[14],"type":["int"],"align":["right"]},{"label":["NumStorePurchases"],"name":[15],"type":["int"],"align":["right"]},{"label":["NumWebVisitsMonth"],"name":[16],"type":["int"],"align":["right"]},{"label":["AcceptedCmp3"],"name":[17],"type":["int"],"align":["right"]},{"label":["AcceptedCmp4"],"name":[18],"type":["int"],"align":["right"]},{"label":["AcceptedCmp5"],"name":[19],"type":["int"],"align":["right"]},{"label":["AcceptedCmp1"],"name":[20],"type":["int"],"align":["right"]},{"label":["AcceptedCmp2"],"name":[21],"type":["int"],"align":["right"]},{"label":["Kids"],"name":[22],"type":["int"],"align":["right"]},{"label":["total_expenses"],"name":[23],"type":["int"],"align":["right"]},{"label":["total_frequency"],"name":[24],"type":["int"],"align":["right"]},{"label":["total_accepted"],"name":[25],"type":["int"],"align":["right"]}],"data":[{"1":"66","2":"2","3":"1","4":"58138","5":"58","6":"635","7":"88","8":"546","9":"172","10":"88","11":"88","12":"3","13":"8","14":"10","15":"4","16":"7","17":"0","18":"0","19":"0","20":"0","21":"0","22":"0","23":"1617","24":"25","25":"0","_rn_":"1"},{"1":"69","2":"2","3":"1","4":"46344","5":"38","6":"11","7":"1","8":"6","9":"2","10":"1","11":"6","12":"2","13":"1","14":"1","15":"2","16":"5","17":"0","18":"0","19":"0","20":"0","21":"0","22":"2","23":"27","24":"6","25":"0","_rn_":"2"},{"1":"58","2":"2","3":"2","4":"71613","5":"26","6":"426","7":"49","8":"127","9":"111","10":"21","11":"42","12":"1","13":"8","14":"2","15":"10","16":"4","17":"0","18":"0","19":"0","20":"0","21":"0","22":"0","23":"776","24":"21","25":"0","_rn_":"3"},{"1":"39","2":"2","3":"2","4":"26646","5":"26","6":"11","7":"4","8":"20","9":"10","10":"3","11":"5","12":"2","13":"2","14":"0","15":"4","16":"6","17":"0","18":"0","19":"0","20":"0","21":"0","22":"1","23":"53","24":"8","25":"0","_rn_":"4"},{"1":"42","2":"3","3":"2","4":"58293","5":"94","6":"173","7":"43","8":"118","9":"46","10":"27","11":"15","12":"5","13":"5","14":"3","15":"6","16":"5","17":"0","18":"0","19":"0","20":"0","21":"0","22":"1","23":"422","24":"19","25":"0","_rn_":"5"},{"1":"56","2":"3","3":"2","4":"62513","5":"16","6":"520","7":"42","8":"98","9":"0","10":"42","11":"14","12":"2","13":"6","14":"4","15":"10","16":"6","17":"0","18":"0","19":"0","20":"0","21":"0","22":"1","23":"716","24":"22","25":"0","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

## Conducting Principal Component Analysis (PCA) 

Principal component analysis (PCA) is a technique that transforms high-dimensions data into lower-dimensions while retaining as much information as possible. It uses linear algebra to identify the most important features in a data set that contribute a data set's variance. Thus, it will be helpful in determining key features that serve as points of differentiation among the segments produced from k-means clustering.

In this section, we use FactoMineR to conduct PCA. First, we need save a copy of our main data frame where linear combinations of the features are removed. Combinations such as total expenses, frequency, and accepted will not be useful for PCA. Then, we standardize the data and plot a correlation matrix with it.





![](Figs/cor_matrix-1.png)<!-- -->

Lastly, we conduct the PCA analysis.


![](Figs/pca_viz-1.png)<!-- -->![](Figs/pca_viz-2.png)<!-- -->

The first 10 principle components explain 95% of the variance. The top 10 contributors to the first 10 principal components are income, amount spent on meat, sweets, fish, wines, and fruits, the number of web visits per month, the number of kids, and the number of in-store and catalog purchases. The top 10 contributors will help differentiate the clusters later on during the construction of the customer profiles in the discussion section.

## Applying K-means Clustering

In this section, we use the k-means clustering algorithm to segment the customers. First, we need to find out how many clusters, k, we need using a WSS plot, silhouette analysis, and an ensemble function called NBClust.

![](Figs/cluster_hyperparam-1.png)<!-- -->![](Figs/cluster_hyperparam-2.png)<!-- -->



For the first plot, the sum of squared errors seems to slow down from three clusters to four clusters. This implies a k=3. However, the silhouette analysis and the ensemble clustering algorithm, which runs 30 different kind of hyper parameter (k) tests, insists on a k=2. Thus, two clusters will be used. Now, let's run the k-means clustering algorithm with k=2 and visualize the results.

![](Figs/clustering-1.png)<!-- -->

The results are two clusters that are almost linearly separable .



## Results: Cluster Analysis 

### Customer Attributes/Demographics

![](Figs/cluster_age-1.png)<!-- -->



![](Figs/cluster_education-1.png)<!-- -->

![](Figs/cluster_marital-1.png)<!-- -->

![](Figs/cluster_income-1.png)<!-- -->


----------------------
 cluster   avg_income 
--------- ------------
    1        38620    

    2        71952    
----------------------

Table: Median Income per Cluster


![](Figs/cluster_kids-1.png)<!-- -->

![](Figs/cluster recency-1.png)<!-- -->

### Customer Behavior: Product purchases and Channels

![](Figs/cluster_purchases-1.png)<!-- -->

![](Figs/cluster_channels-1.png)<!-- -->

![](Figs/unnamed-chunk-22-1.png)<!-- -->

### Customer Totals over 2 Years: Frequency, Cumalitive Expenses, Number of Campaigns Accepted, and Expenses per Product

![](Figs/unnamed-chunk-23-1.png)<!-- -->




![](Figs/unnamed-chunk-25-1.png)<!-- -->


### Cluster 1 Breakdown:

#### Attributes
- Much lower income compared to Cluster 2 with a median average of $38,620
- Median age is 53, similar spread to Cluster 2
- 66% are married
- 89% have kids, majority have 1 kid at home
- 4% have higher level education
- Similar even spread to Cluster 2

#### Behavior

- Much lower spend across all product categories,
- Average total expenses in last two years is $96
- Much lower purchase frequency than Cluster 2 with median of 9 purchases
- Average spend per item is $19.14
- Top 2 products that attracted most spending are meat and wine

#### Channels
- Lower average number of store purchases compared to Cluster 2 with a median of 3
- Barely any catalog purchases, median of 1
- Barely any deals purchases, higher average than Cluster 2, median of 2
- Lower number of web purchases than Cluster 2, with a median of 2
- Visit website more than Cluster 2 with a median of 7 web visits

#### Campaigns
- Only 11% accepted campaigns, nobody accepted more than 3 campaigns

### Cluster 2 Breakdown:

#### Attributes
- Much higher income compared to Cluster 2 with a median average of $71,952
- Median age is 55, similar spread to Cluster 1
- 62% are married
- 58% do not have kids and the majority of the ones that do have 1 kid
- Nobody has higher level education
- Similar even spread to Cluster 1

#### Behavior
- Much higher spend across all product categories
- Average total expenses in last two years is $1,192 
- Much higher purchase frequency than Cluster 1 with median of 22 purchases
- Average spend per item is $58.07
- Top 2 products that attracted most spending are meat and Wine

#### Channels
- Higher average number of store purchases compared to Cluster 1 with a median of 9
- Fair number of catalog purchases, median of 5
- Barely any deals purchases, lower average than Cluster 1, median of 1
- Higher number of web purchases than Cluster 1 with a median of 5
- Visit website less than Cluster 1 with a median of 3 web visits

#### Campaigns
- 37% accepted campaigns

## Discussion and Recommendations

**NOTE: The Top 10 contributors in the PCA (Income, Kids, Meats, wines, fruits, sweets, fish, catalog purchases, in-store purchases, and website visits) were considered as points of differentiation when developing the customer profiles**

#### <b>Cluster 1 - the Impulse Buyers:</b>

Majority of Cluster 1 have significantly less spending power than Cluster 2 and have more kids at home. Their average spend is $96 with an average of 9 purchases in the last two years. To add, these customers mostly shop in-store with only three in-store purchases versus an average of two online purchases in the last two years. Interestingly,they spend more time visiting the website compared to Cluster 2 but check-out less. Moreover, this cluster has minimal response to campaigns, deals, and catalogs. Based on these, Cluster 1 could be considered impulse buyers that do not have a thought out shopping list or are simply passersby that may be comparing prices. This may be further supported by their low average spend per item of 
$19.14. Location or inconvenience may be an issue since these people may be living too far a way from the store in order to buy in-store regularly. It is also possible that price is an issue since these customers have larger families and lower average income.

#### <b>Cluster 1 Recommendations:</b>

- Understand the obstacles to purchasing on the website and in-store. Overall, a great place to start would be investigating if price and location are relevant factors that are limiting the conversions. Analysts can do correlation analysis, hypothesis testing, or, if possible, A/B testing with different price levels to understand the impact of price on conversions. Furthermore, understanding where these customers are visiting from (distance or fuzzy location) and their customer journey both online and in-store are worth looking into. By understanding these factors, we can decide how much resources is worth spending to acquire these customers, increase conversions, and retain them.

- Make improvements to the website to become easier to navigate, more intriguing, and attractive. Since this cluster mostly browses the website, we need to make sure that this group is landing on relevant page and has access to customer support. Having a seamless experience and innovating product display or recommending systems in browsing pages may serve a good investment. Removing any intrusive elements such as pop-ups or irrelevant ads from the customer journey may also prove to be helpful. One possible solution would be to direct them towards items on sale, time-sensitive deals, or providing discount codes if possible.


#### <b>Cluster 2 - the Loyal Customers:</b>

Cluster 2 has high spending power. Majority are married but more than half of the customers do not have children, which is 31% less than Cluster 1. Their average spend is $1,192 with an average of 22 purchases in the last two
years. They spend the most on wine and meat. Similar to Cluster 1, shopping in-store is the most popular channel with an average of 9 purchases in the last two years. However, Cluster 2 has higher average of web purchases but a minimal number of web visits. This may imply that Cluster 2 has higher interest  in making web purchases than Cluster 1 but are less reliant on frequent visits to the website. It is also possible that awareness about the website within this cluster is low. Moreover, this cluster was more responsive to campaigns and catalogs but not deals. Based on these, Cluster 2 could be considered the loyal customers. This group purchases more frequently and spends $58.07 per item on average which is 3 times more than Cluster 1's average spend per item. They are also less responsive to deals. Thus, unlike Cluster 1, location, inconvenience, and price are likely to be less of an issue since they have higher income and less people to support.


#### <b>Cluster 2 Recommendations:</b>

- Understand their loyalty by connecting with customers. Offer incentives such as discounts or inclusive offers in-store to motivate them to take the time to share their positive or negative feedback. Another way to get organic feedback about what they like about the brand is hosting community-centered events that align with the brand's values and interests.

- Investigate the minimal number of web visits through awareness and customer location. Understanding their awareness of the website and its usefulness to these customers will give us a glimpse of how an online platform can be utilized to streamline the customer journey. One possible strategy could be to promote the website and its benefits in-store. Asking about their awareness during in-person purchases may also provide a better idea of how known the website is and its usefulness. However, it is possible that these customers may live nearby the store and have no use for ordering online or deliveries. Through further analysis, ideas such as a online purchase & pick-up feature may serve to be useful for this group, shortening the customer journey. 

- Since this cluster spends the most on wine and meats, creating online and in-store campaigns to promote complementary sales of these products can be done. One possible option could be to place these two items closer to each other within the store. Another solution could be to investigate the likelihood of purchasing a product given that a certain item is already in their basket.


## Personal Learnings

One of the main challenges was understanding how to integrate PCA and k-means clustering together and there are many approaches to do this. One approach is using the clustering algorithm on the PCA scores. Another approach is joining the PCA scores to the cleaned data set and applying the clustering on the resulting data set. In this project, the k-means clustering was used on the cleaned data set while the PCA was used to find out the most significant contributors to the data. This methodology's advantage is interpretability since each of the features in the cleaned data set are straightforward to understand. On the other hand, interpreting PCA scores is not as easy without several graphs and sufficient mathematical knowledge. Thus, this project used PCA to understand the most significant contributors to the variance in the data set in order to serve as significant points of differentiation among the clusters. In the future, it may be worth looking into the difference in silhouette score for each possible approach to check if there are significant differences in the strength of data used for clustering

Another challenge was organizing graphs for numerous features. With the amount of features that needed to be cleaned and analyzed, it was important to keep the markdown file organized. I learned the importance of labeling r chunks and utilizing r markdown's global functions, like knitr, to keep track of the document and generate a clean html file. Another important learning was using ggplot2 to create compelling graphs that were properly labeled and visually appealing. Dividing the features into themes provided some structure to the order of processing and analysis the data. To add, using functions like facet wrap to group graphs shortened the code book. Moreover separating the code for data manipulation and the code for visualization appears to be good practice during testing. This prevents any errors from occurring when changes are made to a saved variable, making the code modular. As I embark on more projects, I will keep in mind to develop a style of code that keeps visualization and organization consistent and easy to read.



