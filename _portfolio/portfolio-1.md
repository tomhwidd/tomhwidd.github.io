---
title: "Exoplanet light curves using TESS data"
excerpt: "A short project downloading, cleaning and folding light curve data to demonstrate the ability to detect exoplanets. <br/><br/> <br/><img src='/images/500x300.png'>"
collection: portfolio
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

#### PLotting average global temperatures over time

For the initial analysis, I decided to plot average global land temperaures, to see the general trend. Luckily this data came with uncertanties, which we will use later on.


```python
global_temps = pd.read_csv('GlobalTemperatures.csv')

# Extract and convert Year to numeric
global_temps['Year'] = pd.to_numeric(global_temps['dt'].str[0:4])


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
sns.lineplot(data=global_temps, x='Year', y='LandAverageTemperature', color='#B39DDB', ax=ax, label='Temperature')  # Soft Purple
sns.lineplot(data=global_temps, x='Year', y='LandAverageTemperature+Unc', color='#EF5350', ax=ax, label='+ Uncertainty')  # Soft Red
sns.lineplot(data=global_temps, x='Year', y='LandAverageTemperature-Unc', color='#66BB6A', ax=ax, label='- Uncertainty')  # Green

# Add the legend
ax.legend()



# Set the title and labels
plt.title('Average Global Land Temperature Over Time', fontsize=14)
plt.ylabel('Land Average Temperature (ºC)')
plt.xlabel('Year')

# Set x-ticks and labels
ax.set_xticks(years)
ax.set_xticklabels(years, rotation=45, fontsize=10)

# Save and show the figure
plt.savefig('The_Title_of_the_Graph.png')
plt.show()

```


    
![png](/images/projects/global_temps/global_land_average.png)
    

