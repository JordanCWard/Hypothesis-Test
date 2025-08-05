Repair Our Air is formulating policy recommendations to improve the air quality in America, using the Environmental Protection Agency's Air Quality Index (AQI) to guide their decision making. An AQI value close to 0 signals "little to no" public health concern, while higher values are associated with increased risk to public health.

ROA is considering the following decisions:

1. ROA is considering a metropolitan-focused approach. Within California, they want to know if the mean AQI in Los Angeles County is statistically different from the rest of California.
2. With limited resources, ROA has to choose between New York and Ohio for their next regional office. Does New York have a lower AQI than Ohio?
3. A new policy will affect those states with a mean AQI of 10 or greater. Will Michigan be affected by this new policy?


```python
import pandas as pd        # For data manipulation
import numpy as np         # For numerical operations
from scipy import stats     # For statistical functions
```


```python
aqi = pd.read_csv('c4_epa_air_quality.csv')
```

### Hypothesis 1: ROA is considering a metropolitan-focused approach. Is the mean AQI in Los Angeles County statistically different from the rest of California?

Null and alternative hypotheses:

*   $H_0$: There is no difference in the mean AQI between Los Angeles County and the rest of California.
*   $H_A$: There is a difference in the mean AQI between Los Angeles County and the rest of California.

Significance level = 5%

Comparing the sample means between two independent samples: two-sample  𝑡-test


```python
california = aqi[aqi['state_name'] == 'California']
los_angeles = california[california['county_name'] == 'Los Angeles']
not_los_angeles = california[california['county_name'] != 'Los Angeles']

stats.ttest_ind(a=los_angeles['aqi'], b=not_los_angeles['aqi'], equal_var=False)
```




    Ttest_indResult(statistic=2.1107010796372014, pvalue=0.049839056842410995)



With a p-value being less than 0.05, reject the null hypothesis in favor of the alternative hypothesis.

Therefore, a metropolitan strategy may make sense in this case.

### Hypothesis 2: With limited resources, ROA has to choose between New York and Ohio for their next regional office. Does New York have a lower AQI than Ohio?

Null and alternative hypotheses:

*   $H_0$: The mean AQI of New York is greater than or equal to that of Ohio.
*   $H_A$: The mean AQI of New York is less than Ohio.

Significance level = 5%

Comparing the sample means between two independent samples in one direction: two-sample  𝑡-test


```python
new_york = aqi[aqi['state_name'] == 'New York']
ohio = aqi[aqi['state_name'] == 'Ohio']

stats.ttest_ind(a=new_york['aqi'], b=ohio['aqi'], alternative='less', equal_var=False)
```




    Ttest_indResult(statistic=-2.025951038880333, pvalue=0.030446502691934697)



With a p-value of less than 0.05 and a t-statistic less than 0, reject the null hypothesis in favor of the alternative hypothesis.

Therefore, conclude at the 5% significance level that New York has a lower mean AQI than Ohio.

###  Hypothesis 3: A new policy will affect those states with a mean AQI of 10 or greater. Will Michigan be affected by this new policy?

Null and alternative hypotheses:

*   $H_0$: The mean AQI of Michigan is less than or equal to 10.
*   $H_A$: The mean AQI of Michigan is greater than 10.

Significance level = 5%

Comparing one sample mean relative to a particular value in one direction: one-sample  𝑡-test


```python
michigan = aqi[aqi['state_name'] == 'Michigan']

stats.ttest_1samp(michigan['aqi'], 10, alternative='greater')
```




    Ttest_1sampResult(statistic=-1.7395913343286131, pvalue=0.9399405193140109)



With a p-value being greater than 0.05 and a t-statistic < 0, fail to reject the null hypothesis.

Cannot conclude at the 5% significance level that Michigan's mean AQI is greater than 10. This implies that Michigan would most likely not be affected by the new policy.
