## Welcome to my GitHub!
### <ins>Evaluating GHG Emissions by Neighborhood Wealth</ins>

#### Overview
*"The 1 percent could emit 30 times more than the poorest 50 percent and 175 times more than the poorest 10 percent"*
<br> 
> -- Oxford Media Briefing, 2015

Do high income neighborhoods emit more GHG emissions than low income neighborhoods? If so, by how much? This is a short project that evaluates GHG emissions on a neighborhood-level as part of my capstone for my masters program. I am primarily interested in the differences in emissions between high income neighborhoods and low income neighborhoods. To do this, I will be utilizing subclassification and matching via k-nearest neighbors to control for the differences in means and estimate average treatment effects. 

#### Data
Multiple data sources were used and combined into one large dataframe for this analysis. The four datasets sourced are from the following: UC Berkeley’s Cool Climate Program, Google Earth Engine, U.S. Census, and the California Government. Berkeley’s Cool Climate Program dataset makes up most of the data, containing observations related to GHG emissions, CO2 emissions, etc. on a zip code level. Satellite data was pulled using Google Earth Engine which contains temperatures by zip code, and both the U.S. Census and California Government data were used to get income and urban/rural classification data, respectively. One of the biggest challenges was cleaning and combining the datasets in a usable format, without sacrificing a good chunk of observations. Te end result had a final observation size of 1594, with 956 observations that are categorized as “low income” and 638 observations that are categorized as “high income”.

#### Methods
The primary methods used in this analysis were subclassification and matching. To control for differences in income, there needed to be some type of grouping to compare GHG emissions across different neighborhoods with the same income distribution. To do this, neighborhoods were placed into buckets relative to the average household income and average number of people per household. Once the dataset was adjusted for income distribution, it was clear that there was some fairly significant difference in GHG emissions between the two groups. The next step was to try and compare low income neighborhoods to high income neighborhoods; however, the problem is that not all high/low income neighborhoods are similar. For example, you cannot compare East Oakland to Compton, or Atherton to Beverley Hills. This is due to are differences in climate/temperature, population density, car usage-- all things that could possibly impact overall GHG emissions. To solve for this, a method called matching was used. What this does is that it matches each high income neighborhood with a similar low income neighborhood. Essentially, it's similar to finding high/low income neighborhood twins. With matching, comparisons between low and high income neighborhoods become comparable. To determine a matching estimator, we leaned on k-nearest neighbors to do the heavy lifting. All that was needed was to define measurements of proximity to establish baselines on how similar observations were to each other. To correct for confounding variables, the features were standardized and scaled. The final piece is that we used a combination of OLS and the CausalModel function from the `causalinference` package. These two methods gave us the true average treatment effect of low income neighborhoods on GHG emissions.


Test photo:

![image](https://user-images.githubusercontent.com/65251932/163538842-54a0420b-032b-4e16-9ce2-7d5541793e37.png)
