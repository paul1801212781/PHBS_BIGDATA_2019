# Big Data Analysis Assignment 2
## Yongqiao Chen 1801212781
### Problem 1
#### Q1: Calculate the closed form solution of linear regression  
**Python code:**  
import numpy as np  
import pandas as pd

df = pd.read_excel('C:\\Users\\Lenovo\\Desktop\\PHBS\\研二\\module 2\\Big data analysis\\HW\\climate_change_1.xlsx')  
y_training=np.array(df.loc[:283,['Temp']])  
y_training=y_training.reshape(len(y_training),1)  
x_training=np.array(df.iloc[:284,2:10])  
x_training=x_training.reshape(len(y),8)  

def closed_form_1(x,y):  
&#8195;x=np.concatenate((np.ones((len(y),1)),x),axis=1)  
&#8195;parameter=np.linalg.inv(x.T@x)@x.T@y  
&#8195;return parameter  
closed_form_1(x_training,y_training)

#### Q2: Formula and R square
**Mathematical formula:**  
Temp=-124.59+0.0642*MEI+0.0065*CO2+0.0001*CH4-0.0165*N2O-0.0066*CFC-11+0.0038*CFC-12+0.0931*TSI-1.537*Aerosols  
The R_square for training data is 75.09%  
The R_square for testing data is 22.52%  

**Python code:**  
def R_square(para,x,y):  
&#8195;x=np.concatenate((np.ones((len(y),1)),x),axis=1)  
&#8195;y_hat=x@para  
&#8195;TSS=np.sum((y-np.mean(y))**2)  
&#8195;ESS=np.sum((y_hat-np.mean(y))**2)  
&#8195;return ESS/TSS  
para_closed_form_1=closed_form_1(x_training,y_training)  
R_square(para_closed_form_1,x_training,y_training)  

y_testing=np.array(df.loc[284:,['Temp']])  
y_testing=y_testing.reshape(len(y_testing),1)  
x_testing=np.array(df.iloc[284:,2:10])  
x_testing=x_testing.reshape(len(y_testing),8)  
R_square(para_closed_form_1,x_testing,y_testing)  

#### Q3: Significant Variables in the Model
Regressors | constant | MEI | CO2 | CH4 | N2O | CFC-11 | CFC-12 | TSI | Aerosols
---------- | ---------| --- | --- | --- | --- | ------ | ------ | --- | --------
Coefficients | -124.59| 0.0642 | 0.0065 | 0.0001 | -0.0165 | -0.0066 | 0.0038 | 0.0931 | -1.537
t-statistics | -6.265 | 9.923 | 2.826 | 0.240 | -1.930 | -4.078 | 3.757 | 6.313 | -7.210
p-value | 0.000 | 0.000 | 0.005 | 0.810 | 0.055 | 0.000 | 0.000 | 0.000 | 0.000

For 5% significance level, all variables above are significant except CH4 and N2O.  

**Python code:**  
para=closed_form_1(x_training,y_training)  
x_training_re=np.concatenate((np.ones((len(y_training),1)),x_training),axis=1)  
y_training_hat=x_training_re@para  
TSS=np.sum((y_training-np.mean(y_training))**2)  
ESS=np.sum((y_training_hat-np.mean(y_training))**2)  
RSS=TSS-ESS  
sigma_square=RSS/(len(y_training)-len(para))  
SE_mat=sigma_square*np.linalg.inv(x_training_re.T@x_training_re)  

t_stat=np.zeros(len(para))  
for ii in range(len(para)):  
    t_stat[ii]=para[ii]/(SE_mat[ii,ii]**0.5)  
t_stat  

#### Q4: Necessary Condition and application to dataset 2
**Necessary condition:** 
1. Dependent variable is linearly correlated with regressors (linear in parameters)
1. Exogeneity between error terms and regressors (they are uncorrelated)
1. Homoskedasticity of error terms (variance of error term is constant)
1. No autocorrelation of error terms
1. No multicollinearity of regressors (otherwise inverse of x.T@x is not feasible)
1. Normality of error term (under large sample, this condition is not necessary)

??What is the problem

### Problem 2
#### Q1: Loss Function of L1 and L2 Regularization  
**cost function of L1 regularization (LASSO):** 
![cost function of L1](https://s2.ax1x.com/2019/12/20/QOLnyj.png)

**cost function of L2 regularization (Ridge):** 
![cost function of L2](https://s2.ax1x.com/2019/12/20/QOqvWD.png)

#### Q2: Function for Regularization  
**Python code:**  
def closed_form_2(x,y,lumbda):  
&#8195;x=np.concatenate((np.ones((len(y),1)),x),axis=1)  
&#8195;parameter=np.linalg.inv(x.T@x+lumbda*np.eye(len(x[0,:])))@x.T@y  
&#8195;return parameter  

#### Q3: Comparison and Explaination
Lumbda | constant | MEI | CO2 | CH4 | N2O | CFC-11 | CFC-12 | TSI | Aerosols
------ | ---------| --- | --- | --- | --- | ------ | ------ | --- | --------
0 | -124.59| 0.0642 | 0.0065 | 0.0001 | -0.0165 | -0.0066 | 0.0038 | 0.0931 | -1.537
0.1 | -0.0250 | 0.0507 | 0.00699 | 0.000131 | -0.0148 | -0.00608 | 0.00366 | 0.00136 | -0.871
1 | -0.00229 | 0.0440 | 0.00804 | 0.000214 | -0.0169 | -0.00647 | 0.00377 | 0.00146 | -0.212
10 | -0.000220 | 0.0405 | 0.00815 | 0.000205 | -0.0161 | -0.00636 | 0.00369 | 0.00126 | -0.0244

**Reasons for robustness:**  
Regularization term in the cost function prevent the coefficients to fit so perfectly to overfit. Regularization term shrink the parameter to zeros. With such constraints, parameters are less subject to the noise of training data so they can perform better in testing. 

#### Q4: Choosing the optimal lumbda  
Lumbda | 0 | 0.001 | 0.01 | 0.1 | 1 | 10
------ | --| ----- | ---- | --- | - | ---
Training Data| 75.09% |71.48%| 71.17%| 69.45%| 67.95%|67.46%
Testing Data | 22.52% | 56.25%| 58.53%| 67.33%| 84.68%| 94.09%
