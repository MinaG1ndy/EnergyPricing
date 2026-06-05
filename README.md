# EnergyPricing
Energy Pricing Analysis  
Data Source: Transparency platform entsoe.eu  
Market, Energy Prices  
Region: BZN|DE-LU (Germany)  
Duration: 1/1/2023 to 31/12/2023 (1 Year)  
The dataset was downloaded in CSV format with 6 columns and 70081 rows (including headings)
The goal is to build a forecasting model. But before doing that, I need to understand this price series. Doing a statistical summary and telling what's unusual about it.  

I will Divide this exploration into 5 steps as follows:  
1- Basic summary statistics (Clean, Mean, Median, std, skewness, percentiles)  
2- Distribution Shape (Histogram, BoxPlot)  
3- Segment and Compare (Split peak and off-peak hours)  
4- Identify anomalies (Variance at extreme events)  
5- Analysis Summary  

Data Sample:  
<img width="1582" height="151" alt="image" src="https://github.com/user-attachments/assets/a2bf6eeb-196d-4ceb-8c58-9369f48016a3" />  
**MTU (CET/CEST):** Central European Time / Central European Summer Time, in format of duration window of the observations as "dd/mm/yyyy hh:mm:ss - dd/mm/yyyy hh:mm:ss"  
**Area:** Represents Germany (BZN|DE-LU)  
**Sequence:** Sequence 1 represents prices that are set for each full hour as one block price applied to all four quarter-hours. This is the main day-ahead market most people refer to.
While Sequence 2 reflects a more precise supply/demand balance at that specific quarter-hour, Can diverge significantly from the hourly price because of factors like demand revised, different participants bidding...etc  
**Day-ahead Price (EUR/MWh):** The price we would focus on, It can be negative or positive for **noe!
Intraday Period (CET/CEST)**  
**Intraday Price (EUR/MWh)**

Questions to Answer at the end of the Analysis: 
1- Why is the mean higher/lower than the median in electricity prices?  
2- What does high kurtosis tell you about this market?  
3- What do the 5th and 95th percentiles tell you that min/max don't?  
4- Is electricity price normally distributed? Why does this matter for risk modeling?  
5- Is this is normal for normal distribution peak to be lower than the dataset max?  
6- Why do negative prices exist in electricity markets? Why do spikes exist? What does this mean for a trader trying to hedge?  

Basic summary statistics results:
The mean is: 95.1755  
The median is: 98.0200  
The variance is: 2263.9990  
The standard deviation is: 47.5815  
The minimum value is: -500.0000  
The maximum value is: 524.2700  

--- Percentiles ---
 5th Percentile: 1.0695  
25th Percentile (Q1): 75.8775  
75th Percentile (Q3): 122.1200  
95th Percentile: 166.2525  

--- Shape Metrics ---
The skewness is: -0.4908  
The excess kurtosis is: 6.3060  

Distribution Shape results:
<img width="3520" height="1634" alt="anaconda_projects_ded83cfd-535e-4296-9093-a2531b9e9461_my_plot" src="https://github.com/user-attachments/assets/5ffc9e44-9113-4128-9b5a-21b6a21087e1" />


Segment and Compare results:
I had split the dataset into on-peak (8:00 - 20:00) and off-peak demand, also in semesters (Summer, Winter, Others)
<img width="3300" height="1800" alt="anaconda_projects_ded83cfd-535e-4296-9093-a2531b9e9461_energy_price_distribution" src="https://github.com/user-attachments/assets/1584dfc7-f006-4bba-8584-d577465c9372" />
<img width="3000" height="1800" alt="anaconda_projects_ded83cfd-535e-4296-9093-a2531b9e9461_average_energy_prices" src="https://github.com/user-attachments/assets/64f4135d-4080-45fc-be5b-b8c24f28e495" />


Identify anomalies results:
--- Negative Prices ---
Count of readings: 1204
Percentage of total observations: 3.4361%

--- Prices Above 200 EUR/MWh ---
Count of readings: 448
Percentage of total observations: 1.2785%

--- Variance Analysis ---
Total extreme observations: 1652 (4.7146%)
Percent of Total Sum of Squares contributed: 32.51%
Percent reduction in variance if removed: 29.36%

Analysis Summary:
In energy markets, the mean is always higher than the median as the wholesale electricity prices are highly right-skewed (positive).  
<img width="1083" height="523" alt="Right skewed" src="https://github.com/user-attachments/assets/60a9d10a-bc69-4884-b315-b77aa75d1245" />

As the market has to reach the equilibrium of supply and demand, spikes such as plant shutdowns are massive as it pulls the mean, dragging it upward; the median is barely moving because these spikes only represent a tiny fraction of the total year's 15-minute reading.  
In financial markets, pricing can go down to zero.  
In energy market, prices can go to negative due to inflexible generation of oversupply. at this case generators are shutdown manually before further negative negative prices (meaning it has a minimum ceiling). On the other hand, spikes can reach thousands of money (I don't know if there would be ceilings or no :)).  
This creates an asymmetrical profile.  
Surprisingly, our dataset has high negative outliers (-500 EUR/MWh on the left). Those extreme negative prices are rare but they pull the mean downward away from the median.  
In our data: mean = 95.18, median = 98.02.  
The skewness of -0.49 confirms this. Negative skew means the left tail; also, we have three times as many negative price events (1,204) as extreme high-price events (448).  
The potential reasons: 
Conclusion: If you used the mean to price a contract, you would be slightly underpricing it relative to where most prices actually settle. The median is a better description of the "typical" price in this market.
