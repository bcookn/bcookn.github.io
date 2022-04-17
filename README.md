## Welcome to my GitHub!
### <ins>Evaluating GHG Emissions by Neighborhood Wealth</ins>

#### Overview
*"The 1 percent could emit 30 times more than the poorest 50 percent and 175 times more than the poorest 10 percent"*
<br> 
> -- Oxford Media Briefing, 2015

Do high income neighborhoods emit more GHG emissions than low income neighborhoods? If so, by how much? This is a short project that evaluates GHG emissions on a neighborhood-level in California as part of my capstone for my masters program. I am primarily interested in the differences in emissions between high income neighborhoods and low income neighborhoods. To do this, I will be utilizing subclassification and matching via k-nearest neighbors to control for the differences in means and estimate average treatment effects. 

#### Data
Multiple data sources were used and combined into one large dataframe for this analysis. The four datasets sourced are from the following: UC Berkeley’s Cool Climate Program, Google Earth Engine, U.S. Census, and the California Government. Berkeley’s Cool Climate Program dataset makes up most of the data, containing observations related to GHG emissions, CO2 emissions, etc. on a zip code level. Satellite data was pulled using Google Earth Engine which contains temperatures by zip code, and both the U.S. Census and California Government data were used to get income and urban/rural classification data, respectively. One of the biggest challenges was cleaning and combining the datasets in a usable format, without sacrificing a good chunk of observations. Te end result had a final observation size of 1594, with 956 observations that are categorized as “low income” and 638 observations that are categorized as “high income”.

#### Methods
The primary methods used in this analysis were subclassification and matching. To control for differences in income, there needed to be some type of grouping to compare GHG emissions across different neighborhoods with the same income distribution. To do this, neighborhoods were placed into buckets relative to the average household income and average number of people per household. Once the dataset was adjusted for income distribution, it was clear that there was some fairly significant difference in GHG emissions between the two groups. The next step was to try and compare low income neighborhoods to high income neighborhoods; however, the problem is that not all high/low income neighborhoods are similar. For example, you cannot compare East Oakland to Compton, or Atherton to Beverley Hills. This is due to are differences in climate/temperature, population density, car usage-- all things that could possibly impact overall GHG emissions. To solve for this, a method called matching was used. What this does is that it matches each high income neighborhood with a similar low income neighborhood. Essentially, it's similar to finding high/low income neighborhood twins. With matching, comparisons between low and high income neighborhoods become comparable. To determine a matching estimator, we leaned on k-nearest neighbors to do the heavy lifting. All that was needed was to define measurements of proximity to establish baselines on how similar observations were to each other. To correct for confounding variables, the features were standardized and scaled. The final piece is that we used a combination of OLS and the CausalModel function from the `causalinference` package. These two methods gave us the true average treatment effect of low income neighborhoods on GHG emissions.

#### Results
And now it's time for the real bread and butter of this project-- the results! During the beginning stages of this project, a simple scatter plot was made to determine if a relationship even existed between GHG emissions and income.
<br>

![image](https://user-images.githubusercontent.com/65251932/163735451-323453f4-4e76-41c4-812f-c24fb0fc067d.png)

Lo and behold! A positive relationship does in fact seem to exist. However as seen in the visualization, there is a lot of bunching and blending in the middle which gives us a somewhat mixed message. This tends to happen with income data, as folks who are in the middle tend to have incentives to lie about income due to government incentives or programs. To get a better look at this relationship, an SLR (simple linear regression) model was ran with GHG emissions as the dependent variable and income as the independent variable.
<br>

![image](https://user-images.githubusercontent.com/65251932/163735448-83b9b5d5-96c5-4526-825c-fdb843777708.png)

These initial results were fairly promising, as income seemed to explain 57% of the variability in the data. It's saying that for every $1 increase in average neighborhood household income, CO2e increases by 0.0003 metric tons. To put this into perspective, 0.0003 tCO2e is equivalent to GHG emissions from 0.75 miles driven in an ICE (internal combustion engine) vehicle. 

Another OLS regression was ran, this time with interaction variables to evaluate the effect of income on GHG emissions while keeping population density in mind. GHG emissions is the dependent variable, with income and population density as the independent variables and as the interaction variable. 

![image](https://user-images.githubusercontent.com/65251932/163735432-bcbf8688-d9f0-4534-8d7c-150a95eb40c7.png)

While the p-value is fairly insignificant (as well as the confidence interval, i.e. it crosses 0) the negative coefficient in the interaction term suggests that income is less strongly related to emission in areas where population density is larger. There could be many factors contributing to this, such as miles traveled in cars. From a qualitative perspective, this would make sense. People who live in Fresno or Tracy (more rural areas) might have to drive more than people who live in San Francisco, where there is a halfway-decent public transportation system that is established. Though our data is focused on California, let's also take a look at New York. New York City's population size is very different from Buffalo. The population in Buffalo is about 250,000, while New York City's population is roughly twice the amount. New York City has a well-developed public transportation system in the five boroughs, and thus most New Yorkers do not have a car (and if they do, it's rarely used). Living in Buffalo on the otherhand, would require a car. So it is likely that the more metropolitan an area is, the more likely a public transportation system exists-- which in turn, lessens overall GHG emissions.
