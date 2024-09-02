---
title: "Earth surface temperature and the impacts of climate change "
excerpt: "My first data analysis project, for me to learn and improve my skills. In this project I look at global temperature data and use different methods of plottng, to see trends.  <br/><img src='/images/projects/global_temps/global_land_average.png'>"
collection: projects
featured: true
image: "/images/projects/global_temps/global_land_average.png"
---

# Global temperature data

This project will be a work in progress, continually updated as I learn new python data analysis skills. The aim is to break down global temperature data by different measures, to see trends, outliers and get a general sense of the impacts of climate change.

Data was obtained through [Kaggle](https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data?resource=download) and all analysis was done through python using jupyter notebooks.







### Importing and packages and data

The first step was to import the relevant libraries needed for this data analysis. The following libraries may be updated as necessary.




```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import plotly as py
import seaborn as sns
```

### Plotting average global land temperatures over time

For the initial analysis, I decided to plot average global land temperaures, to see the general trend. Luckily this data came with uncertanties, which we will use later on.


```python
global_temps = pd.read_csv('GlobalTemperatures.csv')

# Extracting and converting the year to numeric
global_temps['Year'] = pd.to_numeric(global_temps['dt'].str[0:4]) 
#grabs the first 4 elements of the string ad turns into a number
```


```python
#creating an array of decades to use on plot
years = np.arange(global_temps['Year'].min(), global_temps['Year'].max() + 1, 10)

# Calculating upp and lower uncertainties
global_temps['LandAverageTemperature+Unc'] = 
global_temps['LandAverageTemperature'] 
+ global_temps['LandAverageTemperatureUncertainty']

global_temps['LandAverageTemperature-Unc'] = global_temps['LandAverageTemperature'] 
- global_temps['LandAverageTemperatureUncertainty']

# Creating the figure and axis
fig, ax = plt.subplots(figsize=(10, 5))

#Plotting average and uncertanties
sns.lineplot(data=global_temps,
             x='Year', y='LandAverageTemperature',
             color='#B39DDB', ax=ax, 
             label='Average Temperature')  # Soft Purple
sns.lineplot(data=global_temps,
             x='Year', y='LandAverageTemperature+Unc',
             color='#EF5350', ax=ax, label='+ Uncertainty')  # Soft Red
sns.lineplot(data=global_temps,
             x='Year', y='LandAverageTemperature-Unc',
              color='#66BB6A', ax=ax, 
              label='- Uncertainty')  # Green
ax.legend()

# Set the title and labels
plt.title('Average Global Land Temperature Since 1750', fontsize=14)
plt.ylabel('Land Average Temperature (ºC)')
plt.xlabel('Year')

# Set x-ticks and labels
ax.set_xticks(years)
ax.set_xticklabels(years, rotation=45, fontsize=10)

# Save and show the figure
plt.savefig('global_land_average.png')
plt.show()

```


    
![png](/images/projects/global_temps/global_land_average.png)
    


### We can repeat this for the ocean temperatures (noting that the data starts in 1850)


```python
#upper and lower uncertainties
global_temps['LandAndOceanAverageTemperature+Unc'] = (
    global_temps['LandAndOceanAverageTemperature'] +
    global_temps['LandAndOceanAverageTemperatureUncertainty']
)
global_temps['LandAndOceanAverageTemperature-Unc'] = (
    global_temps['LandAndOceanAverageTemperature'] -
    global_temps['LandAndOceanAverageTemperatureUncertainty']
)

# Creating the figure and axis
fig, ax = plt.subplots(figsize=(10, 5))

# Plotting ocean temperature data
sns.lineplot(data=global_temps,
             x='Year', y='LandAndOceanAverageTemperature', 
             color='#B39DDB', ax=ax, 
             label='Average Temperature')  
sns.lineplot(data=global_temps, 
            x='Year', y='LandAndOceanAverageTemperature+Unc', 
            color='#EF5350', ax=ax, 
            label='+ Uncertainty')  
sns.lineplot(data=global_temps,     
            x='Year', y='LandAndOceanAverageTemperature-Unc', 
            color='#66BB6A', ax=ax, 
            label='- Uncertainty')  

# Settig title etc
plt.title('Average Global Ocean Temperature Since 1850', fontsize=14)
plt.ylabel('Ocean Average Temperature (ºC)')
plt.xlabel('Year')

# Set x-ticks to be 5 years, starting at 1850
# steps of 5
ax.set_xticks(np.arange(1850, global_temps['Year'].max() + 1, 5))
ax.set_xticklabels(np.arange(1850, 
                   global_temps['Year'].max() + 1, 5),
                   rotation=45, fontsize=10)


ax.legend(loc='lower right')

# saving figure
fig.savefig('ocean.png')
plt.show()

```


    
![png](/images/projects/global_temps/global_ocean_average.png)
    


