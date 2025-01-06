# 7.1 Concepts
**No classes**, but the target is a value, an approximate  relationship between dependent and independent variables.

- Simple linear regression
- Multiple linear regression 

**2 proposites**:
- Explanatory: regression analysis explains the relationship between response and predictor variables
- Predictive: giva an estimate of the response based on the value of the predictors

![[Pasted image 20241002161106.png]]![[Pasted image 20241002161227.png]]
# 7.2 Types of relationships

- Deterministic *Not interested in that*
- **Statistic** INTERESTING

## 7.2.1 Fundamentals
- Hipothesis (assumptions)
	- Linearity (response variable is a linear combination of parameters and the predictor variable)
	- Normality (variables follow symmetric and Gaussian distribution)
- Preliminarity assessment of strength of the hypothesis:
	- Linear correlation coefficient (*Parson's coef.*)
		![[Pasted image 20241002161619.png]]
		- Si $r = 0$ -> No correlation
		- If $r > 0$ positive relationship
		- If $r < 0$ negative relationship
		And...
		- If $|r| < 0.5$ the correlation is, generally speaking, a weak one
		- If $|r| > 0.8$ the correlation is, generally speaking, a strong one
		![[Pasted image 20241002162118.png]]
>[!note]
>On the bottom row, although there is, obviously some kind of order in the samples, since there is no linear relation, the Parson coef. is 0 (no linear correlation)
		
![[Pasted image 20241002162314.png]]
		
>[!example] Visual case:
>![[Pasted image 20241002162725.png]]
>- (a) and (b): the points fit perfectly on the straight line, so that we have a linear relationship between the two variables:
>	- (a): with negative slope, which indicates that as X increases, Y becomes smaller and smaller -> $r<0$
>	- (b) with positive slope. $r>0$
>- (c): it is possible to ensure the existence of a strong relationship between the two variables, but it is not a linear relationship. $r=0$
>- (d): the points are completely dispersed, so that there is no type of relationship between the variables. $r=0$
>- (e) and (f): there is some kind of linear relationship between the two variables: 
>	- (e): a type of linear dependence with a negative slope $-0.8<r<-0.5$.
>	- (f): linear relationship with positive slope, but not as strong as the previous case. $0<r<0.8$.
>
>![[Pasted image 20241002163015.png]]


# 7.3 Parameter estimation
$$
e_i = y_i - \hat{y}_i = y_i - (b_0 = b_1x_i)
$$

But, how do we get the best fitting line? Well, it will be the one that minimizes differences between observed and predicted data:

$$
L = \sum_{i=1}^n (y_i - (b_0 + b_1x_i))
$$
By aplying derivatives with respect to b0 and b1, and setting them = 0 (maximum or local minima):

![[Pasted image 20241002163858.png]]

# 7.4 Validation (quality of the fit)

![[Pasted image 20241002165850.png]]
![[Pasted image 20241002165902.png]]
![[Pasted image 20241002165915.png]]

**How do we know if the fit adjusts well to observed data?**

- Residual plot: We are looking for the amount of error should be equal above and under the linera regression
	![[Pasted image 20241002170006.png]]
- Coefficient of determination (R-squared):
	![[Pasted image 20241002170410.png]]
	![[Pasted image 20241002170432.png]]
	in a percentual form, it explains what portion of the dependent variable is explainable due to the independent variable: the closer to 1 the better
	![[Pasted image 20241002170236.png]]
- Observed data vs predicted data

**Is the inferred model adequate for the general problem?**

# 7.4 Hypothesis testing
- The Pearson $r$ coefficient is equal to $R^2$, and informs us of the amount of outputs predicted match the reality (the higher the better)
- We have to draw conclusions also about populations (samples), and not just the samples, for which the $t-test$ is useful
	- The t-test allows validating linear relationship between the predictor variable and the response variable:
		$H_0:\beta_1 = 0$, the null hypothesis (no correlation) -> should be rejected with a certain confidence level
		$H_0:\beta_1 \neq 0$, the alternative hypothesis, this should be proven for our model to be valid; there is a significant linear relationship between the variables
	- Steps for hypothesis testing:
		1. Specify the null and alternative hypotheses
		2. Set a significance level $\alpha$ (typically 0.01 - 0.05)
		3. construct a statistic 𝑇 to test the null hypothesis $H_o$
		4. define a decision rule to reject, or not, the null hypothesis $𝐻_0$ 

>[!abstract] Annotation:
>Note that the intercept 𝛽0 determines the average value of the variable Y for a value of X equal to zero. Since it does not always have a realistic interpretation in the context of the problem, we only make statistical inference about the slope


## 7.4.1 Hypothesis check

### 1. Specify the null and alternative hypotheses

$H_0:\beta_1 = 0$, the null hypothesis (no correlation) -> should be rejected with a certain confidence level

$H_0:\beta_1 \neq 0$, the alternative hypothesis, this should be proven for our model to be valid; there is a significant linear relationship between the variables
### 2. Set a significance level $\alpha$ (typically 0.01 - 0.05)


### 3. construct a statistic 𝑇 to test the null hypothesis $H_o$

![[Pasted image 20241010182340.png]]
### 4. define a decision rule to reject, or not, the null hypothesis $𝐻_0$ 

![[Pasted image 20241010182354.png]]

### Interpreting result

interpreting the result of the hypothesis test 
-  the 𝑝-value indicates how likely is it to get such an extreme 𝑇 value if the null hypothesis 𝐻0 is true 
- if 𝑝-value ≤ 𝛼 means that there is sufficient evidence at the level $\alpha$ to conclude that there is a linear relationship in the population between the predictor and response variables → we reject the null hypothesis $𝐻_0$ – rejecting $𝐻_0$ entails accepting 𝐻𝑎 → there is a significant linear relationship between the variables 
- given 𝑇 and 𝑛 − 2, the 𝑝-value is obtained from the Student’s t-distribution tables or from some web sites

>[!example] #exam Example reggression:
>Sample set
>![[Pasted image 20241010182602.png]]
>Calculation of $\beta_0$, $\beta_1$
>![[Pasted image 20241010182612.png]]
>![[Pasted image 20241010182623.png]]
>Reggression line is defined:
>![[Pasted image 20241010182644.png]]
>Making the predictions (estimate the weight of a person that is 160cm tall)
>![[Pasted image 20241010182753.png]]
>60kg...ok
>Calculate $R^2$ (determination coef; extra columns for variables in equation):
>ObsH, ObsW, EstW, $Error_{obs/mean}$, $Error_{obs/mean}^2$, $\% error_{pred,mean}$,$\% error_{pred,mean}^2$, error, $error^2$ 
>![[Pasted image 20241010182845.png]]
>![[Pasted image 20241010183258.png]]
>$R^2$ is 0.561 (which means approx half of the observations can be predicted with this regression) 
>$r = \sqrt(R^2) = \pm 0.749$ (relationship)
>
>This tells us that the relationship is moderate, not reaching 0.8, which would be considered a strong correlation, but it is close nonetheless
>
>Residual analysis (works as a preliminary validation; difference between predicted and observed):
>![[Pasted image 20241010183718.png]]
>
>*We cannot observe any type of structure in the representation. Therefore, we can conclude that the regression model obtained is a good model to explain the relationship between the two variables*
>
>Objective is to have all those diferences well distributed up and down from 0, and the closest possible
>
>**TEST HIPOTHESIS**
>
>Specify the null alternative hypothesis:
>- Null hypothesis: 𝐻0: 𝛽1 = 0 (the variable 𝑥 is not explanatory) 
>- Alternative hypothesis: 𝐻𝑎: 𝛽1 ≠ 0 (the variable 𝑥 is explanatory)
>  
>Significance level $\alpha = 0.05 = 5\%$
> 
> Construct statistic T to test the null hypothesis:
> ![[Pasted image 20241010184129.png]]
> 
> Can we reject the null hipothesis?
> ![[Pasted image 20241010184147.png]]


