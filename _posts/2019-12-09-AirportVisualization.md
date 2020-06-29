---
layout:     post
title:      [Project] Austin-Bergstorm International Expansion Plan
subtitle:   Using visualizations developing business strategies
date:       2019-12-09
author:     Aly Hu
header-img: img/ausairport.jpg
catalog: true
tags:
    - Project
    - Data Visualization
    - Tableau
    
---

**This is a team project. Co-authors are [Jiali Yin](https://www.linkedin.com/in/jiali-yin/), [Xinyi Zhu](https://www.linkedin.com/in/xinyi-zhu/), [Linsay Trinh](https://www.linkedin.com/in/lindsay-trinh/), [Shangyun Song](https://www.linkedin.com/in/shangyun-song/) and [Yasi Chen](https://www.linkedin.com/in/yasi-chen-214a8418a/). The original tbwx file exceeds the size limitation on Tableau Public, so I used gif images to introduce the interactive functions for each dashboards. The original tbwx file can be found on my [github](https://github.com/AlyyHu/Course-Project/tree/master/Tableau_AUSAirport).** 

## Summary
Austin city is one of the fastest growing cities in the nation, both from economic and demographic perspective. To meet the increasing demand in the airport caused by the rapidly increasing population, AUS introduced a 40-Year Master Expansion Plan around 2000. We analyzed the data of flights departing from or arriving at Austin airport using **Tableau**. Our goal is to give recommendation on how to improve the capacity and efficiency of the airport and develope the AUS 40-years expansion plan. 

The original data was provided by Fuqua Business School. The data contains all the airport's flights information in the United States from November 2015 to June 2018. We analyzed the flight information by answering the following questions:

* **Why does AUS need to be expanded?**  
  We incorporated U.S. GDP regional growth rate and population information in order to answer the question. 
* **Can AUS Meet the capacity in 2019?**  
  We analyzed the trend of total number of flights departing from or arriving in AUS from November 2015 to June 2018, and we forecasted the estimated number of flights for AUS airport from July 2018 to September 2019. Based on the forecasts, we analyzed whether the 40-Year Expansion Plan could enable AUS airport to meet the capacity in 2019.
  
* **How to allocate gates based on Airlines?**  
  There are 12 major airlines cooporating with AUS airport. We analyzed the number of flights and average taxi-in time for different airlines.
  
* **How does each airline perform on each route?**
  We projected all the flight paths on US maps, and analyzed the total number of flights on the route, the average departure/arrival delay on the route, and compared the average time of delay for different carriers on the same route.
  
* **Do carriers have different causes of departure delays? How can we mitigate them?**  
  Departure delay can be devided into five categories: Carrier Delay, Weather Delay, Late Aircraft Delay, Security Delay and NAS Delay. We analyzed the composition of departure delays for different carriers to see which part can be improved in order to decrease the total time of delay.

* **How is the current cancellation performance?**  
  We analyzed the trend of cancellation rate in each month from 2016 to 2018.

By visualizing these questions, we came up with four recommendations for AUS to implement its expansion plans.

## Background
### Population Growth
With more and more high-tech companies establishing in the Austin metropolitan area, Austin city becomes one of the fastest developing city in the nation. According to the GDP and population information provided by U.S. Bureau of Economic Analysis, Austin has the highest GDP growth as well as the highest population growth amongst all major cities in the United States. As a result, an expansion is necessary for Austin International Airport in order to meet busy traffics. 

 ![Population Growth](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8iig2wp9j31re0u0q3h.jpg)
 
### Capacity Forecasting
Around 2000, AUS airport introduced a Master 40-Year Plan to build 32 more gates by the end of 2040. 

 ![Expansion Plan](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8j3hxf6oj31nq0sitaf.jpg)

We forecasted the number of flights departing from or arriving in AUS airport based on the information from 2015 to 2017. The orange line is the actual number of flights in each month from 2015 to 2017. The yellow line is the forcast monthly information based on the trend in previous years. The light yellow rectangle behind the yellow line is the confidential interval, which means the actual figure is highly likely to fall in this range. 

 ![Capacity Forecasting](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8j3i9rm3j31qc0u00ta.jpg)
 
In November 2019, the estimated number of flights is 14,392, almost 1.5 times of the number of flights in November 2018. Currently AUS airport has 26 gates. In order to satisfy the heavier traffics, AUS airport will need to add at least 13 more gates. However, according to the expansion plan, AUS plans to build 10 more gates by the end of 2020, which may not be sufficient if no other action is taken. As a result, we tried to develope a solution for AUS airport to improve overall operating efficiency in order to meet the capacity without adding more gates. 

 
## Solutions

### Gate Allocation

Based on this expansion plan, we target to allocate these additional gates to carriers efficiently. The allocation of gates should be based on two components, taxi-in periods and change in number of flights. In order to improve the efficiency, we can allocate more gates to the carriers with longer taxi-in period. 

Here we visualized the monthly average taxi-in period and number of flights for each carriers.

![gate allocation](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8jxk2279j320q0ssjsq.jpg)

The chart shows the information for 11 major carriers cooporating with AUS airport. By clicking on the carriers' icons, the dashboard will show the detailed infromation for the selected carrier. The picture below shows the detailed information for Delta Airline.

![gate - Delta](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8zo5pe7og31190g0agk.gif)


Considering there are several airlines having increasing number of flights arriving at AUS airport over time, we would suggest AUS to allocate more gates to the airlines which shows increasing trend in the number of flights.

### Different Routes’ Delay Performance 

There are several carriers serving on the same flight path. AUS should colaborate more with the carriers with lower time of delay in order to increase its overall efficiency. Flight delays can be divided into two categories: departure delay and arrival delay. As a result, we projected all flights departing from or arriving at the Austin Airport on two maps.

![routes](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8xiurqroj31ba0u0tdn.jpg)

This dashboard can be used as a tool for both AUS airport and passenges. By hovering over the flight routes, you can see the detailed information about the flight path you selected. The information includes the average departure delay for all carriers, total number of flights and ranking of the average departure delay for this specific route. Other than hovering over the flight routes, you can also look for a specific destination or origin by selecting the city in the selection box. Also, you can also look at a carrier's information on a specific route by selecting carrier name in the carrier selection box.

![routes2](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8ys86uv5g310j0kntkg.gif)

Based on our analysis, we found Jetblue Airways and Southwest Airlines have the highest departure and arrival delays for several airlines. We would recommend the airport to collaborate more closely with the airlines with lower average delays.


### Different Carriers’ Departure Delays 
Departure Delays could be divided into five sub-categories: NAS Delay, Weather Delay, Security Delay, Late Aircrafts Delay, and Carrier Delay. This dashboard analyzed the composition of total departure delays for different carriers.

![dep](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg900rkp45j31ph0u0gnd.jpg)

By clicking on the delay information for a specific carrier, the pie chart will show the departure delay composition of the selected carrier, and the chart below will be highlighted, showing the time trends of average time of delay per flight and the percentage of delayed flights.

![dep2](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg900towa7g31190gcahb.gif)

The main drivers, based on our analysis, are NAS Delay, Late Aircrafts Delay, and Carrier Delay.  Additionally, based on another analysis of the percentage of delayed flights among all trips, we infer than Jetblue, Frontier, ExpressJet, and Southwest are four airlines that encounter most departure delay issues.


### Cancellation Rate for Departures and Arrivals:
Cancellation rate can be analyzed from two perspectives: departures and arrivals. The dashboard provides information of the annual changes of cancellation rate and more detailed analysis of both cancellation rate and weather delays regarding a specific month. 

![can2](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg90cqp4g0j31oe0u0jt5.jpg)

You can look at annual, monthly and daily information by selecting year, clicking on month and date. By changing the parameter in the Top n Box and selecting departure type in the selection box, the dashboard will show the top n airports with highest departure or arrival delays.

![can](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg90b0ki46g31190gcgr3.gif)

Austin airport may also want to further investigate the airports and departure time periods with the highest cancellation rate whether weather delay has a significant impact on the cancellation or there are other causes.


## Recommendations
Based on the visualizations, we would recommend AUS airport to take following actions:

* Allocating more gates to carrier with longer taxi-in time and increasing number of flights.
* Airports should collaborate more closely with carriers having lower average delay.
* NAS delay, Late Aircraft Delay and Carrier Delay are the three most serious delays. Austin airport needs government intervention to regulate. The maintenance team should be allocated more efficiently.
* Austin airport needs further investigation on the relationship between the weather delay and cancellation rate.
