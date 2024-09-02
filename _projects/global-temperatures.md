---
title: "Earth surface temperature and the impacts of climate change "
excerpt: "My first data analysis project, for me to learn and improve my skills. In this project I look at global temperature data and use different methods of plottng, to see trends. <br/><br/> <br/><img src='/images/projects/global_temps/global_land_average.png'>"
collection: projects
featured: true
image: "<br/><br/> /images/projects/global_temps/global_land_average.png"
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
from scipy.stats import linregress
```

### Plotting average global land temperatures over time

For the initial analysis, I decided to plot average global land temperaures, to see the general trend. Luckily this data came with uncertanties, which we will use later on.


```python
global_temps = pd.read_csv('GlobalTemperatures.csv')

# Extract and convert Year to numeric
global_temps['Year'] = pd.to_numeric(global_temps['dt'].str[0:4])

# Remove rows with missing 'LandAverageTemperature' values
global_temps_clean = global_temps.dropna(subset=['LandAverageTemperature'])

# Calculate uncertainties
global_temps['LandAverageTemperature+Unc'] = global_temps['LandAverageTemperature'] + global_temps['LandAverageTemperatureUncertainty']
global_temps['LandAverageTemperature-Unc'] = global_temps['LandAverageTemperature'] - global_temps['LandAverageTemperatureUncertainty']


```


```python
# Convert Year to numeric
global_temps['Year'] = pd.to_numeric(global_temps['Year'])
years = np.arange(global_temps['Year'].min(), global_temps['Year'].max() + 1, 10)

# Calculate uncertainties
global_temps['LandAverageTemperature+Unc'] = global_temps['LandAverageTemperature'] + global_temps['LandAverageTemperatureUncertainty']
global_temps['LandAverageTemperature-Unc'] = global_temps['LandAverageTemperature'] - global_temps['LandAverageTemperatureUncertainty']

# Create the figure and axes
fig, ax = plt.subplots(figsize=(10, 5))

# Plot the data with updated colors and labels for the legend
sns.lineplot(data=global_temps, x='Year', y='LandAverageTemperature', color='#B39DDB', ax=ax, label='Average Temperature')  # Soft Purple
sns.lineplot(data=global_temps, x='Year', y='LandAverageTemperature+Unc', color='#EF5350', ax=ax, label='+ Uncertainty')  # Soft Red
sns.lineplot(data=global_temps, x='Year', y='LandAverageTemperature-Unc', color='#66BB6A', ax=ax, label='- Uncertainty')  # Green

# Add the legend
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
    


While it appears there is a trend upward here, it is difficult to directly see to be sure, in part due to the large uncertainties in earlier measurements. If we focus on just the average temperature, we can then plot a trendline using a linear regression model.


```python
# Calculating the trendline using the cleaned data (to avoid nan errors)
slope, intercept, r_value, p_value, std_err = linregress(global_temps_clean['Year'], global_temps_clean['LandAverageTemperature'])

# Generating the trendline values
trendline = intercept + slope * global_temps_clean['Year']

# Create the figure and axes
fig, ax = plt.subplots(figsize=(10, 5))

# Plotting the average temperature using the cleaned data
sns.lineplot(data=global_temps_clean, x='Year', y='LandAverageTemperature', color='#B39DDB', ax=ax, label='Average Temperature')  # Soft Purple

# Plotting the trendline on top
ax.plot(global_temps_clean['Year'], trendline, color='red', linestyle='--', label='Trendline')


ax.legend(loc='lower right')

# Set the title and labels
plt.title('Average Global Land Temperature with Trendline', fontsize=14)
plt.ylabel('Land Average Temperature (ºC)')
plt.xlabel('Year')

# Set x-ticks and labels
ax.set_xticks(years)
ax.set_xticklabels(years, rotation=45, fontsize=10)

# Save and show the figure
plt.savefig('global_land_average_with_trendline.png')
plt.show()

```


    
![png](/images/projects/global_temps/trendline.png)
    


So we see a clear trend upward. Early data is less tightly bound, due to the aforementioned uncertainties, so the trendline perhaps appears less steep than it may othwerwise be. 

### We can repeat this for the ocean temperatures (noting that the data starts in 1850)


```python
global_temps['LandAndOceanAverageTemperature+Unc'] = (
    global_temps['LandAndOceanAverageTemperature'] +
    global_temps['LandAndOceanAverageTemperatureUncertainty']
)
global_temps['LandAndOceanAverageTemperature-Unc'] = (
    global_temps['LandAndOceanAverageTemperature'] -
    global_temps['LandAndOceanAverageTemperatureUncertainty']
)

# Create the figure and axes
fig, ax = plt.subplots(figsize=(10, 5))

# Plot the ocean temperature data
sns.lineplot(data=global_temps, x='Year', y='LandAndOceanAverageTemperature', color='#B39DDB', ax=ax, label='Average Temperature')  
sns.lineplot(data=global_temps, x='Year', y='LandAndOceanAverageTemperature+Unc', color='#EF5350', ax=ax, label='+ Uncertainty')  
sns.lineplot(data=global_temps, x='Year', y='LandAndOceanAverageTemperature-Unc', color='#66BB6A', ax=ax, label='- Uncertainty')  

# Set the title and labels
plt.title('Average Global Ocean Temperature Since 1850', fontsize=14)
plt.ylabel('Ocean Average Temperature (ºC)')
plt.xlabel('Year')

# Set x-ticks and labels
# Set x-ticks starting from 1850
ax.set_xticks(np.arange(1850, global_temps['Year'].max() + 1, 5))
ax.set_xticklabels(np.arange(1850, global_temps['Year'].max() + 1, 5), rotation=45, fontsize=10)


# Add the legend
ax.legend(loc='lower right')

# Save and show the figure
fig.savefig('ocean.png')
plt.show()

```


    
![png](/images/projects/global_temps/global_ocean_average.png)
    




