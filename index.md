## IPO ML Project 

### Team Members

Jun Chen

Joshua Hess

Zhening Xu

Owen Reece

Hunter Germundsen

# Introduction/Background

For day traders and stock brokers, one of the most intriguing investments are companies selling their stock publicly for the first time (IPO, or Initial Public Offering). Because these stocks are new, and the companies relatively unproven, IPO stocks are much more volatile than stocks for established companies.  With the “extraordinarily high variability of initial returns” [1] there is increased opportunity; using machine learning techniques we will try to best capitalize on this volatility by predicting whether an IPO stock will increase or decrease in the first 5 days of trading.

# Problem Definition

 
With the controversy surrounding wall street and the stock market spiking recently, companies have more reason than ever to be fearful of going public. We seek to create a program to predict the change-in-price of an IPO after its first week on the market, to help smaller companies determine if going public is the right move for them. --This will most likely be done using models of the financial details of previous IPOs compared with the current financial details of the IPO we aim to predict.-- 


# Methods

“Neural network models are a popular tool in predicting IPO pricing” [2]. To contrast these, using clustering methods such as hierarchical clustering and density based clustering, we can also determine which input variables such as operating cost, profit/loss, and revenue growth impact the first 5 trading days of the IPO. 
We also want to try to determine the relationship between these input variables to determine the positive or negative percentage of trading prices with linear regression. Based on the relationship between these features, we can try to determine a random forest decision tree for the best outcome which is the percentage of change for the first 5 trading days of the IPO. A similar study was conducted by [3] Basti, Kuzey, and Delen which they examined the short term IPO performance using decision trees and SVM based on the variables of market sentiment, the annual sales amounts, the total assets turnover rates, IPO stocks sales methods, the underwriting methods, the offer prices, debt ratio, and number of shares sold of Turkish companies.


# Potential Results

The main goal of this project would be to provide a prediction of the outcome of a new potential IPO by analyzing the data and outcomes of previous IPOs. It’s not uncommon to see an IPO where the opening stock price is drastically different from the stock price after 5 days, due to a variety of factors (internal to the company itself or from external sentiment). This prediction could provide key insight to users and allow a higher level of certainty for potential investors, minimizing their losses and maximizing their gains.


# Discussion

Through the unsupervised learning of the past IPO data set, we could conclude certain patterns in change-in-price of IPOs which share similar features after its first week on the market. This would help the public understand the expectations of companies in different fields. The supervised model would provide what range of change-in-price of IPOs could be expected after a week by the given data of the IPO based on the past IPO data. The Model could help the public to make investment decisions and assist the companies to make more reasonable decisions regarding IPO.




# References

[1] M. Lowry, M. S. Officer, and G. W. Schwert, “The Variability of IPO Initial Returns,” http://schwert.ssb.rochester.edu/, Apr-2010. [Online]. Available: http://schwert.ssb.rochester.edu/jofi_1540.pdf. [Accessed: Mar-2021].

[2] T. H. Ly and K. Nguyen, “Do Words Matter: Predicting IPO Performance from Prospectus Sentiment,” IEEEXplore, 2020. [Online]. Available: https://ieeexplore.ieee.org/document/9031486/. [Accessed: 03-Mar-2021].

[3] E. Bastı, C. Kuzey, and D. Delen, “Analyzing initial public offerings' short-term performance using decision trees and SVMs,” Decision Support Systems, 21-Feb-2015. [Online]. Available: https://www.sciencedirect.com/science/article/pii/S0167923615000317. [Accessed: 04-Mar-2021]. 

