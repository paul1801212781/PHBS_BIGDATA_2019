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
The R_square for training data is 75.10%  
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

