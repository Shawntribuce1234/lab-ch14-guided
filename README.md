# OLS Diagnostics: When the Standard Error Is Lying

## Objective

In this lab, I explored how regression diagnostics help identify problems with standard errors, outliers, and model assumptions, and why passing these tests doesn't always mean our estimates are accurate.

## Methodology

- Simulated data with heteroskedasticity, where the spread of errors increases with x.
- Compared classical OLS and HC3 standard errors to see how heteroskedasticity affects confidence intervals.
- Ran 400 simulations to measure how often the confidence intervals captured the true slope.
- Analyzed four residual plots using Breusch-Pagan and Jarque-Bera tests to identify changing variance, missing curvature, and outliers.
- Used Cook's distance to measure how much individual observations influenced the regression.
- Removed an influential observation to compare changes in the slope and standard error.
- Fitted a regression using California Housing data to examine HC3 standard errors and the effect of districts at the housing value ceiling.
- Created an `hc3_se()` function to calculate HC3 standard errors for different variables and datasets.
- Simulated a regression with an omitted confounder to see whether diagnostic tests could identify causal bias.

## Key Findings

### Classical vs. HC3 Standard Errors

Both regressions estimated the same slope of 2.0147, but the classical standard error was 0.0264 compared to 0.0290 using HC3. This showed that heteroskedasticity can affect our uncertainty estimates without changing the coefficient itself.

Across 400 simulations, the classical confidence interval covered the true slope 91.8% of the time with a Monte Carlo standard error of 1.4%. The HC3 interval covered it 93.5% of the time with a Monte Carlo standard error of 1.2%.

### Regression Diagnostics

I compared four residual plots and found that different problems require different tests. The well-behaved model showed no clear issues, while the heteroskedastic model had changing error variance.

The missing-curvature model had a Breusch-Pagan p-value of 0.7918 even though the residual plot showed a curved relationship. This showed that passing one diagnostic test doesn't mean the model is correctly specified.

### Influential Observations

After removing one observation from a dataset of 600, the slope changed by 0.0182, or 0.58 of its standard error. However, the standard error decreased by 33%, from 0.0317 to 0.0213.

This showed that an influential observation can affect uncertainty more than the coefficient itself, but removing observations just to improve the results can introduce bias.

### California Housing Analysis

I used California Housing data to examine how robust standard errors change when observations at the housing value ceiling are removed.

The HC3 standard error on `AveRooms` was 0.0121 with all districts and decreased to 0.0082 after excluding the 965 districts at the 5.0 ceiling.

Although the standard error became smaller, this didn't necessarily mean the model improved because removing those districts could introduce selection bias.

### Omitted Variable Bias Challenge

I simulated a regression where the true coefficient was 2.0000, but the estimated coefficient was 3.5276 because I left out a confounder.

The Breusch-Pagan test passed with a p-value of 0.6731, and the largest Cook's distance was only 0.0091. However, Jarque-Bera failed with a p-value of 0.0045.

This showed that even when some diagnostics pass, the regression can still give a misleading estimate because of omitted variable bias.

## Conclusion

This lab helped me understand that regression diagnostics are important for checking model assumptions, but they don't automatically tell us whether our estimates are correct. Heteroskedasticity can make standard errors unreliable, outliers can influence results, and missing variables can create bias even when the model looks fine.

My biggest takeaway is that passing statistical tests doesn't always mean we should trust the regression. We also need to understand how the data was collected and whether important variables were left out.
