# Big Data Analysis Assignment 2
## Yongqiao Chen 1801212781
### Problem 1
#### Q1: Calculate the closed form solution of linear regression
import numpy as np
import pandas as pd

df = pd.read_excel('C:\\Users\\Lenovo\\Desktop\\PHBS\\研二\\module 2\\Big data analysis\\HW\\climate_change_1.xlsx')
y_training=np.array(df.loc[:283,['Temp']])
y_training=y_training.reshape(len(y),1)
x_training=np.array(df.iloc[:284,2:10])
x_training=x_training.reshape(len(y),8)

def closed_form_1(x,y):
    x=np.concatenate((np.ones((len(y),1)),x),axis=1)
    parameter=np.linalg.inv(x.T@x)@x.T@y
    return parameter

closed_form_1(x_training,y_training)
