## IPO ML Project Final Report

### Team Members

Jun Chen

Joshua Hess

Zhening Xu

Owen Reece

Hunter Germundsen

[Link to Project Proposal](https://ipo-project-ml-2021.github.io/)

[Link to Midterm Report](https://ipo-project-ml-2021.github.io/MidtermReport)

[Link to Github Code Repository](https://github.com/jchen706/pub_ml_project)

# Introduction/Background

For day traders and stock brokers, one of the most intriguing investments are companies selling their stock publicly for the first time (IPO, or Initial Public Offering). Because these stocks are new, and the companies relatively unproven, IPO stocks are much more volatile than stocks for established companies. With the “extraordinarily high variability of initial returns” [1] there is increased opportunity alongside this increased risk; using machine learning techniques we will try to best capitalize on this volatility by predicting whether an IPO stock will increase or decrease in the first 5 business days of trading. In an average year there are only 100-200 IPOs, however in 2020, there were over 400 IPOs with many expecting there to be higher volumes of IPOs going forward [2]. As of April 27th, 2021 there have been 442 IPOs up over 900% from 2020 which itself was a high volume year [3]. With more IPOs each year and each IPO having high potential for returns, IPO trading may become a staple for day traders in the near future. 

# Problem Definition

With the controversy surrounding wall street and the recent spikes of particular stocks, companies have more reason than ever to be fearful of going public. We seek to create a program to predict the change-in-price of an IPO after its first week on the market, to help smaller companies determine if going public is the right move for them.

# Data Collection

We retrieved our IPO data using Selenium and BeautifulSoup. We used a Firefox webdriver to scrape IPO dates and which companies and their prices from NASDAQ’s IPO Calendar. During this first process, there were timeout errors which caused Selenium Timeout errors and StaleElementException in our program which we used an iterative point algorithm which we kept a list of error months and years and keep iterating through these months and years until there is no error. We also scraped the link to each company on this Nasdaq website to go to a secondary site that has more information about the company. For example, Applovin’s company page on the Nasdaq website provided by the href of the <a> tag on the IPO Calendar. We scrape from the year January 2009 to December 2020. Within that 11 year span, we have an initial collection of around 1557 companies with features of ticker, exchange, share price, shares offered, dollar value of shares, and date of the IPO price which is shown in Figure 2. Some companies have After that we use the company link provided by Nasdaq and scrape that page for the number of employees, shares_outstanding,stock_valuation,company_address, and phone as shown in Figure 3. Companies with less than 10 employees were dropped from the dataset to take out SPAC companies (blank check companies that don’t have business and are for the purpose of merging a private company and bringing that company to the public). We are going back and looking through these 1557 companies to add the ones that are not SPAC. After this process, there were 1083 companies left in the data frame. The next process is fetching the ebit (earnings before interest and tax), gross profit (company’s profit before interest and taxes), net income (sales minus cost of goods sold), total revenue (total sales of goods and services), cost revenue (total cost of manufacturing and delivering a product), total assets (total economic value owned by the company), revenue_growth (one year annual revenue compared to the previous year revenue) which we use the edgar-online.com api using https://datafied.api.edgar-online.com/v2/corefinancials/ann?primarysymbols=etsy&numperiods=10&appkey=<appkey\> as shown in Figure 4. A number of companies were not in the database thus they were omitted in this step which left the number of companies to 449. Another scenario was parsing straight from the SEC database which has pdf files in html format which is still under development. The last step is using Selenium and BeautifulSoup to get the first five day closing price from Marketwatch.com from the initial IPO date here. While scraping this data some companies don’t have a table of their historical stock price which was null or 0 as the share prices for the five days as shown in Figure 5.. After this step, we move on to part 2 of the Data Cleaning.  We converted the field “sicdiscription” to a numeric value called (sic_num) based on the [Wikipedia Standard Industrial Classification](https://en.wikipedia.org/wiki/Standard_Industrial_Classification#:~:text=The%20Standard%20Industrial%20Classification%20(SIC,agencies%20to%20classify%20industry%20areas)) site which were represented in the below table. This data is shown in Figure 6 which is used in the DBSCAN. Figure 6 also shows the day 1 change in price, day 2 change in price, day 3 change in price, and day 4 change in price which is just day X+1 price minus day X price which X is the day such as 1, 2, 3, 4, 5.


SIC Number | SIC Description
-----------|------------------
0  | Agriculture, Forestry and Fishing
 1 | Mining  
2 | Construction 
3 | Manufacturing
4 | Transportation, Communications, Electric, Gas and Sanitary service
5 | Wholesale Trade
6 | Retail Trade
7 | Finance, Insurance and Real 
8 | Services
9 | Public Administration
10 | Nonclassifiable
11 | Medical
12 | Education
13 | Tech/Computer Related/ Engineering

Figure 1. Standard Industry Classification Conversion from Number to Description

![Figure2](https://user-images.githubusercontent.com/46770398/116352888-da6d7580-a7c3-11eb-87f8-c8d4889d4d0e.png)

Figure 2. Data Collection Processing Image 1

![Figure3](https://user-images.githubusercontent.com/46770398/116352943-f113cc80-a7c3-11eb-8435-c496111607f3.png)

Figure 3. Data Collection Processing Image 2

![Figure4](https://user-images.githubusercontent.com/46770398/116352990-0983e700-a7c4-11eb-9719-2dd7150f5f5d.png)

Figure 4. Data Collection Processing Image 3

![Figure5](https://user-images.githubusercontent.com/46770398/116353003-10125e80-a7c4-11eb-8879-1ad06a4a52a8.png)

Figure 5. Data Collection  Processing Image 4

![Figure6](https://user-images.githubusercontent.com/46770398/116353017-14d71280-a7c4-11eb-8e16-4aed67adbb80.png)

Figure 6: Data Collection  Processing Image 5

# Methods

### DBSCAN:
For the DBSCAN, we set the eps to 0.7 which means that distance between two points will have to be lower than or equal that value to be considered neighbors. We set the minimum points to 10. We used Sklearn’s DBSCAN function and plotted the fit of the model for the dataset whose input is representative of Figure 6. The results we got were as follows shown in the below. In the Industry Number which is the Standard Industrial Number vs Day 4 , Day 3, Day 2, and Day 1 Percentage Changes in Price, there was only 1 cluster shown as in Plot 1, Plot 2, Plot 3, and Plot 4. Such can be caused by the data having inconclusive data which the numbers for the percentages are scattered for all the day changes which is random so there is only one cluster. The same situation happened with the Initial Price Offering vs Day 4 , Day 3, Day 2, and Day 1 Percentage Changes in Price as shown in Plot 5, Plot 6, Plot 7, Plot 8. There was 1 cluster 0 and 30 dollars vs the Percentage changes. In the 3D scatter plots for industry Number, Revenue Growth, and Day Percentage Changes in Price in Plot 9, Plot 10, Plot 11, and Plot 12, there were outliers for around 0 percentage change and 1500% revenue growth, this maybe a data error caused by the Edgar Online API. The model was inconclusive for the clustering between the fields of  industry number , percentage change in price, initial price offering and revenue growth . Only 1 cluster presented in Scatter 2D and had 406 outliers out of 449 , so our data collected or fields could not determine relationships among the fields.

![Plot1](https://user-images.githubusercontent.com/46770398/116353042-1d2f4d80-a7c4-11eb-8c10-539bfaff6fb7.png)

Plot 1. DBSCAN plot between Industry Number and Day 4 Percentage Change in Price

![Plot2](https://user-images.githubusercontent.com/46770398/116353050-215b6b00-a7c4-11eb-9145-f00b502d1339.png)

Plot 2: DBSCAN plot between Industry Number and Day 3 Percentage Change in Price

![Plot3](https://user-images.githubusercontent.com/46770398/116353062-26201f00-a7c4-11eb-983f-0e0cccdbb3b0.png)

Plot 3: DBSCAN plot between Industry Number and Day 2 Percentage Change in Price

![Plot4](https://user-images.githubusercontent.com/46770398/116353068-291b0f80-a7c4-11eb-8daa-33f76515afab.png)

Plot 4: DBSCAN plot between Industry Number and Day 1 Percentage Change in Price

![Plot5](https://user-images.githubusercontent.com/46770398/116353081-2d472d00-a7c4-11eb-9c51-5241e5e3c884.png)

Plot 5: DBSCAN plot between Initial Price Offering and Day 2 Percentage Change in Price

![Plot6](https://user-images.githubusercontent.com/46770398/116353092-30421d80-a7c4-11eb-9dc5-dc355c3de6ce.png)

Plot 6: DBSCAN plot between Initial Price Offering and Day 1 Percentage Change in Price

![Plot7](https://user-images.githubusercontent.com/46770398/116353101-33d5a480-a7c4-11eb-8060-e63db192d87f.png)

Plot 7: DBSCAN plot between Initial Price Offering and Day 3 Percentage Change in Price

![Plot8](https://user-images.githubusercontent.com/46770398/116353108-37692b80-a7c4-11eb-8bff-9230918b3a2e.png)

Plot 8: DBSCAN plot between Initial Price Offering and Day 4 Percentage Change in Price

![Plot9](https://user-images.githubusercontent.com/46770398/116353116-3b954900-a7c4-11eb-8060-fe260a5a3e08.png)

Plot 9: DBSCAN plot of  Industry Number, Revenue Growth, and Day 4 Percentage Change in Price

![Plot10](https://user-images.githubusercontent.com/46770398/116353123-4059fd00-a7c4-11eb-9e50-4a002e9f9f1a.png)

Plot 10: DBSCAN plot of  Industry Number, Revenue Growth, and Day 1 Percentage Change in Price

![Plot11](https://user-images.githubusercontent.com/46770398/116353131-42bc5700-a7c4-11eb-827f-24a5cd6cc96b.png)

Plot 11: DBSCAN plot of  Industry Number, Revenue Growth, and Day 2 Percentage Change in Price

![Plot12](https://user-images.githubusercontent.com/46770398/116353140-45b74780-a7c4-11eb-82e4-9a5e0de873af.png)

Plot 12: DBSCAN plot of  Industry Number, Revenue Growth, and Day 3 Percentage Change in Price

### Neural Network:
We separated the data into three different labels, which are increase, maintain, and decrease based upon the growth percentageprice after 5 days - initial priceinitial price. Our current thresholds are -5% and +5%. The data was split into 70% for training and 30% for testing, just based on the default recommendations provided by the libraries used here (sklearn in this instance). There are lots of different things that could be considered hyperparameters here, including the number of hidden layers and the number of neurons per hidden layer. After initial testing we determined that changing the number of hidden layers and the number of neurons per layer had a negligible effect on the accuracy of the model, so those variations were excluded from the charts.

![Figure7](https://user-images.githubusercontent.com/46770398/116353174-4fd94600-a7c4-11eb-86cd-572b02b23613.png)

As you can see in the figure above, the main parameters we chose to change in this case were the activation function, the solver for optimization of weights, and the maximum number of iterations. The variations using lbfgs and adam are listed as only using versions with 5000 as the max iteration because any values below that caused the optimizer not to converge within the given iteration number. To eliminate this issue, we simply chose to include only the variations that consistently output correctly. Changing the activation function and the max number of iterations had little to affect on the accuracy, with the largest changes coming from changes to the solver for optimization of weights. SVD (Stochastic Gradient Descent) was easily the best and there was a noticeable drop in accuracy when switching to adam and lbfgs, with the worst of the two being lbfgs (despite being touted in documentation as better for smaller datasets). Upon seeing the accuracy given here we were initially happy with the results until upon further inspection, we realized that the model was heavily predicting that companies would increase in stock price and heavily underpredicting when they would maintain or decrease in stock price. This is reflected in the confusion matrix shown below:

![Plot13](https://user-images.githubusercontent.com/46770398/116353198-5798ea80-a7c4-11eb-9a09-4850612ff2f3.png)
   
### Linear Regression
We performed 500 iterations of the Linear Regression algorithm to model the relationship between our chosen 13 variables and the fifth-day difference of each stock’s price. Each of the 13 independent variables was weighted equally. This equal weighting with a plurality of variables coupled poorly with the sklearn functions used, producing the peculiar result of an extremely negative Average Coefficient of Determination: -24.269597426340304, with individual values ranging from as little as -.003 to as large as -295. Even when removing the outliers (n < -5), the average score dropped to -.9. 
The graph below shows each fifth day difference in relation to its initial price. Due to using a single independent variable, a clearer picture can be seen: Here, a more normal, though no more encouraging Coefficient of Determination was had: .01. 

![Plot14](https://user-images.githubusercontent.com/46770398/116353255-6e3f4180-a7c4-11eb-8ffe-3087d465b9ab.png)

These stark differences just serve to demonstrate what we believe to be a method which was not as appropriate for our purposes as others. When factoring in multiple independent variables, unforeseen quirks clouded our judgement. But when focusing in to find a clearer picture, there were too many important details missing for the algorithm to give a potential user confidence in its suggestions.


### Random Forest
The data is separated into three different labels, which are increase, maintain, and decrease.  They are separated based on the growth percent.price after 5 days - initial priceinitial priceThe thresholds are -5% and 5%. The data is split into 80% for training and 20% for testing. In order to find out the parameters for the best maximum depth of the trees and the number of trees in the forest, cross validation is used.

![Plot15](https://user-images.githubusercontent.com/46770398/116353347-96c73b80-a7c4-11eb-9f10-8034b73154bf.png)

![Plot16](https://user-images.githubusercontent.com/46770398/116353355-9b8bef80-a7c4-11eb-8bed-d6263c399fc5.png)

According to the graph, the maximum depth of the trees is chosen to be 3, which would increase the accuracy of the model as well as prevent potential overfitting. The number of trees is chosen to be 165, as the model is more stable and provides a relatively high accuracy for the prediction.
As a result, the model using these parameters achieves an accuracy of 0.6451 on the training data set, and 0.6098 on testing data set. This is not a good prediction model because it makes a lot of false increase label predictions, and fails to predict the decrease and maintain labels. The 0.6098 accuracy is due to the high ratio of increase labels in the data set. If the prediction gives an increased label, there are relatively high chances that it is correct. Also the importances of different features are close to each other, which adds difficulty to use random forest methods and increases the errors.
A larger and more balanced data set could potentially provide a more accurate model using random forest.

![Plot17](https://user-images.githubusercontent.com/46770398/116353806-659b3b00-a7c5-11eb-9d9f-f3ddd7e02770.png)

![Plot18](https://user-images.githubusercontent.com/46770398/116353405-b3fc0a00-a7c4-11eb-97c8-7113bf73100a.png)


### SVM
The data are labeled the same way in the Random Forest. The ratio of training to testing is also 4:1. Different kernels are used to find the best SVM model to make the prediction. For all SVM models, the regularization parameter C is chosen to be 1. There is only a minor change, which is neglectable, in accuracy for larger C, and  to decrease the calculation time, C=1 is selected. For the polynomial kernel, cross validation is used to find the best degree.

![Plot19](https://user-images.githubusercontent.com/46770398/116353459-cb3af780-a7c4-11eb-986a-efcd94babf68.png)

As the graph shows, 6 is the best degree which would provide a relatively high accuracy in prediction. The model using the polynomial kernel with a degree of 6 achieves an accuracy of 0.6451 on the training data set, and 0.6220 on the test data set. The models using the rbf kernel and sigmoid kernel achieve similar accuracy.

![Plot20](https://user-images.githubusercontent.com/46770398/116353825-6f24a300-a7c5-11eb-83cc-75227b208484.png)

![Plot21](https://user-images.githubusercontent.com/46770398/116353835-721f9380-a7c5-11eb-9466-63ebd32b1349.png)

In general, the models generated from svm provide similar accuracy to the model generated by random forest. The results show the model provides a large amount  of false increase labels in prediction and fails to predict decrease and maintain labels.
The reason for the inaccuracy is due to the bias in the origin data set and the data set itself is respectively small. A more balanced and large data set could be used to generate a more accurate model by SVM.


# Results

### DBSCAN
The model was inconclusive for the clustering between the fields of  industry number, percentage change in price, initial price offering and revenue growth. Only 1 cluster presented in Scatter 2D and had 406 outliers out of 449 , so our data collected or fields could not determine relationships among the fields.

### Linear Regression
The average score was -24.269597426340304, ranging from  -0.003318710070917996 to -295.1381520711185. When factoring in extreme outliers (n < -5), the average score dropped to -0.9412391437642692.

### Neural Network
The best model from NN has an accuracy of 0.618812, using a Relu activation function, SGD as the solver, and 1000 maximum iterations. The model mainly makes false increase label prediction, with many missed maintain/decrease labels.

### Random Forest
The best model from random forest has an accuracy of 0.6451 on the training data set, and 0.6098 on the test data set. The model mainly makes false increase label prediction.

### SVM
The best model from SVM has an accuracy of 0.6451 on the training data set, and 0.6220 on the test data set. The model mainly makes false increase label prediction.

# Discussion
After consulting while working on all of our methods, we immediately noticed a trend that seemed to persist amongst them (notably within the classification methods) in that we were having a similar issue where the model incorrectly classifies a datapoint as “increases” at a much larger rate than anything else. This largely stems from issues with our dataset itself, given that over 60% of the dataset is classified as an “increases” stock. This skew in the dataset and not having enough variation definitely shows in our data resulting from our methods.GIven these issues with the dataset, we aren’t surprised by the fact that our results came out as they did.

# Conclusions
We believe that our main issues for this project lied within the data, not within the models. We are unhappy with the results of the models due to this skew in the data, despite the fact that our classification models were returning +60% accuracy across the board as we don’t feel these results will be able to help companies with their decision making as effectively as we would have hoped. Overall a larger dataset with more varied data would have helped, but this is a valuable lesson to have learned firsthand with real-world data.

# References

[1] M. Lowry, M. S. Officer, and G. W. Schwert, “The Variability of IPO Initial Returns,” http://schwert.ssb.rochester.edu/, Apr-2010. [Online]. Available: http://schwert.ssb.rochester.edu/jofi_1540.pdf. [Accessed: Mar-2021].

[2] Rudden, J. (2021, January 28). Number of IPOs in the U.S. 1999-2020. Statista. https://www.statista.com/statistics/270290/number-of-ipos-in-the-us-since-1999/#:~:text=In%202020%2C%20there%20were%20407,as%20in%20the%20previous%20year. 

[3] All 2021 IPOs (so far). Stock Analysis. (2021, April 27). https://stockanalysis.com/ipos/2021-list/#:~:text=There%20have%20been%20442%20IPOs,44%20IPOs%20by%20this%20date. 


