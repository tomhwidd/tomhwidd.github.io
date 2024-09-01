---
title: "Exoplanet light curves using TESS data"
excerpt: "A short project downloading, cleaning and folding light curve data to demonstrate the ability to detect exoplanets. 1<br/><img src='/images/500x300.png'>"
collection: portfolio
---

### 2. Select file to plot using ipywidget

Now we'll use a widget to list the files and allow the user to select one to plot.  This can be done using an `ipywdigets` dropdown box.


```python
# Define the list of options to display
# Just want to show the filename, not the complete path, so chop off path name here
lstOptions = dirlist.copy()

for ix in range (len(dirlist)):
    lstOptions[ix] = dirlist[ix][nPathLen:]

def on_selection_change(change):
    with output_2:
        clear_output()
        print(f"Selected file: {drpFile.value}")

drpFile = widgets.Dropdown(options = lstOptions, description = "File:")

output_2 = widgets.Output()

display(drpFile, output_2)

# Call function each time the value is changed
drpFile.observe(on_selection_change, names = "value");
```


    Dropdown(description='File:', options=('50.7.csv', '150.8.csv', '31.1.csv', '12.7.csv', '40.4.csv', '101.csv',…



    Output()


The next cell is simply a check that we can retrieve the selected value from the dropdown box


```python
strSelectedFile = strPath + drpFile.value
print(strSelectedFile)
```

    C:/OU/SXPS288/DataFiles/ReferenceSpectra/50.7.csv
    

## 3 Open file and plot contents

In order to open the data files and read the contents into a Pandas DataFrame ready for processing and plotting, we need to understand the file format and contents.  The best way to do this is to open the files in a text editor and inspect the contents visually. 

Do this now with some of the `.jdx` files that you have downloaded. 

You will find that the files start with a number of header lines beginning with "##", followed by the data, in the form of a number of columns.  There are no column headers, but the first column contains wavenumber values (in $\text{cm}^{-1}$), with the subsequent columns containing intensity values (typically 5 repeats).  Crucially, the number of header lines is not fixed - it can be different in different files, so our program needs to take this into account. 

Since the `.jdx` files have variable numbers of headers, we need to read the headers first and determine the number of lines to discard before the data starts.



```python
## first, open the file and read all the lines into a list
f = open(strSelectedFile)
lstLines = f.readlines()
f.close()

## The first task is to determine the number of header lines
## Now iterate through the list to find the last line beginning with "##" that is NOT "##End"

nLastHeaderLine = 0
strTitle = ""
strYlabel = ""

## Read through the file to find the number of header lines
## Some of the lines contain useful information, such as the name of the substance
## or the units, so we will make a note of these as they are found

for ix in range((len(lstLines))):
    if (lstLines[ix][:8] == "##TITLE="):
        strTitle = lstLines[ix][8:]
    if (lstLines[ix][:9] == "##YUNITS="):
        strYlabel = lstLines[ix][9:]
    if (lstLines[ix][:2] == "##") and (lstLines[ix][:5] != "##END"):
        nLastHeaderLine = ix+1
        
print(f"File {strTitle} has {nLastHeaderLine} header lines")

## Now, can read dataset using Pandas
df_Spectrum = pd.read_csv(strSelectedFile, skiprows = nLastHeaderLine, delimiter = "\s+", 
                          names = ["Wavenumber", "A", "B", "C", "D", "E"])

## Average the values in the five data columns to get a single averaged spectrum
df_Spectrum["Mean"] = df_Spectrum["A"]+df_Spectrum["B"]+df_Spectrum["C"]+df_Spectrum["D"]+df_Spectrum["E"]
df_Spectrum["Mean"] /= 5
df_Spectrum.head()
        
```

    File  has 0 header lines
    