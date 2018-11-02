

```python
%reset
```

    Once deleted, variables cannot be recovered. Proceed (y/[n])? y
    


```python
import os
cwd = os.getcwd()
```


```python
cwd
```




    'C:\\Users\\cevi herdian'



# DATA SCIENTIST

In this tutorial, I only explain you what you need to be a data scientist neither more nor less.

Data scientist need to have these skills:

1.Basic Tools: Like **python**, **R** or **SQL**. You do not need to know everything. What you only need is to learn how to use python

2.Basic Statistics: Like mean, median or standart deviation. If you know basic statistics, you can use python easily.

3.Data Munging: Working with messy and difficult data. Like a inconsistent date and string formatting. As you guess, python helps us.

4.Data Visualization: Title is actually explanatory. We will visualize the data with python like matplot and seaborn libraries.

5.Machine Learning: You do not need to understand math behind the machine learning technique. You only need is understanding basics of machine learning and learning how to implement it while using python.


# *Confucius: Give a man a fish, and you feed him for a day. Teach a man to fish, and you feed him for a lifetime*

1. [Machine Learning](#1)
    1. [Supervised Learning](#2)
        1. [EDA(Exploratory Data Analysis)](#3)
        1. [K-Nearest Neighbors (KNN)](#4)
        1. [Regression](#5)
        1. [Cross Validation (CV)](#6)
        1. [ROC Curve](#7)
        1. [Hyperparameter Tuning](#8)
        1. [Pre-procesing Data](#9)
    1. [Unsupervised Learning](#10)
        1. [Kmeans Clustering](#11)
        1. [Evaluation of Clustering](#12)
        1. [Standardization](#13)
        1. [Hierachy](#14)
        1. [T - Distributed Stochastic Neighbor Embedding (T - SNE)](#15)
        1. [Principle Component Analysis (PCA)](#16)


```python
# This Python 3 environment comes with many helpful analytics libraries installed
# It is defined by the kaggle/python docker image: https://github.com/kaggle/docker-python
# For example, here's several helpful packages to load in 

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import matplotlib.pyplot as plt
import seaborn as sns
# Input data files are available in the "../input/" directory.
# For example, running this (by clicking run or pressing Shift+Enter) will list the files in the input directory
# import warnings
import warnings
# ignore warnings
warnings.filterwarnings("ignore")
#from subprocess import check_output
#print(check_output(["ls", "../input"]).decode("utf8"))

# Any results you write to the current directory are saved as output.
```


```python
# read csv (comma separated value) into data
data = pd.read_csv('column_2C_weka.csv')
print(plt.style.available) # look at available plot styles
plt.style.use('ggplot')
```

    ['bmh', 'classic', 'dark_background', 'fast', 'fivethirtyeight', 'ggplot', 'grayscale', 'seaborn-bright', 'seaborn-colorblind', 'seaborn-dark-palette', 'seaborn-dark', 'seaborn-darkgrid', 'seaborn-deep', 'seaborn-muted', 'seaborn-notebook', 'seaborn-paper', 'seaborn-pastel', 'seaborn-poster', 'seaborn-talk', 'seaborn-ticks', 'seaborn-white', 'seaborn-whitegrid', 'seaborn', 'Solarize_Light2', 'tableau-colorblind10', '_classic_test']
    

<a id="1"></a> <br>
# MACHINE LEARNING (ML)
In python there are some ML libraries like sklearn (scikit-learn), keras or tensorflow. We will use sklearn (scikit-learn).

For info about scikit-learn: http://scikit-learn.org/stable/index.html

<a id="2"></a> <br>
## A. SUPERVISED LEARNING
* Supervised learning: It uses data that has labels. Example, there are orthopedic patients data that have labels *normal* and *abnormal*.
    * There are features(predictor variable) and target variable. Features are like *pelvic radius* or *sacral slope*(If you have no idea what these are like me, you can look images in google like what I did :) )Target variables are labels *normal* and *abnormal*
    * Aim is that as given features(input) predict whether target variable(output) is *normal* or *abnormal*
    * **Classification: target variable consists of categories like normal or abnormal**
    * **Regression: target variable is continious like stock market**
    * If these explanations are not enough for you, just google them. However, be careful about terminology: features = predictor variable = independent variable = columns = inputs. target variable = responce variable = class = dependent variable = output = result

<a id="3"></a> <br>
### EXPLORATORY DATA ANALYSIS (EDA)
* In order to make something in data, as you know you need to explore data. Detailed exploratory data analysis is in my Data Science Tutorial for Beginners
* I always start with *head()* to see features that are *pelvic_incidence,	pelvic_tilt numeric,	lumbar_lordosis_angle,	sacral_slope,	pelvic_radius* and 	*degree_spondylolisthesis* and target variable that is *class*
* head(): default value of it shows first 5 rows(samples). If you want to see for example 100 rows just write head(100)


```python
# to see features and target variable
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pelvic_incidence</th>
      <th>pelvic_tilt numeric</th>
      <th>lumbar_lordosis_angle</th>
      <th>sacral_slope</th>
      <th>pelvic_radius</th>
      <th>degree_spondylolisthesis</th>
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63.027818</td>
      <td>22.552586</td>
      <td>39.609117</td>
      <td>40.475232</td>
      <td>98.672917</td>
      <td>-0.254400</td>
      <td>Abnormal</td>
    </tr>
    <tr>
      <th>1</th>
      <td>39.056951</td>
      <td>10.060991</td>
      <td>25.015378</td>
      <td>28.995960</td>
      <td>114.405425</td>
      <td>4.564259</td>
      <td>Abnormal</td>
    </tr>
    <tr>
      <th>2</th>
      <td>68.832021</td>
      <td>22.218482</td>
      <td>50.092194</td>
      <td>46.613539</td>
      <td>105.985135</td>
      <td>-3.530317</td>
      <td>Abnormal</td>
    </tr>
    <tr>
      <th>3</th>
      <td>69.297008</td>
      <td>24.652878</td>
      <td>44.311238</td>
      <td>44.644130</td>
      <td>101.868495</td>
      <td>11.211523</td>
      <td>Abnormal</td>
    </tr>
    <tr>
      <th>4</th>
      <td>49.712859</td>
      <td>9.652075</td>
      <td>28.317406</td>
      <td>40.060784</td>
      <td>108.168725</td>
      <td>7.918501</td>
      <td>Abnormal</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Well know question is is there any NaN value and length of this data so lets look at info
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 310 entries, 0 to 309
    Data columns (total 7 columns):
    pelvic_incidence            310 non-null float64
    pelvic_tilt numeric         310 non-null float64
    lumbar_lordosis_angle       310 non-null float64
    sacral_slope                310 non-null float64
    pelvic_radius               310 non-null float64
    degree_spondylolisthesis    310 non-null float64
    class                       310 non-null object
    dtypes: float64(6), object(1)
    memory usage: 17.0+ KB
    

As you can see:
* length: 310 (range index)
* Features are float
* Target variables are object that is like string
* Okey we have some ideas about data but lets look go inside data deeper
    * describe(): I explain it in previous tutorial so there is a Quiz :)
        * Why we need to see statistics like mean, std, max or min? I hate from quizzes :) so answer: In order to visualize data, values should be closer each other. As you can see values looks like closer. At least there is no incompatible values like mean of one feature is 0.1 and other is 1000. Also there are another reasons that I will mention next parts.


```python
data.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pelvic_incidence</th>
      <th>pelvic_tilt numeric</th>
      <th>lumbar_lordosis_angle</th>
      <th>sacral_slope</th>
      <th>pelvic_radius</th>
      <th>degree_spondylolisthesis</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>310.000000</td>
      <td>310.000000</td>
      <td>310.000000</td>
      <td>310.000000</td>
      <td>310.000000</td>
      <td>310.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>60.496653</td>
      <td>17.542822</td>
      <td>51.930930</td>
      <td>42.953831</td>
      <td>117.920655</td>
      <td>26.296694</td>
    </tr>
    <tr>
      <th>std</th>
      <td>17.236520</td>
      <td>10.008330</td>
      <td>18.554064</td>
      <td>13.423102</td>
      <td>13.317377</td>
      <td>37.559027</td>
    </tr>
    <tr>
      <th>min</th>
      <td>26.147921</td>
      <td>-6.554948</td>
      <td>14.000000</td>
      <td>13.366931</td>
      <td>70.082575</td>
      <td>-11.058179</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>46.430294</td>
      <td>10.667069</td>
      <td>37.000000</td>
      <td>33.347122</td>
      <td>110.709196</td>
      <td>1.603727</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>58.691038</td>
      <td>16.357689</td>
      <td>49.562398</td>
      <td>42.404912</td>
      <td>118.268178</td>
      <td>11.767934</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>72.877696</td>
      <td>22.120395</td>
      <td>63.000000</td>
      <td>52.695888</td>
      <td>125.467674</td>
      <td>41.287352</td>
    </tr>
    <tr>
      <th>max</th>
      <td>129.834041</td>
      <td>49.431864</td>
      <td>125.742385</td>
      <td>121.429566</td>
      <td>163.071041</td>
      <td>418.543082</td>
    </tr>
  </tbody>
</table>
</div>



pd.plotting.scatter_matrix:
* green: *normal* and red: *abnormal*
* c:  color
* figsize: figure size
* diagonal: histohram of each features
* alpha: opacity
* s: size of marker
* marker: marker type 


```python
color_list = ['red' if i=='Abnormal' else 'green' for i in data.loc[:,'class']]
pd.plotting.scatter_matrix(data.loc[:, data.columns != 'class'],
                                       c=color_list,
                                       figsize= [15,15],
                                       diagonal='hist',
                                       alpha=0.5,
                                       s = 200,
                                       marker = '*',
                                       edgecolor= "black")
plt.show()

```


![png](output_16_0.png)


Okay, as you understand in scatter matrix there are relations between each feature but how many *normal(green)* and *abnormal(red)* classes are there. 
* Searborn library has *countplot()* that counts number of classes
* Also you can print it with *value_counts()* method

<br> This data looks like balanced. Actually there is no definiton or numeric value of balanced data but this data is balanced enough for us.
<br> Now lets learn first classification method KNN


```python
sns.countplot(x="class", data=data)
data.loc[:,'class'].value_counts()
```




    Abnormal    210
    Normal      100
    Name: class, dtype: int64




![png](output_18_1.png)

