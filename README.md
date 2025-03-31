**Authors**
- Chinmay Bharambe
- Praveen Sharma

**Introduction**

In this project, we will be exploring a dataset that recorded major power outages in the U.S - defined by the U.S. Department of Energy as outages that either affected a minimum of 50,000 customers or resulted in an unplanned energy demand loss of at least 300 MegaWatts - from January 2000 to July 2016. The data was sourced from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure (available at https://engineering.purdue.edu/LASCI/research-data/outages). The original dataset comprises 1,534 observations across, where each entry corresponds to an outage, and 55 variables.

In addition to information on outages, the dataset provides extensive information on the affected states’ geographic, climatic, land-use, energy consumption, and economic profiles.

We will explore the following question: What is the relationship between the different classifications of regions, namely states and climate region, and the average outage duration?

We plan to explore how these classifications correlate with the length of power outages by developing models to analyse the data. This exploration is significant because understanding whether certain states or climate regions are more prone to prolonged outages can help energy providers and policymakers design targeted strategies. For instance, if a specific climate region consistently experiences longer outage durations, it could signal the need for improved grid resilience or more efficient emergency protocols, ultimately leading to more effective and proactive management of power infrastructure.

| Column | Description |
|--------|-------------|
| 'YEAR' | Year an outage occurred |
| 'MONTH' | Month an outage occurred |
| 'U.S._STATE' | State the outage occurred in |
| 'OUTAGE.DURATION' | Duration of outage events (in minutes) |
| 'CLIMATE.REGION' | U.S. Climate regions as specified by National Centers for Environmental Information (9 Regions) |
| 'CAUSE.CATEGORY' | Categories of all the events causing the major power outages |
| 'HURRICANE.NAMES' | If the outage is due to a hurricane, then the hurricane name is given by this variable |
| 'CUSTOMERS.AFFECTED' | Number of customers affected by the power outage event |
| 'POPULATION' | Population in the U.S. state in a year |
| 'PCT_LAND' | Percentage of land area in the U.S. state as compared to the overall land area in the continental U.S. (in %) |
| 'OUTAGE.START' | Date and time when the outage event started |
| 'OUTAGE.RESTORATION' | Date and time when power was restored to all the customers |


## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

Before we begin our analysis, we need to clean our dataset. To do so, we perofrmed the following steps:

1. We combined `OUTAGE.START.TIME` and `OUTAGE.START.DATE` into a single timestamp column called `OUTAGE.START`, and combined `OUTAGE.RESTORATION.TIME` and `OUTAGE.RESTORATION.DATE` into a single timestamp column called `OUTAGE.RESTORATION`.

2. Next, we limited our dataset to columns that were relevant for our analysis. These are: `YEAR`, `MONTH`, `U.S._STATE`, `OUTAGE.DURATION`, `CLIMATE.REGION`, `CAUSE.CATEGORY`, `HURRICANE.NAMES`, `CUSTOMERS.AFFECTED`, `POPULATION`, `PCT_LAND`, `OUTAGE.START`, and `OUTAGE.RESTORATION`.

3. All the quantitative columns of the dataset were of the type object. While this did not affect any of the operations carried out with these columns, for consistency's sake we decided to make all qunatitative columns of type float. These columns are: `YEAR`, `MONTH`, `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `POPULATION`.

4. Lastly, We only need to know if an outage is affected by a hurricane or not - we do not need to know the name of the hurricane for the purposes of our exploration. Hence, we decided to create a new column `is_hurricane` that contains *True* for all corresponding values in `HURRICANE.NAMES` that were not missing, and *False* for all corresponding vlaues that were missing (since this indicated that the outage was not affected by a hurricane).

The first few rows of the cleaned dataframe are shown below:
| OBS | YEAR | MONTH | U.S._STATE | OUTAGE.DURATION | CLIMATE.REGION | CAUSE.CATEGORY | CUSTOMERS.AFFECTED | POPULATION | PCT_LAND | OUTAGE.START | OUTAGE.RESTORATION | is_hurricane |
|-----|------|-------|------------|-----------------|----------------|----------------|---------------------|------------|----------|--------------|---------------------|--------------|
| 1 | 2011 | 7 | Minnesota | 3060.0 | East North Central | severe weather | 70000.0 | 5.35e+06 | 91.59 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 | False |
| 2 | 2014 | 5 | Minnesota | 1.0 | East North Central | intentional attack | NaN | 5.46e+06 | 91.59 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 | False |
| 3 | 2010 | 10 | Minnesota | 3000.0 | East North Central | severe weather | 70000.0 | 5.31e+06 | 91.59 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 | False |
| 4 | 2012 | 6 | Minnesota | 2550.0 | East North Central | severe weather | 68200.0 | 5.38e+06 | 91.59 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 | False |
| 5 | 2015 | 7 | Minnesota | 1740.0 | East North Central | severe weather | 250000.0 | 5.49e+06 | 91.59 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 | False |

### Univariate Analysis

#### State Choropleths
First, we wanted to explore the average power duration by state to get a sense of how the power outage duration is distributed across the country.
<iframe
  src="assets/choropleth_map-duration-outage-by-state.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Then we looked at a choropleth map for the number of power outages by state.
<iframe
  src="assets/choropleth_map_2-num-outage-by-state.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Then we looked at the average number of customers affected by state.
<iframe
  src="assets/choropleth_map_3-customers-outage-by-state.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


#### Climate Region Choropleths
Similarly, we wanted to explore the average power duration by climate region to get a sense of how the power outage duration is distributed across these regions.
<iframe
  src="assets/choropleth_climate-duration-outage-by-cregion.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Then we looked at a choropleth map for the number of power outages by climate regions
<iframe
  src="assets/choropleth_climate2-num-outage-by-cregion.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Then we looked at the average number of customers affected by climate regions
<iframe
  src="assets/choropleth_climate3-customer-outage-by-cregion.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


#### Total Population Choropleths by State and Climate Regions
In light of all the interesting patterns and unexpected insights we gained from the previous choropleths, we decided to map a distribution of populations by both states and climate regions to see if it could – to some extent – explain some of these patterns.
<iframe
  src="assets/choropleth_state_pop-pop-by-state.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/choropleth_climate_pop-pop-by-cregion.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Insights from Univariate Analysis
From the 8 choropleths above we see can glean insights – some obvious others not so much. 

##### Average Outage Duration 

1. There's significant variation in outage duration across both states and climate regions
2. States in the upper Midwest (particularly Wisconsin and Michigan) and parts of the Northeast (New York) show longer average outage durations (shown in darker red)
3. By climate region, East North Central i.e the region surrounding the great lakes, show the longest average outage durations. Again this keeps in line with what we saw from the individual states in these regions.
4. The North West and West North Central regions generally have shorter durations (lighter colors). This keeps in line from what we see with the individual states of these regions.

##### Power Outage Count 

1. California stands out with the highest count of power outages at the state level.
2. When grouped by climate region, the Northeast has the highest concentration of outages
3. Many central and mountain states have very few recorded outages

##### Customers Affected 

1. South Carolina, Florida, and Texas show high numbers of affected customers
2. Climate regions with high population density (Southeast, South) show more affected customers
3. Some states with moderate outage counts still have high numbers of affected customers

##### Population Distribution 

1. California and Texas have the highest populations
2. In isolation there is not much else of note however the population choropleths help shape patterns in the other graphs.

##### The Interesting Insights:
1. There exists this frequency-duration paradox where while California has the highest count of outages (dark green in image 2), it has a relatively moderate average duration (light red in image 1). Conversely, Wisconsin and Michigan have fewer outages but much longer durations. From this, we learnt that systems that fail more often may be better at recovering quickly — a counterintuitive finding that challenges an assumption we thought to be obvious – which is that reliability and restoration speed go hand-in-hand.

2. The Northeast climate region shows both high outage counts along with longer durations of outages, but not proportionally high customer impacts. This suggests a pattern which is that outages in some regions are more localised but harder to fix. 

3. Comparing the customer affected to the population choropleths (images 3 and 4) show that some states with low population (like South Carolina) have disproportionately high numbers of affected customers. This could indicate that outages in these states tend to be more critical in nature affecting many people, rather than localized failures.


### Bivariate Analysis

#### Distribution of Outage Duration by Climate Regions

First, we explored the distribution of outage durations by climate region.
<iframe
  src="assets/bivariate1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Then we explore the average outage duration by cause for each climate region
<iframe
  src="assets/bivariate2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Next we explored the relationship between outage duration and customers affected
<iframe
  src="assets/bivariate3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Then, we decided to stratify our data by state and then climate region to see if we derive any new relationships between these variables.
<iframe
  src="assets/bivariate4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/bivariate5.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Next, we plotted outage duration against population.
<iframe
  src="assets/bivariate6.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Since the graph above was convoluted and did not hint at any real relationship between the variables in question, we decided to once again stratify our data by both state and climate region to see if any relationship can be derived.
<iframe
  src="assets/bivariate7.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/bivariate8.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Insights from Bivariate analysis

##### Regional Duration Patterns (Graph 1)
1. The East North Central climate region shows the highest median outage duration (around 3,000 minutes) with a much higher interquartile range than other regions. 
2. The Southwest and West North Central climate regions show the shortest median outage durations (around 70-100 minutes). 
3.  The Northeast and East North Central climate regions both have higher medians, but their distribution shapes differ significantly. East North Central has a wider IQR, indicating more inconsistent outage times, while Northeast's narrower distribution suggests more predictable, though still lengthy, outage times.

##### Outage Cause and Region Interaction (Graph 2)

1. East North Central: Extremely long outage durations for both fuel supply emergencies, approximately 34,000 minutes, and equipment failures – approximately 26,000 minutes.
2. Southwest: Severe weather causes extremely long outages – approximately 11,500 minutes.
3. South: Again, fuel supply emergencies lead to long outages – approximately 17,500 minutes.

##### Outage Duration and Customers Affected (Graphs 3, 4, 5)
1. There is a slightly negative and non-linear correlation between the number of customers affected and outage duration. This was somewhat unexpected as we thought if more people are affected by an outage, it is probably more critical and therefore will take longer to fix. 
2. Graph 4 reveals that states with similar numbers of average affected customers can have dramatically different average outage durations. For example, take the states of Oregon and Wisconsin. The former has an average of approximately 43,000 customers affected while the latter has a marginally higher average of approximately 45,000 customers affected. Despite this small difference, the average outage duration for Oregon is roughly 776 minutes whereas for West Virgina it is roughly 7900 minutes – a difference of more than 10 times. 
3. In Graph 5, the East North Central climate region (in orange) demonstrates the most staggering pattern. It has very long outage durations – roughly 5352 minutes on average – all while having a moderate number of affected customers aproximately 150,000. This is in line with previous visualisations conducted during the univariate analysis. 

##### Outage duration and Population (Graph 6, 7, 8):

1. Somewhat counterintuitively, states with moderate populations (5-15M) show the highest average outage durations, with some exceeding 7,000 minutes such as Wisconsin and Michigan. On the other hand, the most populated states like California, Texas, and Florida actually have shorter average durations than many medium-sized states.
2. States with populations under 3M generally experience shorter outage durations. That being there are some exception to this such as Nebraska, the District of Columbia, and West Virginia.
3. The East North Central climate region stands out again maintaining the longest average duration regardless of its middling population.

##### Interesting Insights
1. When all visualizations are considered together, the strongest insight is that regional factors have a more significant influence on outage duration than simple metrics like the number of people affected or population size. In particular, the East North Central region demonstrates a concerning pattern of lengthy outages across nearly all analyses.
2. This points to your research question's answer: there is indeed a strong relationship between region classification and outage duration, but the relationship is complex and multifaceted, influenced by region-specific vulnerabilities to different outage causes and other unknown factors rather than simply population or number of people affected.


### Interesting Aggregates

First, we will make a pivot table that encapsulates the average outage duration of each cause by their causes. We will also add an overall outage duration average by climate region column to check for instances of Simpson's Paradox.

| CLIMATE.REGION | equipment failure | fuel supply emergency | intentional attack | islanding | public appeal | severe weather | system operability disruption | Overall_Outage_Duration_Avg |
|---------------|-------------------|----------------------|-------------------|-----------|--------------|---------------|---------------------------|---------------------------|
| East North Central | 2643.33 | 3997.25 | 2376.05 | 1.00 | 733.00 | 4434.82 | 2610.00 | 5352.04 |
| Northeast | 215.80 | 14629.57 | 195.98 | 881.00 | 2655.00 | 4429.90 | 773.50 | 2991.66 |
| South | 295.78 | 17482.50 | 325.61 | 493.50 | 1163.98 | 4391.35 | 866.07 | 2846.10 |
| ... | ... | ... | ... | ... | ... | ... | ... | ... |
| Southwest | 113.80 | 76.00 | 265.67 | 2.00 | 2275.00 | 11572.90 | 329.22 | 1566.14 |
| Northwest | 702.00 | 1.00 | 373.81 | 73.33 | 898.00 | 4838.00 | 141.00 | 1284.50 |
| West North Central | 61.00 | 0.00 | 23.50 | 68.20 | 439.50 | 2442.50 | 0.00 | 696.56 |

#### Insights
Looking at the pivot table above, we can see some interesting things. 
    
1. The first and the most interesting pattern that stood out to us was in the equipment failure column. The average outage duration due equipment failure in the East North Central climate region is 26,435 minutes – roughly 18 days! This is over 40 times longer than the next in line for equipment failure which is the Northwest with only 702 minutes on average – roughly 12 hours. 

2. There exist no instances of Simpson's paradox within this pivot table. That being said, the Southeast and West regions very close with the Southeast having a higher average in only 2 of the 7 cause categories – that too by a relatively small margin – but the overall average outage duration of the former is roughly 590 minutes longer than the latter. 



## Assessment of Missingness

Several columns in our dataset contained missing values, and these were handled appropriately based on their missingness dependency.

Since `OUTAGE.START` and `OUTAGE.RESTORATION` are not columns we will be taking into consideration when training our model, we decided to drop these columns altogether.

### NMAR Analysis
The `CUSTOMERS.AFFECTED` column represents the number of customers who experienced power loss during an power outage event. This column in the dataset contains missing values. We believe that these values are Not Missing At Random (NMAR) because the missingness is likely related to what those missing values would have been. In many cases, the exact number of affected customers may not be recorded when the impact is particularly severe or widespread, making it difficult to perform accurate counts. Power companies might struggle to track customer impact during catastrophic events when their monitoring systems themselves are compromised or overwhelmed. Some outages might occur in remote areas with less sophisticated tracking infrastructure, further complicating customer impact assessment. Therefore missingness in `CUSTOMERS.AFFECTED` likely depends on the unobserved values themselves. This makes `CUSTOMERS.AFFECTED` a clear example of NMAR data.

We decided to impute missing values in this column using quantiles.

### MD Analysis
Upon exploring missing values in `CLIMATE.REGION`, we found that the missing values corresponded to either Hawaii or Alaska. Upon external research, we found that these states do not fall under any of the general climate regions for the U.S., implying that these values in the dataset are missing by design (MD). Since our analysis focuses on exploring outage durations across different climate regions, these entries are not relevant and can therefore be dropped.


### MAR Analysis
#### Checking if Missing Values in `OUTAGE.DURATION` are MAR or MCAR
The `OUTAGE.DURATION` Column is critical to our exploration and therefore we will have to address the missing values in this column.

We conducted a permutation test to check the relationship between `OUTAGE.DURATION` and `CAUSE.CATEGORY`, using Total Variation Distance (TVD) as our test statistic since both variables are categorical.

<iframe
  src="assets/missingness1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/missingness2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Our analysis of the missingness in `OUTAGE.DURATION` reveals a significant dependency on `CAUSE.CATEGORY` with a p-value of 0.0057 which is well below the signicance level of 0.05. Therefore we reject the null hypothesis which is that the missing values of `OUTAGE.DURATION` are independent of `CAUSE.CATEGORY`.

The observed Total Variation Distance of 0.22 between the `CAUSE.CATEGORY` distributions when `OUTAGE.DURATION` is missing versus when it is not missing falls outside what would be expected by random chance, as shown in the histogram above. This provides strong evidence that the missingness mechanism for `OUTAGE.DURATION` is MAR rather than MCAR, since the probability of a missing duration value depends on the cause of the power outage. More specifically, certain types of outage causes appear to be associated with a higher likelihood of missing duration data. 

We also conducted a permutation test to check the relationship between `OUTAGE.DURATION` and `POPULATION`, using difference in absolute means as our test statistic.

<iframe
  src="assets/missingness3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


Our analysis of the missingness in `OUTAGE.DURATION` reveals no significant dependency on `POPULATION` values. That is, we fail to reject the null hypothesis which is that the missingness is independent of Population. This is because the p-value we got was 0.3523 which is well above the significance level of 0.05.

The empirical distribution of permuted absolute differences in means shows that our observed absolute difference of 1,434,011.04 falls within the range of what would be expected due to random chance. Approximately 35.23% of the permuted samples produced absolute differences in means as extreme or more extreme than what we observed in the actual data.

This finding suggests that the POPULATION variable does not significantly influence whether `OUTAGE.DURATION` values are missing. In other words, outages in areas with higher populations are not more or less likely to have missing duration data compared to outages in areas with lower populations.

We conducted conditional mean imputation for `OUTAGE.DURATION` because the analysis proved it's MAR, with  the missingness depending significantly on `CAUSE.CATEGORY`. This approach will preserve the important relationship between outage cause and duration.


## Hypothesis Testing
One of the most common themes during our bivariate analysis was that the East North Central stood out in terms out average outage duration irrespective of how the data was stratified. This led us to question if the outage duration distribution of this region comes from the overall distribution of outage duration. To test this the two columns we will be exploring are `OUTAGE.DURATION` and `CLIMATE.REGION` and  will be performing a hypothesis test. 

**Null Hypothesis**: The distribution of outage durations in the East North Central climate region comes from the same underlying distribution as outage durations across all climate regions. That is to say that any observed differences are due to random chance.

**Alternative Hypothesis**: The distribution of outage durations in the East North Central climate region does not come from the same underlying distribution as outage durations across all regions. That is to say that there is a systematic difference.

<iframe
  src="assets/hyp1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


Based on our permutation test, we obtained a p-value of 0, which is sinificantly lower than our significance level of 0.05. This means we reject the null hypothesis.

We can say that the distribution of outage durations in the East North Central climate region does not come from the same underlying distribution as outage durations across all climate regions. The observed differences in outage durations between the East North Central region and other regions are not due to random chance. Instead, these differences represent a statistically significant systematic difference within the two distributions. 

Our analysis showed that the East North Central climate region experiences considerably longer power outages on average (mean duration of approximately 5,411 minutes) compared to the overall average across all regions (approximately 2707 minutes).



## Framing a Prediction Problem
Considering that through parts 2-4 we have been exlporing the impact of various variables on outage duration, we thought it was fitting to try and predict outage duration. At the time of prediction we would know the following variables: YEAR, MONTH, CLIMATE.REGION, CAUSE.CATEGORY, CUSTOMERS.AFFECTED, POPULATION, is_hurricane, PCT_LAND. 

For our baseline model, we will be using a RandomForestRegressor as it is a versatile model that is especially good at dealing with both categorical and numerical data. This makes it a good choice for predicting outage durations. 

Our response  variable here is OUTAGE.DURATION as this is the variable we are trying to predict. 

The metric we are choosing to evaluate our model is Root Mean Squared Error (RMSE). 


Based on our exploration of power outages and their relationship with regional factors, we've identified an important prediction problem:
forecasting the duration of power outages. This is a regression problem since we're predicting a continuous variable (time in minutes).

Our response variable is OUTAGE.DURATION, which represents the length of power outages in minutes. We've chosen this variable because our earlier exploratory analysis showed interesting relationships between outage durations and regional variables, suggesting this is a meaningful prediction to pursue

At the time when a prediction would need to be made, we would have access to the following features:

`YEAR` and `MONTH`: Temporal information about when the outage occurs

`CLIMATE.REGION`: The climate classification of the affected area

`CAUSE.CATEGORY`: The general cause of the outage (e.g., severe weather, equipment failure)

`CUSTOMERS.AFFECTED`: Initial estimate of the number of customers without power

`POPULATION`: Population of the affected area

`is_hurricane`: Binary indicator of whether the outage is hurricane-related

`PCT_LAND`: Percentage of the affected area that is land (versus water)

Evaluation Metric: RMSE

We've selected Root Mean Squared Error (RMSE) as our evaluation metric for the following reasons:

**Interpretability:** RMSE is measured in the same units as our response variable (minutes), making it inherently interpretable. A lower RMSE directly translates to more accurate predictions in terms of minutes.

**Sensitivity to large errors:** RMSE penalizes large prediction errors more heavily than small ones (due to the squaring of errors). This is particularly important in our context because underestimating a long outage by several hours could have serious consequences for emergency responses and similarly overestimating short outages might unnecessarily escalate response efforts

We considered other metrics like Mean Absolute Error (MAE) and R² but determined RMSE to be most appropriate given the high cost of large prediction errors in emergency response planning.


## Baseline Model

For our baseline model, we will implement a Random Forest Regressor, which is well-suited for handling the mix of categorical and numerical features in our dataset while capturing potential non-linear relationships between the features and outage duration.

### Model Evaluation Results
RMSE for training set: 2328.89442745741 minutes
RMSE for testing set: 5153.596924069349 minutes

### Interpreting the results

1. The large gap between training and testing RMSE, more than 2 times, suggests that the model is overfitting to the training data. 
2. These numbers establish a baseline, but clearly indicate there's significant room for improvement. For emergency response planning, predictions that could be off by nearly 4 days would not provide sufficient reliability.
3. The high testing error suggests high variance in your model. The model might be creating deep trees that make very specific predictions based on training patterns that don't generalize.

## Final Model

Based on our analysis of the baseline model's RMSE values, it was clear to us that our Random Forest Regressor was struggling with generalizability and precision. The substantial gap between training and testing performance, 2362.84 vs. 5462.88 minutes, indicates that while Random Forest captures some patterns in the data, it most likely created overly complex trees that memorized the training data rather than learning to generalize relationships.

This performance gap led us to choose a model that can better handle the complex, potentially non-linear relationships while maintaining strong generalization capabilities. This is when we came across gradient boosting regressors which offered a compelling alternative for our final model because they build an ensemble of weak prediction models sequentially, with each new model focusing specifically on correcting the errors of previous models. We also believe that unlike random forest regressor, which build independent trees in parallel, gradient boosting regressor's sequential approach may be better suited to capture the nuanced patterns in outage duration data.

### Feature Engineering
#### Season Feature
The Season feature transforms the numerical column MONTHS into categorical seasons to capture seasonal weather patterns that may affect power infrastructure. Winter months often experience ice storms and heavy snow, while summer brings thunderstorms and heat-related equipment failures. By grouping months into meaningful seasonal categories, we believe the model can identify stronger relationships between time of year and outage duration than with just numerical month values.

#### Hurricane Season Interaction
This feature creates a specific indicator for hurricanes occurring during the official hurricane season (June-October). Hurricane impacts during peak season tend to be more intense and widespread as compared to the off-season hurricanes. This feature will help the model distinguish between wether the hurricane is in season or off-season. 
#### Customer Impact Ratio Feature
We engineered a Customer Impact Ratio feature that calculates the percentage of a region's total population affected by each power outage. This ratio provides crucial insight into the relative severity and scale of outages across different regions, regardless of their absolute population size.

### Making the Model

First, we categorized our features into distinct groups based on their data types and required transformations:

**Original categorical features** includes climate region, outage cause category, and hurricane indicators

**Engineered categorical features** includes our newly created season and hurricane season variables

**Standard scale features** includes year and land percentage, which benefit from normalization

**Quantile transform features** addresses highly skewed distributions like customer counts and population metrics

The pipeline construction follows a logical sequence:
1. **Feature engineering step** applies our custom transformations using a FunctionTransformer
2. **Preprocessing step** uses ColumnTransformer to apply the appropriate transformations to each feature group:
   - OneHotEncoder for categorical variables (with drop='first' to avoid multicollinearity)
   - StandardScaler to normalize year and land percentage features. We do this because unlike linear models, tree-based algorithms like gradient boosting are highly sensitive to the scale of input features
   - QuantileTransformer to normalize the distribution of highly skewed features like population metrics

3. **Model step** implements a GradientBoostingRegressor as our prediction algorithm


### Hyperparameter Tuning
For our Gradient Boosting Regressor, we plan to tune the following hyperparameters:

n_estimators [50, 100, 200, 300]: This parameter controls the number of boosting stages (trees). More trees generally reduce the error but increase computation time and risk of overfitting. We're testing moderate values to balance complexity and performance.

max_depth [3, 5, 7, 9]: This limits the maximum depth of each tree. Deeper trees can capture more complex patterns but risk overfitting to noise in the training data. We're testing a range from shallow to moderately deep trees.

learning_rate [0.01, 0.05, 0.1]: This controls how much each tree contributes to the final prediction. Smaller values require more trees but often yield better performance. We're testing small values to ensure stable improvements during the gradient boosting process.

min_samples_split [2, 5, 10]: This sets the minimum number of samples required to split an internal node. Higher values prevent creating nodes with few samples, which helps prevent overfitting. We're testing values that represent different levels of regularization.

#### Grid Search Results

Best Gradient Boosting parameters: {'regressor__learning_rate': 0.01, 'regressor__max_depth': 9, 'regressor__min_samples_split': 2, 'regressor__n_estimators': 50}
Best Gradient Boosting CV RMSE: 5566.043095740817


#### Interpreting the Grid Search Results

1. **learning_rate:** 0.01 - a relatively small learning rate of 0.01 was the most optimal. This suggests that the model benefits from making small, careful steps during training to avoid overshooting the optimal solution.
2. **max_depth:** 9 - Trees with a maximum depth of 9 levels performed best. This is a moderate depth that allows the model to capture reasonably complex relationships in the data without overfitting to the noise.
3. **min_samples_split:** 2 - The minimum of 2 samples required to split a node was optimal. This allows the model to create very specific splits, even for less common outage scenarios. 
4. **n_estimators:** 50 - The model performed best with 50 trees. This is the lower end of the tested range (50, 100, 200, 300), suggesting that more trees weren't necessary for improvement. 
5. **Cross-Validation RMSE:** 5566.04 minutes - This represents the average error in the model's predictions across all cross-validation folds. While this might seem high compared to the baseline model's RMSE, the baseline RMSE came from a single train-test split, while the CV RMSE represents performance across multiple data folds.


#### Model Evaluation Results
RMSE: 4294.763166121633 minutes
Improvement over baseline: 16.66474446103111%

#### Interpreting the Results
The final model achieves a staggering 16.66% reduction in RMSE compared to the baseline model (5,153.60 minutes).
This result indicates that the engineered features optimized hyperparameters have successfully captured additional patterns in outage durations that were missed in the baseline model.


## Fairness Analysis

### Group Selection and Rationale

For our fairness analysis, we evaluated whether our final model performs equally well for hurricane-related outages versus non-hurricane-related outages.
We believe this comparison is particularly relevant because:
- Hurricane-related outages are often more severe and widespread than other types of outages – generally speaking
- The infrastructure damage from hurricanes can be more complex and difficult to repair
- These two groups represent fundamentally different outage scenarios that power companies must respond to
- Accurate predictions for both groups are crucial for effective resource allocation during emergencies


### Metric of Evaluation

We chose Root Mean Squared Error (RMSE) as our evaluation metric, which is consistent with our approach throughout the project. The RMSE measures the average magnitude of prediction errors in minutes, which directly relates to our prediction task of estimating outage durations.


### Evaluation Results

RMSE for hurricane outages: 7260.250758981379 minutes
RMSE for non-hurricane outages: 4097.652727316633 minutes
Observed difference in RMSE: 3162.5980316647456 minutes

<iframe
  src="assets/fair1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Hypotheses:
**Null Hypothesis:** Our model is fair. The RMSE for hurricane outages and non-hurricane outages are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis:** Our model is unfair. The RMSE for hurricane outages is different from the RMSE for non-hurricane outages.


### Permutation Test
**Test Statistic:** We used the absolute difference in RMSE between the two groups as our test statistic. 

**Significance Level:** We chose a standard significance level of 0.05 for our analysis

<iframe
  src="assets/fair2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
p-value: 0.044295570442955706

### Interpreting the results
Since the **p-value** is 0.044 which is less than the significance level we established of 0.05, this result is statistically significant. That is, we reject the null hypothesis. This provides evidence that our model performs differently for hurricane and non-hurricane outages. This permutation test suggests that the observed difference in prediction accuracy - measured by RMSE - between these two categories is unlikely to be due to random chance alone. 
