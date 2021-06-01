# Geospatial-Plotting-in-python-with-geopandas
Two Jupyter notebook files containing university assignment tasks for Geospatial Analysis module


# Task 2.1

__SUMMARY__
- Step 1: Import required libraries
- Step 2: Read all requred datasets into dataframes
  - [total_population, urban_population] from worldbank datasets 
  - [Metadata] dataset for the above for data cleansing operation
  - [Natural earth] dataset for geometry details of country codes
- Step 3: Data Cleansing
  - Remove grouped country codes from [total_population, urban_population] like OECD, Arab Nations, etc
  - Deal with "-99" ISO_A3 codes in [Natural earth] dataset
- Step 4: Data Preparation
  - Make country codes as index of all datasets
  - calculate per_capita_urban_population using pandas division
  - join the above created dataset to natural earth dataset to add geometry values
- Step 5: Save the processed datasets for futher use
  - save the processed datasets so they can be used for Task 2.2
- Step 6: Choropleth Plots
  - Plot choropleth maps for the years 1990, 2000 & 2010

# STEP 1: IMPORT REQUIRED LIBRARIES
1. Pandas library to read/ edit tabular data from csv files, perform group operations if required, etc
2. Geopandas library to create geopandas dataframes, plot choropleth maps
3. matplotlib library to edit/ manipulate the plots


```python
from IPython.display import display
import pandas as pd
import geopandas as gpd
import matplotlib.pyplot as plt
from matplotlib.ticker import FormatStrFormatter
import numpy as np
import seaborn as sns
```

# STEP 2: READ ALL REQURED DATASETS INTO DATAFRAMES

## 2.1 Read total population dataset
Use skiprows to skip over the first 4 rows after which the table starts in the csv file


```python
total_pop = pd.read_csv(r'Input datasets/API_SP.POP.TOTL_DS2_en_csv_v2_1637443.csv', skiprows=4)
total_pop
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Name</th>
      <th>Country Code</th>
      <th>Indicator Name</th>
      <th>Indicator Code</th>
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>...</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
      <th>2020</th>
      <th>Unnamed: 65</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aruba</td>
      <td>ABW</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>54211.0</td>
      <td>55438.0</td>
      <td>56225.0</td>
      <td>56695.0</td>
      <td>57032.0</td>
      <td>57360.0</td>
      <td>...</td>
      <td>102560.0</td>
      <td>103159.0</td>
      <td>103774.0</td>
      <td>104341.0</td>
      <td>104872.0</td>
      <td>105366.0</td>
      <td>105845.0</td>
      <td>106314.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>8996973.0</td>
      <td>9169410.0</td>
      <td>9351441.0</td>
      <td>9543205.0</td>
      <td>9744781.0</td>
      <td>9956320.0</td>
      <td>...</td>
      <td>31161376.0</td>
      <td>32269589.0</td>
      <td>33370794.0</td>
      <td>34413603.0</td>
      <td>35383128.0</td>
      <td>36296400.0</td>
      <td>37172386.0</td>
      <td>38041754.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>5454933.0</td>
      <td>5531472.0</td>
      <td>5608539.0</td>
      <td>5679458.0</td>
      <td>5735044.0</td>
      <td>5770570.0</td>
      <td>...</td>
      <td>25107931.0</td>
      <td>26015780.0</td>
      <td>26941779.0</td>
      <td>27884381.0</td>
      <td>28842484.0</td>
      <td>29816748.0</td>
      <td>30809762.0</td>
      <td>31825295.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>1608800.0</td>
      <td>1659800.0</td>
      <td>1711319.0</td>
      <td>1762621.0</td>
      <td>1814135.0</td>
      <td>1864791.0</td>
      <td>...</td>
      <td>2900401.0</td>
      <td>2895092.0</td>
      <td>2889104.0</td>
      <td>2880703.0</td>
      <td>2876101.0</td>
      <td>2873457.0</td>
      <td>2866376.0</td>
      <td>2854191.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andorra</td>
      <td>AND</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>13411.0</td>
      <td>14375.0</td>
      <td>15370.0</td>
      <td>16412.0</td>
      <td>17469.0</td>
      <td>18549.0</td>
      <td>...</td>
      <td>82427.0</td>
      <td>80774.0</td>
      <td>79213.0</td>
      <td>78011.0</td>
      <td>77297.0</td>
      <td>77001.0</td>
      <td>77006.0</td>
      <td>77142.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>259</th>
      <td>Kosovo</td>
      <td>XKX</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>947000.0</td>
      <td>966000.0</td>
      <td>994000.0</td>
      <td>1022000.0</td>
      <td>1050000.0</td>
      <td>1078000.0</td>
      <td>...</td>
      <td>1807106.0</td>
      <td>1818117.0</td>
      <td>1812771.0</td>
      <td>1788196.0</td>
      <td>1777557.0</td>
      <td>1791003.0</td>
      <td>1797085.0</td>
      <td>1794248.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>260</th>
      <td>Yemen, Rep.</td>
      <td>YEM</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>5315355.0</td>
      <td>5393036.0</td>
      <td>5473671.0</td>
      <td>5556766.0</td>
      <td>5641597.0</td>
      <td>5727751.0</td>
      <td>...</td>
      <td>24473178.0</td>
      <td>25147109.0</td>
      <td>25823485.0</td>
      <td>26497889.0</td>
      <td>27168210.0</td>
      <td>27834821.0</td>
      <td>28498687.0</td>
      <td>29161922.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>261</th>
      <td>South Africa</td>
      <td>ZAF</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>17099840.0</td>
      <td>17524533.0</td>
      <td>17965725.0</td>
      <td>18423161.0</td>
      <td>18896307.0</td>
      <td>19384841.0</td>
      <td>...</td>
      <td>52834005.0</td>
      <td>53689236.0</td>
      <td>54545991.0</td>
      <td>55386367.0</td>
      <td>56203654.0</td>
      <td>57000451.0</td>
      <td>57779622.0</td>
      <td>58558270.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>262</th>
      <td>Zambia</td>
      <td>ZMB</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>3070776.0</td>
      <td>3164329.0</td>
      <td>3260650.0</td>
      <td>3360104.0</td>
      <td>3463213.0</td>
      <td>3570464.0</td>
      <td>...</td>
      <td>14465121.0</td>
      <td>14926504.0</td>
      <td>15399753.0</td>
      <td>15879361.0</td>
      <td>16363507.0</td>
      <td>16853688.0</td>
      <td>17351822.0</td>
      <td>17861030.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>263</th>
      <td>Zimbabwe</td>
      <td>ZWE</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>3776681.0</td>
      <td>3905034.0</td>
      <td>4039201.0</td>
      <td>4178726.0</td>
      <td>4322861.0</td>
      <td>4471177.0</td>
      <td>...</td>
      <td>13115131.0</td>
      <td>13350356.0</td>
      <td>13586681.0</td>
      <td>13814629.0</td>
      <td>14030390.0</td>
      <td>14236745.0</td>
      <td>14439018.0</td>
      <td>14645468.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>264 rows × 66 columns</p>
</div>



## 2.2 Read urban population dataset
Use skiprows to skip over the first 4 rows after which the table starts in the csv file


```python
urban_pop = pd.read_csv(r'Input datasets/API_SP.URB.TOTL_DS2_en_csv_v2_1868968.csv', skiprows=4)
urban_pop
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Name</th>
      <th>Country Code</th>
      <th>Indicator Name</th>
      <th>Indicator Code</th>
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>...</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
      <th>2020</th>
      <th>Unnamed: 65</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aruba</td>
      <td>ABW</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>27526.0</td>
      <td>28141.0</td>
      <td>28532.0</td>
      <td>28761.0</td>
      <td>28924.0</td>
      <td>29082.0</td>
      <td>...</td>
      <td>44057.0</td>
      <td>44348.0</td>
      <td>44665.0</td>
      <td>44979.0</td>
      <td>45296.0</td>
      <td>45616.0</td>
      <td>45948.0</td>
      <td>46295.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>755836.0</td>
      <td>796272.0</td>
      <td>839385.0</td>
      <td>885228.0</td>
      <td>934135.0</td>
      <td>986074.0</td>
      <td>...</td>
      <td>7528588.0</td>
      <td>7865067.0</td>
      <td>8204877.0</td>
      <td>8535606.0</td>
      <td>8852859.0</td>
      <td>9164841.0</td>
      <td>9477100.0</td>
      <td>9797273.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>569222.0</td>
      <td>597288.0</td>
      <td>628381.0</td>
      <td>660180.0</td>
      <td>691532.0</td>
      <td>721552.0</td>
      <td>...</td>
      <td>15383127.0</td>
      <td>16130304.0</td>
      <td>16900847.0</td>
      <td>17691524.0</td>
      <td>18502165.0</td>
      <td>19332881.0</td>
      <td>20184707.0</td>
      <td>21061025.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>493982.0</td>
      <td>513592.0</td>
      <td>530766.0</td>
      <td>547928.0</td>
      <td>565248.0</td>
      <td>582374.0</td>
      <td>...</td>
      <td>1575788.0</td>
      <td>1603505.0</td>
      <td>1630119.0</td>
      <td>1654503.0</td>
      <td>1680247.0</td>
      <td>1706345.0</td>
      <td>1728969.0</td>
      <td>1747593.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andorra</td>
      <td>AND</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>7839.0</td>
      <td>8766.0</td>
      <td>9754.0</td>
      <td>10811.0</td>
      <td>11915.0</td>
      <td>13067.0</td>
      <td>...</td>
      <td>73056.0</td>
      <td>71515.0</td>
      <td>70057.0</td>
      <td>68919.0</td>
      <td>68213.0</td>
      <td>67876.0</td>
      <td>67813.0</td>
      <td>67873.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>259</th>
      <td>Kosovo</td>
      <td>XKX</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>260</th>
      <td>Yemen, Rep.</td>
      <td>YEM</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>483697.0</td>
      <td>510127.0</td>
      <td>538117.0</td>
      <td>567679.0</td>
      <td>598799.0</td>
      <td>631542.0</td>
      <td>...</td>
      <td>8065870.0</td>
      <td>8439118.0</td>
      <td>8822594.0</td>
      <td>9215171.0</td>
      <td>9615916.0</td>
      <td>10024989.0</td>
      <td>10442489.0</td>
      <td>10869523.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>261</th>
      <td>South Africa</td>
      <td>ZAF</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>7971774.0</td>
      <td>8200255.0</td>
      <td>8427003.0</td>
      <td>8662570.0</td>
      <td>8906585.0</td>
      <td>9158950.0</td>
      <td>...</td>
      <td>33429132.0</td>
      <td>34249974.0</td>
      <td>35079618.0</td>
      <td>35905874.0</td>
      <td>36724030.0</td>
      <td>37534797.0</td>
      <td>38339668.0</td>
      <td>39149717.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>262</th>
      <td>Zambia</td>
      <td>ZMB</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>557192.0</td>
      <td>599672.0</td>
      <td>645120.0</td>
      <td>695945.0</td>
      <td>762426.0</td>
      <td>834489.0</td>
      <td>...</td>
      <td>5837255.0</td>
      <td>6099716.0</td>
      <td>6372726.0</td>
      <td>6654564.0</td>
      <td>6944345.0</td>
      <td>7243041.0</td>
      <td>7551686.0</td>
      <td>7871713.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>263</th>
      <td>Zimbabwe</td>
      <td>ZWE</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>476164.0</td>
      <td>500664.0</td>
      <td>528408.0</td>
      <td>567387.0</td>
      <td>609178.0</td>
      <td>653686.0</td>
      <td>...</td>
      <td>4306222.0</td>
      <td>4359425.0</td>
      <td>4416215.0</td>
      <td>4473868.0</td>
      <td>4531255.0</td>
      <td>4589499.0</td>
      <td>4650663.0</td>
      <td>4717305.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>264 rows × 66 columns</p>
</div>



## 2.3 Read world bank report metadata
- This dataset is required for data cleansing operation
- The total and urban population datasets include grouped data (such as OECD countries, Arab countries and even a record for entire world)
- The metadata information can be used to get the list of these grouped countries


```python
country_code_info = pd.read_csv(r'Input datasets/Metadata_Country_API_SP.POP.TOTL_DS2_en_csv_v2_2252106.csv')
country_code_info.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Code</th>
      <th>Region</th>
      <th>IncomeGroup</th>
      <th>SpecialNotes</th>
      <th>TableName</th>
      <th>Unnamed: 5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ABW</td>
      <td>Latin America &amp; Caribbean</td>
      <td>High income</td>
      <td>NaN</td>
      <td>Aruba</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AFG</td>
      <td>South Asia</td>
      <td>Low income</td>
      <td>NaN</td>
      <td>Afghanistan</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AGO</td>
      <td>Sub-Saharan Africa</td>
      <td>Lower middle income</td>
      <td>NaN</td>
      <td>Angola</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ALB</td>
      <td>Europe &amp; Central Asia</td>
      <td>Upper middle income</td>
      <td>NaN</td>
      <td>Albania</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AND</td>
      <td>Europe &amp; Central Asia</td>
      <td>High income</td>
      <td>NaN</td>
      <td>Andorra</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## 2.4 Read downloaded large scale geometry data from Natural earth website
- The large scale dataset is used because it has countries
- naturalearth lowres data is readily available in geopandas using _gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))_. But this was not used because only 177 countries are present in this (compared to over 210 countries in total and urban population datasets)


___Link to dataset source:___ https://www.naturalearthdata.com/downloads/10m-cultural-vectors/


```python
country_shapes = gpd.read_file(r"Input datasets/ne_10m_admin_0_countries\ne_10m_admin_0_countries.shp")
country_shapes.plot(figsize=(16, 12))
# print(len(total_pop['Country Code'].unique()))
# print(len(country_shapes.ISO_A3.unique()))

```




    <AxesSubplot:>




    
![png](output_12_1.png)
    


# STEP 3: DATA CLEANSING

## 3.1 Data Cleansing for total and urban pop
Remove records from total pop and urban pop which are entries for grouped countries like OECD.

This is done in two steps,
1. We get the list of country code for grouped countries from the world bank metadata dataset (These grouped countries have no data in Region column in metadata dataset)
2. We use this list to filter out these country codes from total and urban pop


```python
#display(country_code_info[country_code_info['Region'].isna()])

temp_list = []
temp_list = country_code_info[country_code_info['Region'].isna()]['Country Code'].tolist()
print(temp_list,)
```

    ['ARB', 'CEB', 'CSS', 'EAP', 'EAR', 'EAS', 'ECA', 'ECS', 'EMU', 'EUU', 'FCS', 'HIC', 'HPC', 'IBD', 'IBT', 'IDA', 'IDB', 'IDX', 'LAC', 'LCN', 'LDC', 'LIC', 'LMC', 'LMY', 'LTE', 'MEA', 'MIC', 'MNA', 'NAC', 'OED', 'OSS', 'PRE', 'PSS', 'PST', 'SAS', 'SSA', 'SSF', 'SST', 'TEA', 'TEC', 'TLA', 'TMN', 'TSA', 'TSS', 'UMC', 'WLD']
    


```python
# total_poptotal_pop.drop(temp_list, axis=0, inplace=True)
# urban_pop.drop(temp_list, axis=0, inplace=True)

total_pop = total_pop[~total_pop['Country Code'].isin(temp_list)]
urban_pop = urban_pop[~urban_pop['Country Code'].isin(temp_list)]

print('\t\t\t\t\t\t\t\t\t\t\t\t total_pop\n\t\t\t\t\t\t\t\t\t\t\t\t =========\n')
display(total_pop)
print('\t\t\t\t\t\t\t\t\t\t\t\t urban_pop\n\t\t\t\t\t\t\t\t\t\t\t\t =========\n')
display(urban_pop)
```

    												 total_pop
    												 =========
    
    


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Name</th>
      <th>Country Code</th>
      <th>Indicator Name</th>
      <th>Indicator Code</th>
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>...</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
      <th>2020</th>
      <th>Unnamed: 65</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aruba</td>
      <td>ABW</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>54211.0</td>
      <td>55438.0</td>
      <td>56225.0</td>
      <td>56695.0</td>
      <td>57032.0</td>
      <td>57360.0</td>
      <td>...</td>
      <td>102560.0</td>
      <td>103159.0</td>
      <td>103774.0</td>
      <td>104341.0</td>
      <td>104872.0</td>
      <td>105366.0</td>
      <td>105845.0</td>
      <td>106314.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>8996973.0</td>
      <td>9169410.0</td>
      <td>9351441.0</td>
      <td>9543205.0</td>
      <td>9744781.0</td>
      <td>9956320.0</td>
      <td>...</td>
      <td>31161376.0</td>
      <td>32269589.0</td>
      <td>33370794.0</td>
      <td>34413603.0</td>
      <td>35383128.0</td>
      <td>36296400.0</td>
      <td>37172386.0</td>
      <td>38041754.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>5454933.0</td>
      <td>5531472.0</td>
      <td>5608539.0</td>
      <td>5679458.0</td>
      <td>5735044.0</td>
      <td>5770570.0</td>
      <td>...</td>
      <td>25107931.0</td>
      <td>26015780.0</td>
      <td>26941779.0</td>
      <td>27884381.0</td>
      <td>28842484.0</td>
      <td>29816748.0</td>
      <td>30809762.0</td>
      <td>31825295.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>1608800.0</td>
      <td>1659800.0</td>
      <td>1711319.0</td>
      <td>1762621.0</td>
      <td>1814135.0</td>
      <td>1864791.0</td>
      <td>...</td>
      <td>2900401.0</td>
      <td>2895092.0</td>
      <td>2889104.0</td>
      <td>2880703.0</td>
      <td>2876101.0</td>
      <td>2873457.0</td>
      <td>2866376.0</td>
      <td>2854191.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andorra</td>
      <td>AND</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>13411.0</td>
      <td>14375.0</td>
      <td>15370.0</td>
      <td>16412.0</td>
      <td>17469.0</td>
      <td>18549.0</td>
      <td>...</td>
      <td>82427.0</td>
      <td>80774.0</td>
      <td>79213.0</td>
      <td>78011.0</td>
      <td>77297.0</td>
      <td>77001.0</td>
      <td>77006.0</td>
      <td>77142.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>259</th>
      <td>Kosovo</td>
      <td>XKX</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>947000.0</td>
      <td>966000.0</td>
      <td>994000.0</td>
      <td>1022000.0</td>
      <td>1050000.0</td>
      <td>1078000.0</td>
      <td>...</td>
      <td>1807106.0</td>
      <td>1818117.0</td>
      <td>1812771.0</td>
      <td>1788196.0</td>
      <td>1777557.0</td>
      <td>1791003.0</td>
      <td>1797085.0</td>
      <td>1794248.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>260</th>
      <td>Yemen, Rep.</td>
      <td>YEM</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>5315355.0</td>
      <td>5393036.0</td>
      <td>5473671.0</td>
      <td>5556766.0</td>
      <td>5641597.0</td>
      <td>5727751.0</td>
      <td>...</td>
      <td>24473178.0</td>
      <td>25147109.0</td>
      <td>25823485.0</td>
      <td>26497889.0</td>
      <td>27168210.0</td>
      <td>27834821.0</td>
      <td>28498687.0</td>
      <td>29161922.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>261</th>
      <td>South Africa</td>
      <td>ZAF</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>17099840.0</td>
      <td>17524533.0</td>
      <td>17965725.0</td>
      <td>18423161.0</td>
      <td>18896307.0</td>
      <td>19384841.0</td>
      <td>...</td>
      <td>52834005.0</td>
      <td>53689236.0</td>
      <td>54545991.0</td>
      <td>55386367.0</td>
      <td>56203654.0</td>
      <td>57000451.0</td>
      <td>57779622.0</td>
      <td>58558270.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>262</th>
      <td>Zambia</td>
      <td>ZMB</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>3070776.0</td>
      <td>3164329.0</td>
      <td>3260650.0</td>
      <td>3360104.0</td>
      <td>3463213.0</td>
      <td>3570464.0</td>
      <td>...</td>
      <td>14465121.0</td>
      <td>14926504.0</td>
      <td>15399753.0</td>
      <td>15879361.0</td>
      <td>16363507.0</td>
      <td>16853688.0</td>
      <td>17351822.0</td>
      <td>17861030.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>263</th>
      <td>Zimbabwe</td>
      <td>ZWE</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>3776681.0</td>
      <td>3905034.0</td>
      <td>4039201.0</td>
      <td>4178726.0</td>
      <td>4322861.0</td>
      <td>4471177.0</td>
      <td>...</td>
      <td>13115131.0</td>
      <td>13350356.0</td>
      <td>13586681.0</td>
      <td>13814629.0</td>
      <td>14030390.0</td>
      <td>14236745.0</td>
      <td>14439018.0</td>
      <td>14645468.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>218 rows × 66 columns</p>
</div>


    												 urban_pop
    												 =========
    
    


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Name</th>
      <th>Country Code</th>
      <th>Indicator Name</th>
      <th>Indicator Code</th>
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>...</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
      <th>2020</th>
      <th>Unnamed: 65</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aruba</td>
      <td>ABW</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>27526.0</td>
      <td>28141.0</td>
      <td>28532.0</td>
      <td>28761.0</td>
      <td>28924.0</td>
      <td>29082.0</td>
      <td>...</td>
      <td>44057.0</td>
      <td>44348.0</td>
      <td>44665.0</td>
      <td>44979.0</td>
      <td>45296.0</td>
      <td>45616.0</td>
      <td>45948.0</td>
      <td>46295.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>755836.0</td>
      <td>796272.0</td>
      <td>839385.0</td>
      <td>885228.0</td>
      <td>934135.0</td>
      <td>986074.0</td>
      <td>...</td>
      <td>7528588.0</td>
      <td>7865067.0</td>
      <td>8204877.0</td>
      <td>8535606.0</td>
      <td>8852859.0</td>
      <td>9164841.0</td>
      <td>9477100.0</td>
      <td>9797273.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>569222.0</td>
      <td>597288.0</td>
      <td>628381.0</td>
      <td>660180.0</td>
      <td>691532.0</td>
      <td>721552.0</td>
      <td>...</td>
      <td>15383127.0</td>
      <td>16130304.0</td>
      <td>16900847.0</td>
      <td>17691524.0</td>
      <td>18502165.0</td>
      <td>19332881.0</td>
      <td>20184707.0</td>
      <td>21061025.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>493982.0</td>
      <td>513592.0</td>
      <td>530766.0</td>
      <td>547928.0</td>
      <td>565248.0</td>
      <td>582374.0</td>
      <td>...</td>
      <td>1575788.0</td>
      <td>1603505.0</td>
      <td>1630119.0</td>
      <td>1654503.0</td>
      <td>1680247.0</td>
      <td>1706345.0</td>
      <td>1728969.0</td>
      <td>1747593.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andorra</td>
      <td>AND</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>7839.0</td>
      <td>8766.0</td>
      <td>9754.0</td>
      <td>10811.0</td>
      <td>11915.0</td>
      <td>13067.0</td>
      <td>...</td>
      <td>73056.0</td>
      <td>71515.0</td>
      <td>70057.0</td>
      <td>68919.0</td>
      <td>68213.0</td>
      <td>67876.0</td>
      <td>67813.0</td>
      <td>67873.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>259</th>
      <td>Kosovo</td>
      <td>XKX</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>260</th>
      <td>Yemen, Rep.</td>
      <td>YEM</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>483697.0</td>
      <td>510127.0</td>
      <td>538117.0</td>
      <td>567679.0</td>
      <td>598799.0</td>
      <td>631542.0</td>
      <td>...</td>
      <td>8065870.0</td>
      <td>8439118.0</td>
      <td>8822594.0</td>
      <td>9215171.0</td>
      <td>9615916.0</td>
      <td>10024989.0</td>
      <td>10442489.0</td>
      <td>10869523.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>261</th>
      <td>South Africa</td>
      <td>ZAF</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>7971774.0</td>
      <td>8200255.0</td>
      <td>8427003.0</td>
      <td>8662570.0</td>
      <td>8906585.0</td>
      <td>9158950.0</td>
      <td>...</td>
      <td>33429132.0</td>
      <td>34249974.0</td>
      <td>35079618.0</td>
      <td>35905874.0</td>
      <td>36724030.0</td>
      <td>37534797.0</td>
      <td>38339668.0</td>
      <td>39149717.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>262</th>
      <td>Zambia</td>
      <td>ZMB</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>557192.0</td>
      <td>599672.0</td>
      <td>645120.0</td>
      <td>695945.0</td>
      <td>762426.0</td>
      <td>834489.0</td>
      <td>...</td>
      <td>5837255.0</td>
      <td>6099716.0</td>
      <td>6372726.0</td>
      <td>6654564.0</td>
      <td>6944345.0</td>
      <td>7243041.0</td>
      <td>7551686.0</td>
      <td>7871713.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>263</th>
      <td>Zimbabwe</td>
      <td>ZWE</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>476164.0</td>
      <td>500664.0</td>
      <td>528408.0</td>
      <td>567387.0</td>
      <td>609178.0</td>
      <td>653686.0</td>
      <td>...</td>
      <td>4306222.0</td>
      <td>4359425.0</td>
      <td>4416215.0</td>
      <td>4473868.0</td>
      <td>4531255.0</td>
      <td>4589499.0</td>
      <td>4650663.0</td>
      <td>4717305.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>218 rows × 66 columns</p>
</div>


## 3.2 Data Cleansing for the geometry dataset - country_shapes
There are five country codes available in total and urban pop but missing in country_shapes and are handled as below,

- FRA - The record for France has -99 as ISO_A3 code. We will manually update this to FRA in country_shapes
- NOR - The record for Norway has -99 as ISO_A3 code. We will manually update this to NOR in country_shapes
- XKX - The record for Kosovo has -99 as ISO_A3 code. We will manually update this to XKX in country_shapes
- INX - This is the code for 'unidentified' in the total and urban pop and hence will be ignored (it is not available in country shapes)
- CHI - This country code in total and urban pop represents channel islands. This is missing in country_shapes as channel islands has been added to the United Kingdom. No correction will be made for this. So this will be a known anamoly for our study.


```python
#set(total_pop['Country Code'].unique()) - set(country_shapes.ISO_A3.unique()) #---shows the five country codes missing in country_shapes but present in total and urban pop
```


```python
country_shapes.loc[country_shapes['NAME'] == 'Norway', 'ISO_A3'] = 'NOR'
country_shapes.loc[country_shapes['NAME'] == 'France', 'ISO_A3'] = 'FRA'
country_shapes.loc[country_shapes['NAME'] == 'Kosovo', 'ISO_A3'] = 'XKX'
```

There are 16 other records with -99 ISO_A3.

These are not of much importance since total and urban pop do not contain thesse values.

However, to cleanse this, we will copy ADM0_A3 values of these countries (which are not null) to the ISO_A3 values


```python
# country_shapes[['NAME','ISO_A3', 'ADM0_A3']].query('ISO_A3=="-99"') #---shows the pending 16 records with ISO_A3 = -99
```


```python
country_shapes.loc[country_shapes['ISO_A3'] == '-99', 'ISO_A3'] = country_shapes['ADM0_A3']
```


```python
country_shapes.loc[country_shapes['ISO_A3'] == '-99', 'ISO_A3']
```




    Series([], Name: ISO_A3, dtype: object)




```python
country_shapes = country_shapes[['NAME','ISO_A3','geometry']]
```

# STEP 4: DATA PREPARATION

## 4.1 Make country codes as the index in all datasets
Making country codes as the index in each dataframe will make it easy to perform operations using index. For example to find per capita population we can divide the two dataframes using the index values.


```python
total_pop.set_index('Country Code', inplace=True)
urban_pop.set_index('Country Code', inplace=True)
country_shapes.set_index('ISO_A3', inplace=True)


print('\t\t\t\t\t\t\t\t\t\t\t\t total_pop\n\t\t\t\t\t\t\t\t\t\t\t\t =========\n')
display(total_pop.head())
print('\t\t\t\t\t\t\t\t\t\t\t\t urban_pop\n\t\t\t\t\t\t\t\t\t\t\t\t =========\n')
display(urban_pop.head())
print('\t\t\t\t\t\t\t\t\t\t\t\t country_shapes\n\t\t\t\t\t\t\t\t\t\t\t\t ==============\n')
display(country_shapes.head())
```

    												 total_pop
    												 =========
    
    


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Name</th>
      <th>Indicator Name</th>
      <th>Indicator Code</th>
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>1966</th>
      <th>...</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
      <th>2020</th>
      <th>Unnamed: 65</th>
    </tr>
    <tr>
      <th>Country Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ABW</th>
      <td>Aruba</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>54211.0</td>
      <td>55438.0</td>
      <td>56225.0</td>
      <td>56695.0</td>
      <td>57032.0</td>
      <td>57360.0</td>
      <td>57715.0</td>
      <td>...</td>
      <td>102560.0</td>
      <td>103159.0</td>
      <td>103774.0</td>
      <td>104341.0</td>
      <td>104872.0</td>
      <td>105366.0</td>
      <td>105845.0</td>
      <td>106314.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>AFG</th>
      <td>Afghanistan</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>8996973.0</td>
      <td>9169410.0</td>
      <td>9351441.0</td>
      <td>9543205.0</td>
      <td>9744781.0</td>
      <td>9956320.0</td>
      <td>10174836.0</td>
      <td>...</td>
      <td>31161376.0</td>
      <td>32269589.0</td>
      <td>33370794.0</td>
      <td>34413603.0</td>
      <td>35383128.0</td>
      <td>36296400.0</td>
      <td>37172386.0</td>
      <td>38041754.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>AGO</th>
      <td>Angola</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>5454933.0</td>
      <td>5531472.0</td>
      <td>5608539.0</td>
      <td>5679458.0</td>
      <td>5735044.0</td>
      <td>5770570.0</td>
      <td>5781214.0</td>
      <td>...</td>
      <td>25107931.0</td>
      <td>26015780.0</td>
      <td>26941779.0</td>
      <td>27884381.0</td>
      <td>28842484.0</td>
      <td>29816748.0</td>
      <td>30809762.0</td>
      <td>31825295.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>ALB</th>
      <td>Albania</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>1608800.0</td>
      <td>1659800.0</td>
      <td>1711319.0</td>
      <td>1762621.0</td>
      <td>1814135.0</td>
      <td>1864791.0</td>
      <td>1914573.0</td>
      <td>...</td>
      <td>2900401.0</td>
      <td>2895092.0</td>
      <td>2889104.0</td>
      <td>2880703.0</td>
      <td>2876101.0</td>
      <td>2873457.0</td>
      <td>2866376.0</td>
      <td>2854191.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>AND</th>
      <td>Andorra</td>
      <td>Population, total</td>
      <td>SP.POP.TOTL</td>
      <td>13411.0</td>
      <td>14375.0</td>
      <td>15370.0</td>
      <td>16412.0</td>
      <td>17469.0</td>
      <td>18549.0</td>
      <td>19647.0</td>
      <td>...</td>
      <td>82427.0</td>
      <td>80774.0</td>
      <td>79213.0</td>
      <td>78011.0</td>
      <td>77297.0</td>
      <td>77001.0</td>
      <td>77006.0</td>
      <td>77142.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 65 columns</p>
</div>


    												 urban_pop
    												 =========
    
    


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Name</th>
      <th>Indicator Name</th>
      <th>Indicator Code</th>
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>1966</th>
      <th>...</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
      <th>2020</th>
      <th>Unnamed: 65</th>
    </tr>
    <tr>
      <th>Country Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ABW</th>
      <td>Aruba</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>27526.0</td>
      <td>28141.0</td>
      <td>28532.0</td>
      <td>28761.0</td>
      <td>28924.0</td>
      <td>29082.0</td>
      <td>29253.0</td>
      <td>...</td>
      <td>44057.0</td>
      <td>44348.0</td>
      <td>44665.0</td>
      <td>44979.0</td>
      <td>45296.0</td>
      <td>45616.0</td>
      <td>45948.0</td>
      <td>46295.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>AFG</th>
      <td>Afghanistan</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>755836.0</td>
      <td>796272.0</td>
      <td>839385.0</td>
      <td>885228.0</td>
      <td>934135.0</td>
      <td>986074.0</td>
      <td>1041191.0</td>
      <td>...</td>
      <td>7528588.0</td>
      <td>7865067.0</td>
      <td>8204877.0</td>
      <td>8535606.0</td>
      <td>8852859.0</td>
      <td>9164841.0</td>
      <td>9477100.0</td>
      <td>9797273.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>AGO</th>
      <td>Angola</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>569222.0</td>
      <td>597288.0</td>
      <td>628381.0</td>
      <td>660180.0</td>
      <td>691532.0</td>
      <td>721552.0</td>
      <td>749534.0</td>
      <td>...</td>
      <td>15383127.0</td>
      <td>16130304.0</td>
      <td>16900847.0</td>
      <td>17691524.0</td>
      <td>18502165.0</td>
      <td>19332881.0</td>
      <td>20184707.0</td>
      <td>21061025.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>ALB</th>
      <td>Albania</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>493982.0</td>
      <td>513592.0</td>
      <td>530766.0</td>
      <td>547928.0</td>
      <td>565248.0</td>
      <td>582374.0</td>
      <td>599300.0</td>
      <td>...</td>
      <td>1575788.0</td>
      <td>1603505.0</td>
      <td>1630119.0</td>
      <td>1654503.0</td>
      <td>1680247.0</td>
      <td>1706345.0</td>
      <td>1728969.0</td>
      <td>1747593.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>AND</th>
      <td>Andorra</td>
      <td>Urban population</td>
      <td>SP.URB.TOTL</td>
      <td>7839.0</td>
      <td>8766.0</td>
      <td>9754.0</td>
      <td>10811.0</td>
      <td>11915.0</td>
      <td>13067.0</td>
      <td>14262.0</td>
      <td>...</td>
      <td>73056.0</td>
      <td>71515.0</td>
      <td>70057.0</td>
      <td>68919.0</td>
      <td>68213.0</td>
      <td>67876.0</td>
      <td>67813.0</td>
      <td>67873.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 65 columns</p>
</div>


    												 country_shapes
    												 ==============
    
    


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>geometry</th>
    </tr>
    <tr>
      <th>ISO_A3</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>IDN</th>
      <td>Indonesia</td>
      <td>MULTIPOLYGON (((117.70361 4.16341, 117.70361 4...</td>
    </tr>
    <tr>
      <th>MYS</th>
      <td>Malaysia</td>
      <td>MULTIPOLYGON (((117.70361 4.16341, 117.69711 4...</td>
    </tr>
    <tr>
      <th>CHL</th>
      <td>Chile</td>
      <td>MULTIPOLYGON (((-69.51009 -17.50659, -69.50611...</td>
    </tr>
    <tr>
      <th>BOL</th>
      <td>Bolivia</td>
      <td>POLYGON ((-69.51009 -17.50659, -69.51009 -17.5...</td>
    </tr>
    <tr>
      <th>PER</th>
      <td>Peru</td>
      <td>MULTIPOLYGON (((-69.51009 -17.50659, -69.63832...</td>
    </tr>
  </tbody>
</table>
</div>


## 4.2 Prepare per capita urban pop dataset
compute per capital urban population for the years 1990, 2000, 2010

Since the index of all datasets are the Country COdes, pandas division can be used to calculate per capita urban population from total_pop and urban_pop


```python
per_capita_pop = urban_pop.loc[:,'1990':'2010'].div(total_pop.loc[:,'1990':'2010'])
per_capita_pop
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1990</th>
      <th>1991</th>
      <th>1992</th>
      <th>1993</th>
      <th>1994</th>
      <th>1995</th>
      <th>1996</th>
      <th>1997</th>
      <th>1998</th>
      <th>1999</th>
      <th>...</th>
      <th>2001</th>
      <th>2002</th>
      <th>2003</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
    </tr>
    <tr>
      <th>Country Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ABW</th>
      <td>0.503194</td>
      <td>0.503033</td>
      <td>0.499978</td>
      <td>0.495876</td>
      <td>0.491773</td>
      <td>0.487675</td>
      <td>0.483558</td>
      <td>0.479456</td>
      <td>0.475360</td>
      <td>0.471266</td>
      <td>...</td>
      <td>0.463390</td>
      <td>0.459723</td>
      <td>0.456064</td>
      <td>0.452404</td>
      <td>0.448751</td>
      <td>0.445108</td>
      <td>0.441465</td>
      <td>0.437834</td>
      <td>0.434212</td>
      <td>0.430593</td>
    </tr>
    <tr>
      <th>AFG</th>
      <td>0.211770</td>
      <td>0.212660</td>
      <td>0.213550</td>
      <td>0.214440</td>
      <td>0.215340</td>
      <td>0.216240</td>
      <td>0.217140</td>
      <td>0.218050</td>
      <td>0.218950</td>
      <td>0.219860</td>
      <td>...</td>
      <td>0.221690</td>
      <td>0.222610</td>
      <td>0.223530</td>
      <td>0.225000</td>
      <td>0.227030</td>
      <td>0.229070</td>
      <td>0.231130</td>
      <td>0.233200</td>
      <td>0.235280</td>
      <td>0.237370</td>
    </tr>
    <tr>
      <th>AGO</th>
      <td>0.371440</td>
      <td>0.385800</td>
      <td>0.400390</td>
      <td>0.415110</td>
      <td>0.430000</td>
      <td>0.441690</td>
      <td>0.453460</td>
      <td>0.465250</td>
      <td>0.477100</td>
      <td>0.488970</td>
      <td>...</td>
      <td>0.512740</td>
      <td>0.524610</td>
      <td>0.536450</td>
      <td>0.548270</td>
      <td>0.560000</td>
      <td>0.567640</td>
      <td>0.575240</td>
      <td>0.582820</td>
      <td>0.590340</td>
      <td>0.597830</td>
    </tr>
    <tr>
      <th>ALB</th>
      <td>0.364280</td>
      <td>0.367000</td>
      <td>0.372490</td>
      <td>0.377990</td>
      <td>0.383540</td>
      <td>0.389110</td>
      <td>0.394730</td>
      <td>0.400350</td>
      <td>0.406010</td>
      <td>0.411690</td>
      <td>...</td>
      <td>0.424350</td>
      <td>0.435010</td>
      <td>0.445730</td>
      <td>0.456510</td>
      <td>0.467310</td>
      <td>0.478150</td>
      <td>0.489020</td>
      <td>0.499910</td>
      <td>0.510760</td>
      <td>0.521630</td>
    </tr>
    <tr>
      <th>AND</th>
      <td>0.947128</td>
      <td>0.945298</td>
      <td>0.943248</td>
      <td>0.941103</td>
      <td>0.938893</td>
      <td>0.936617</td>
      <td>0.934245</td>
      <td>0.931802</td>
      <td>0.929266</td>
      <td>0.926658</td>
      <td>...</td>
      <td>0.920554</td>
      <td>0.916416</td>
      <td>0.912069</td>
      <td>0.907507</td>
      <td>0.902849</td>
      <td>0.898065</td>
      <td>0.893075</td>
      <td>0.890046</td>
      <td>0.889123</td>
      <td>0.888193</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>XKX</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>YEM</th>
      <td>0.209310</td>
      <td>0.214890</td>
      <td>0.220580</td>
      <td>0.226360</td>
      <td>0.232250</td>
      <td>0.237600</td>
      <td>0.242490</td>
      <td>0.247430</td>
      <td>0.252440</td>
      <td>0.257520</td>
      <td>...</td>
      <td>0.267870</td>
      <td>0.273150</td>
      <td>0.278490</td>
      <td>0.283900</td>
      <td>0.289360</td>
      <td>0.294900</td>
      <td>0.300510</td>
      <td>0.306190</td>
      <td>0.311940</td>
      <td>0.317760</td>
    </tr>
    <tr>
      <th>ZAF</th>
      <td>0.520370</td>
      <td>0.525540</td>
      <td>0.530380</td>
      <td>0.535210</td>
      <td>0.540040</td>
      <td>0.544860</td>
      <td>0.549670</td>
      <td>0.554490</td>
      <td>0.559300</td>
      <td>0.564110</td>
      <td>...</td>
      <td>0.573680</td>
      <td>0.578980</td>
      <td>0.584460</td>
      <td>0.589930</td>
      <td>0.595360</td>
      <td>0.600770</td>
      <td>0.606160</td>
      <td>0.611540</td>
      <td>0.616870</td>
      <td>0.622180</td>
    </tr>
    <tr>
      <th>ZMB</th>
      <td>0.394070</td>
      <td>0.389890</td>
      <td>0.385140</td>
      <td>0.380420</td>
      <td>0.375720</td>
      <td>0.371040</td>
      <td>0.366380</td>
      <td>0.361760</td>
      <td>0.357160</td>
      <td>0.352580</td>
      <td>...</td>
      <td>0.350020</td>
      <td>0.354750</td>
      <td>0.359510</td>
      <td>0.364300</td>
      <td>0.369110</td>
      <td>0.373950</td>
      <td>0.378810</td>
      <td>0.383710</td>
      <td>0.388610</td>
      <td>0.393550</td>
    </tr>
    <tr>
      <th>ZWE</th>
      <td>0.289880</td>
      <td>0.297380</td>
      <td>0.304990</td>
      <td>0.309400</td>
      <td>0.313350</td>
      <td>0.317320</td>
      <td>0.321320</td>
      <td>0.325340</td>
      <td>0.329390</td>
      <td>0.333470</td>
      <td>...</td>
      <td>0.341700</td>
      <td>0.345850</td>
      <td>0.344790</td>
      <td>0.342940</td>
      <td>0.341100</td>
      <td>0.339260</td>
      <td>0.337430</td>
      <td>0.335600</td>
      <td>0.333780</td>
      <td>0.331960</td>
    </tr>
  </tbody>
</table>
<p>218 rows × 21 columns</p>
</div>



## 4.3 Check fr bad values

__There are no infinite values as seen below__

Infinite values could occur when division takees place by a zero


```python
per_capita_pop[(per_capita_pop == np.inf).any(axis=1)]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1990</th>
      <th>1991</th>
      <th>1992</th>
      <th>1993</th>
      <th>1994</th>
      <th>1995</th>
      <th>1996</th>
      <th>1997</th>
      <th>1998</th>
      <th>1999</th>
      <th>...</th>
      <th>2001</th>
      <th>2002</th>
      <th>2003</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
    </tr>
    <tr>
      <th>Country Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
<p>0 rows × 21 columns</p>
</div>



__There are some Null values as seen below__

We do not have to update them because geopandas will simply ignore them when plotting the map. 

Converting them to zero might give a wrong output as 0 might actually represent countries where urban population is very low. Since we do not know the data for these countries, we should not be assuming it.


```python
per_capita_pop.loc[per_capita_pop.isna().any(axis=1)]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1990</th>
      <th>1991</th>
      <th>1992</th>
      <th>1993</th>
      <th>1994</th>
      <th>1995</th>
      <th>1996</th>
      <th>1997</th>
      <th>1998</th>
      <th>1999</th>
      <th>...</th>
      <th>2001</th>
      <th>2002</th>
      <th>2003</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
    </tr>
    <tr>
      <th>Country Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>INX</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>KWT</th>
      <td>0.97974</td>
      <td>0.97988</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.98096</td>
      <td>0.98325</td>
      <td>0.98527</td>
      <td>0.98705</td>
      <td>0.98862</td>
      <td>...</td>
      <td>0.99908</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>MAF</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>SXM</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.00000</td>
      <td>1.00000</td>
      <td>...</td>
      <td>1.00000</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>XKX</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



## 4.4 calculate representativepoints for each geometry which helps with labelling plots


```python
pd.options.mode.chained_assignment = None  # default='warn'

country_shapes['rep_coords'] = country_shapes['geometry'].representative_point()
country_shapes
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>geometry</th>
      <th>rep_coords</th>
    </tr>
    <tr>
      <th>ISO_A3</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>IDN</th>
      <td>Indonesia</td>
      <td>MULTIPOLYGON (((117.70361 4.16341, 117.70361 4...</td>
      <td>POINT (113.32523 0.10491)</td>
    </tr>
    <tr>
      <th>MYS</th>
      <td>Malaysia</td>
      <td>MULTIPOLYGON (((117.70361 4.16341, 117.69711 4...</td>
      <td>POINT (102.11153 3.98945)</td>
    </tr>
    <tr>
      <th>CHL</th>
      <td>Chile</td>
      <td>MULTIPOLYGON (((-69.51009 -17.50659, -69.50611...</td>
      <td>POINT (-71.49640 -35.71034)</td>
    </tr>
    <tr>
      <th>BOL</th>
      <td>Bolivia</td>
      <td>POLYGON ((-69.51009 -17.50659, -69.51009 -17.5...</td>
      <td>POINT (-64.28580 -16.28784)</td>
    </tr>
    <tr>
      <th>PER</th>
      <td>Peru</td>
      <td>MULTIPOLYGON (((-69.51009 -17.50659, -69.63832...</td>
      <td>POINT (-75.76765 -9.18342)</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>MAC</th>
      <td>Macao</td>
      <td>MULTIPOLYGON (((113.55860 22.16303, 113.56943 ...</td>
      <td>POINT (113.55943 22.13618)</td>
    </tr>
    <tr>
      <th>ATC</th>
      <td>Ashmore and Cartier Is.</td>
      <td>POLYGON ((123.59702 -12.42832, 123.59775 -12.4...</td>
      <td>POINT (123.58635 -12.43255)</td>
    </tr>
    <tr>
      <th>BJN</th>
      <td>Bajo Nuevo Bank</td>
      <td>POLYGON ((-79.98929 15.79495, -79.98782 15.796...</td>
      <td>POINT (-79.98794 15.79558)</td>
    </tr>
    <tr>
      <th>SER</th>
      <td>Serranilla Bank</td>
      <td>POLYGON ((-78.63707 15.86209, -78.64041 15.864...</td>
      <td>POINT (-78.63779 15.86565)</td>
    </tr>
    <tr>
      <th>SCR</th>
      <td>Scarborough Reef</td>
      <td>POLYGON ((117.75389 15.15437, 117.75569 15.151...</td>
      <td>POINT (117.75383 15.15313)</td>
    </tr>
  </tbody>
</table>
<p>255 rows × 3 columns</p>
</div>



## 4.5: Join per_capita_pop to country_shapes to get the geometry data for each country code


```python
geo_combined_pop = country_shapes.merge(per_capita_pop, left_index=True, right_index=True)
geo_combined_pop
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>geometry</th>
      <th>rep_coords</th>
      <th>1990</th>
      <th>1991</th>
      <th>1992</th>
      <th>1993</th>
      <th>1994</th>
      <th>1995</th>
      <th>1996</th>
      <th>...</th>
      <th>2001</th>
      <th>2002</th>
      <th>2003</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>IDN</th>
      <td>Indonesia</td>
      <td>MULTIPOLYGON (((117.70361 4.16341, 117.70361 4...</td>
      <td>POINT (113.32523 0.10491)</td>
      <td>0.305840</td>
      <td>0.316130</td>
      <td>0.327030</td>
      <td>0.338080</td>
      <td>0.349330</td>
      <td>0.360760</td>
      <td>0.372350</td>
      <td>...</td>
      <td>0.427830</td>
      <td>0.435680</td>
      <td>0.443560</td>
      <td>0.451490</td>
      <td>0.459420</td>
      <td>0.467380</td>
      <td>0.475350</td>
      <td>0.483350</td>
      <td>0.491340</td>
      <td>0.499140</td>
    </tr>
    <tr>
      <th>MYS</th>
      <td>Malaysia</td>
      <td>MULTIPOLYGON (((117.70361 4.16341, 117.69711 4...</td>
      <td>POINT (102.11153 3.98945)</td>
      <td>0.497940</td>
      <td>0.505760</td>
      <td>0.518140</td>
      <td>0.531090</td>
      <td>0.544020</td>
      <td>0.556880</td>
      <td>0.569690</td>
      <td>...</td>
      <td>0.629220</td>
      <td>0.638560</td>
      <td>0.647800</td>
      <td>0.656940</td>
      <td>0.665940</td>
      <td>0.674830</td>
      <td>0.683600</td>
      <td>0.692250</td>
      <td>0.700750</td>
      <td>0.709120</td>
    </tr>
    <tr>
      <th>CHL</th>
      <td>Chile</td>
      <td>MULTIPOLYGON (((-69.51009 -17.50659, -69.50611...</td>
      <td>POINT (-71.49640 -35.71034)</td>
      <td>0.832710</td>
      <td>0.833980</td>
      <td>0.835640</td>
      <td>0.838960</td>
      <td>0.842230</td>
      <td>0.845450</td>
      <td>0.848610</td>
      <td>...</td>
      <td>0.863630</td>
      <td>0.866060</td>
      <td>0.866650</td>
      <td>0.867250</td>
      <td>0.867830</td>
      <td>0.868420</td>
      <td>0.869000</td>
      <td>0.869590</td>
      <td>0.870170</td>
      <td>0.870740</td>
    </tr>
    <tr>
      <th>BOL</th>
      <td>Bolivia</td>
      <td>POLYGON ((-69.51009 -17.50659, -69.51009 -17.5...</td>
      <td>POINT (-64.28580 -16.28784)</td>
      <td>0.555770</td>
      <td>0.565790</td>
      <td>0.575410</td>
      <td>0.580790</td>
      <td>0.586150</td>
      <td>0.591500</td>
      <td>0.596820</td>
      <td>...</td>
      <td>0.623060</td>
      <td>0.627830</td>
      <td>0.632480</td>
      <td>0.637110</td>
      <td>0.641700</td>
      <td>0.646280</td>
      <td>0.650820</td>
      <td>0.655350</td>
      <td>0.659840</td>
      <td>0.664300</td>
    </tr>
    <tr>
      <th>PER</th>
      <td>Peru</td>
      <td>MULTIPOLYGON (((-69.51009 -17.50659, -69.63832...</td>
      <td>POINT (-75.76765 -9.18342)</td>
      <td>0.689010</td>
      <td>0.693000</td>
      <td>0.696970</td>
      <td>0.700890</td>
      <td>0.705210</td>
      <td>0.709510</td>
      <td>0.713770</td>
      <td>...</td>
      <td>0.734480</td>
      <td>0.738500</td>
      <td>0.742490</td>
      <td>0.746440</td>
      <td>0.750340</td>
      <td>0.754210</td>
      <td>0.758030</td>
      <td>0.760520</td>
      <td>0.762410</td>
      <td>0.764300</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>PLW</th>
      <td>Palau</td>
      <td>MULTIPOLYGON (((134.27149 7.07453, 134.27931 7...</td>
      <td>POINT (134.58089 7.54428)</td>
      <td>0.695909</td>
      <td>0.699631</td>
      <td>0.703393</td>
      <td>0.707060</td>
      <td>0.710714</td>
      <td>0.714286</td>
      <td>0.713068</td>
      <td>...</td>
      <td>0.700918</td>
      <td>0.698477</td>
      <td>0.695997</td>
      <td>0.703830</td>
      <td>0.711541</td>
      <td>0.719089</td>
      <td>0.726537</td>
      <td>0.733907</td>
      <td>0.741099</td>
      <td>0.748148</td>
    </tr>
    <tr>
      <th>GUM</th>
      <td>Guam</td>
      <td>POLYGON ((144.88640 13.64020, 144.89666 13.617...</td>
      <td>POINT (144.76055 13.45962)</td>
      <td>0.907957</td>
      <td>0.910608</td>
      <td>0.913199</td>
      <td>0.915712</td>
      <td>0.918159</td>
      <td>0.920549</td>
      <td>0.922869</td>
      <td>...</td>
      <td>0.932328</td>
      <td>0.933342</td>
      <td>0.934351</td>
      <td>0.935339</td>
      <td>0.936308</td>
      <td>0.937270</td>
      <td>0.938217</td>
      <td>0.939158</td>
      <td>0.940079</td>
      <td>0.940989</td>
    </tr>
    <tr>
      <th>MNP</th>
      <td>N. Mariana Is.</td>
      <td>MULTIPOLYGON (((145.20574 14.18138, 145.25245 ...</td>
      <td>POINT (145.21334 14.15843)</td>
      <td>0.897316</td>
      <td>0.896970</td>
      <td>0.896613</td>
      <td>0.896280</td>
      <td>0.895931</td>
      <td>0.895574</td>
      <td>0.896551</td>
      <td>...</td>
      <td>0.902351</td>
      <td>0.903167</td>
      <td>0.903981</td>
      <td>0.904779</td>
      <td>0.905575</td>
      <td>0.906374</td>
      <td>0.907152</td>
      <td>0.907954</td>
      <td>0.908715</td>
      <td>0.909488</td>
    </tr>
    <tr>
      <th>BHR</th>
      <td>Bahrain</td>
      <td>POLYGON ((50.55161 26.19424, 50.59474 26.16031...</td>
      <td>POINT (50.54479 26.00208)</td>
      <td>0.881401</td>
      <td>0.883290</td>
      <td>0.883981</td>
      <td>0.883950</td>
      <td>0.883919</td>
      <td>0.883881</td>
      <td>0.883850</td>
      <td>...</td>
      <td>0.883691</td>
      <td>0.883731</td>
      <td>0.883829</td>
      <td>0.883990</td>
      <td>0.884220</td>
      <td>0.884520</td>
      <td>0.884880</td>
      <td>0.885300</td>
      <td>0.885790</td>
      <td>0.886340</td>
    </tr>
    <tr>
      <th>MAC</th>
      <td>Macao</td>
      <td>MULTIPOLYGON (((113.55860 22.16303, 113.56943 ...</td>
      <td>POINT (113.55943 22.13618)</td>
      <td>0.997629</td>
      <td>0.998049</td>
      <td>0.998391</td>
      <td>0.998671</td>
      <td>0.998909</td>
      <td>0.999100</td>
      <td>0.999799</td>
      <td>...</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
<p>216 rows × 24 columns</p>
</div>



## 4.6 Mean per capita urban population between 1990 and 2010
Unlike plotting, calculating mean calls for ignoring Nan values.

This is beacuse the mean value might fall because of the Nan values. So we use the skipna=True parameter when calculating means)


```python
mean_per_capita_urban_pop_1990_2010 = geo_combined_pop.loc[:,'1990':'2010'].mean(skipna=True).to_frame(name='mean_per_capita_urban_pop')
mean_per_capita_urban_pop_1990_2010
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mean_per_capita_urban_pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1990</th>
      <td>0.526584</td>
    </tr>
    <tr>
      <th>1991</th>
      <td>0.530099</td>
    </tr>
    <tr>
      <th>1992</th>
      <td>0.530883</td>
    </tr>
    <tr>
      <th>1993</th>
      <td>0.533658</td>
    </tr>
    <tr>
      <th>1994</th>
      <td>0.536311</td>
    </tr>
    <tr>
      <th>1995</th>
      <td>0.540918</td>
    </tr>
    <tr>
      <th>1996</th>
      <td>0.543449</td>
    </tr>
    <tr>
      <th>1997</th>
      <td>0.546106</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>0.550824</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>0.553394</td>
    </tr>
    <tr>
      <th>2000</th>
      <td>0.556082</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>0.559020</td>
    </tr>
    <tr>
      <th>2002</th>
      <td>0.561938</td>
    </tr>
    <tr>
      <th>2003</th>
      <td>0.564868</td>
    </tr>
    <tr>
      <th>2004</th>
      <td>0.567838</td>
    </tr>
    <tr>
      <th>2005</th>
      <td>0.570904</td>
    </tr>
    <tr>
      <th>2006</th>
      <td>0.573930</td>
    </tr>
    <tr>
      <th>2007</th>
      <td>0.576786</td>
    </tr>
    <tr>
      <th>2008</th>
      <td>0.579890</td>
    </tr>
    <tr>
      <th>2009</th>
      <td>0.582983</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>0.586068</td>
    </tr>
  </tbody>
</table>
</div>



## 4.7 Mean total population between 1990 and 2010


```python
mean_total_pop_1990_2010 = total_pop.loc[:,'1990':'2010'].mean(skipna=True).to_frame(name='mean_total_pop')
mean_total_pop_1990_2010
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mean_total_pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1990</th>
      <td>2.435092e+07</td>
    </tr>
    <tr>
      <th>1991</th>
      <td>2.475723e+07</td>
    </tr>
    <tr>
      <th>1992</th>
      <td>2.526356e+07</td>
    </tr>
    <tr>
      <th>1993</th>
      <td>2.565878e+07</td>
    </tr>
    <tr>
      <th>1994</th>
      <td>2.604981e+07</td>
    </tr>
    <tr>
      <th>1995</th>
      <td>2.632144e+07</td>
    </tr>
    <tr>
      <th>1996</th>
      <td>2.670463e+07</td>
    </tr>
    <tr>
      <th>1997</th>
      <td>2.708636e+07</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>2.733720e+07</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>2.770730e+07</td>
    </tr>
    <tr>
      <th>2000</th>
      <td>2.807447e+07</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>2.843932e+07</td>
    </tr>
    <tr>
      <th>2002</th>
      <td>2.880321e+07</td>
    </tr>
    <tr>
      <th>2003</th>
      <td>2.916741e+07</td>
    </tr>
    <tr>
      <th>2004</th>
      <td>2.953415e+07</td>
    </tr>
    <tr>
      <th>2005</th>
      <td>2.990334e+07</td>
    </tr>
    <tr>
      <th>2006</th>
      <td>3.027613e+07</td>
    </tr>
    <tr>
      <th>2007</th>
      <td>3.065116e+07</td>
    </tr>
    <tr>
      <th>2008</th>
      <td>3.103191e+07</td>
    </tr>
    <tr>
      <th>2009</th>
      <td>3.141247e+07</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>3.179140e+07</td>
    </tr>
  </tbody>
</table>
</div>



## 4.8 Combined means dataset
Create a single dataframe with both mean per capita urban population and mean total population for the entire world from 1990 to 2010


```python
_1990_2010_combined_means = pd.merge(mean_per_capita_urban_pop_1990_2010, mean_total_pop_1990_2010, left_index=True, right_index=True)
_1990_2010_combined_means
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mean_per_capita_urban_pop</th>
      <th>mean_total_pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1990</th>
      <td>0.526584</td>
      <td>2.435092e+07</td>
    </tr>
    <tr>
      <th>1991</th>
      <td>0.530099</td>
      <td>2.475723e+07</td>
    </tr>
    <tr>
      <th>1992</th>
      <td>0.530883</td>
      <td>2.526356e+07</td>
    </tr>
    <tr>
      <th>1993</th>
      <td>0.533658</td>
      <td>2.565878e+07</td>
    </tr>
    <tr>
      <th>1994</th>
      <td>0.536311</td>
      <td>2.604981e+07</td>
    </tr>
    <tr>
      <th>1995</th>
      <td>0.540918</td>
      <td>2.632144e+07</td>
    </tr>
    <tr>
      <th>1996</th>
      <td>0.543449</td>
      <td>2.670463e+07</td>
    </tr>
    <tr>
      <th>1997</th>
      <td>0.546106</td>
      <td>2.708636e+07</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>0.550824</td>
      <td>2.733720e+07</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>0.553394</td>
      <td>2.770730e+07</td>
    </tr>
    <tr>
      <th>2000</th>
      <td>0.556082</td>
      <td>2.807447e+07</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>0.559020</td>
      <td>2.843932e+07</td>
    </tr>
    <tr>
      <th>2002</th>
      <td>0.561938</td>
      <td>2.880321e+07</td>
    </tr>
    <tr>
      <th>2003</th>
      <td>0.564868</td>
      <td>2.916741e+07</td>
    </tr>
    <tr>
      <th>2004</th>
      <td>0.567838</td>
      <td>2.953415e+07</td>
    </tr>
    <tr>
      <th>2005</th>
      <td>0.570904</td>
      <td>2.990334e+07</td>
    </tr>
    <tr>
      <th>2006</th>
      <td>0.573930</td>
      <td>3.027613e+07</td>
    </tr>
    <tr>
      <th>2007</th>
      <td>0.576786</td>
      <td>3.065116e+07</td>
    </tr>
    <tr>
      <th>2008</th>
      <td>0.579890</td>
      <td>3.103191e+07</td>
    </tr>
    <tr>
      <th>2009</th>
      <td>0.582983</td>
      <td>3.141247e+07</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>0.586068</td>
      <td>3.179140e+07</td>
    </tr>
  </tbody>
</table>
</div>



# STEP 5: CHOROPLETH PLOTS

## 5.1 Urban Population per Capita for 1990


```python
from mpl_toolkits.axes_grid1 import make_axes_locatable

fig, ax = plt.subplots(1, 1, figsize=(20, 16));

ax.set_title("Urban Population per Capita for the year 1990", fontsize=20, pad=25);

geo_combined_pop.plot(column='1990', 
                      cmap='YlGnBu',
                      ax=ax, 
                      legend=True,
                      legend_kwds={'orientation': "horizontal", 
                                   'pad': 0.03, 
                                   'aspect': 50, 
                                   'ticks': [0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1]
                                  }
                     );
```


    
![png](https://github.com/Blindusername001/Geospatial-Plotting-in-python-with-geopandas/blob/main/Images/output_47_0.png)
    


## 5.2 Urban Population per Capita for 2000


```python
from mpl_toolkits.axes_grid1 import make_axes_locatable

fig, ax = plt.subplots(1, 1, figsize=(20, 16));

ax.set_title("Urban Population per Capita for the year 2000", fontsize=20, pad=25);

geo_combined_pop.plot(column='2000', 
                      cmap='YlGnBu',
                      ax=ax, 
                      legend=True,
                      legend_kwds={'orientation': "horizontal", 
                                   'pad': 0.05, 
                                   'aspect': 50, 
                                   'ticks': [0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1]
                                  }
                     );

plt.show()
```


    
![png](https://github.com/Blindusername001/Geospatial-Plotting-in-python-with-geopandas/blob/main/Images/output_49_0.png)
    


## 5.3 Urban Population per Capita for 2010


```python
from mpl_toolkits.axes_grid1 import make_axes_locatable

fig, ax = plt.subplots(1, 1, figsize=(20, 16));

ax.set_title("Urban Population per Capita for the year 2010", fontsize=20, pad=25);


geo_combined_pop.plot(column='2010', 
                      cmap='YlGnBu',
                      ax=ax, 
                      legend=True,
                      legend_kwds={'orientation': "horizontal", 
                                   'pad': 0.05, 
                                   'aspect': 50, 
                                   'ticks': [0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1]
                                  }
                     );
```


    
![png](https://github.com/Blindusername001/Geospatial-Plotting-in-python-with-geopandas/blob/main/Images/output_51_0.png)
    


## 5.4 2010 Urban Population per Capita for countries with total population greater than 290 million


```python
grt_290_mil = total_pop.index[total_pop['2010'] > 290000000].tolist()
```


```python
grt_290_mil_df = geo_combined_pop[geo_combined_pop.index.isin(grt_290_mil)]

from mpl_toolkits.axes_grid1 import make_axes_locatable

fig, ax = plt.subplots(1, 1, figsize=(25, 16));

ax.set_title("Urban Population per Capita for countries with over 290 million total population", fontsize=20, pad=25)
ax.set_xlabel("Longitude")
ax.set_ylabel("Latitude")

grt_290_mil_df.plot(column='2010', 
                      cmap='coolwarm',
                      ax=ax, 
                      legend=True,
                      legend_kwds={'orientation': "horizontal", 
                                   'pad': 0.05, 
                                   'aspect': 50, 
                                   'ticks': [0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1]
                                  }
                     );

for x, y, cntry, val in zip(grt_290_mil_df.rep_coords.x, grt_290_mil_df.rep_coords.y, grt_290_mil_df['NAME'], grt_290_mil_df['2010']):
    ax.annotate(f'{cntry} / {round(val,2)}', xy=(x, y), xytext=(0, -10), textcoords="offset points")
```


    
![png](https://github.com/Blindusername001/Geospatial-Plotting-in-python-with-geopandas/blob/main/Images/output_54_0.png)
    


<div class="alert alert-block alert-info">
        <b> Interpretation </b>
        
There are only three countires with a total population greater than 290 million - China, India and United States of America
    
Among them United States of America has the highest per capita uprban population in 2010 (81%)
    
China has about 49% of population in urban areas while India has around 31% of population in it's urban areas,
</div>

## 5.5 2010 Urban Population per Capita for countries with total population less than 69 million


```python
less_69_mil = total_pop.index[total_pop['2010'] < 69000000].tolist()
```

<span style="color:red">We are not labelling this plot as it will be over crowded</span>


```python
less_69_mil_df = geo_combined_pop[geo_combined_pop.index.isin(less_69_mil)]

from mpl_toolkits.axes_grid1 import make_axes_locatable

fig, ax = plt.subplots(1, 1, figsize=(25, 16))

ax.set_title("Urban Population per Capita for countries with less than 69 million total population", fontsize=20, pad=25)
ax.set_xlabel("Longitude")
ax.set_ylabel("Latitude")

less_69_mil_df.plot(column='2010', 
                      cmap='coolwarm',
                      ax=ax, 
                      legend=True,
                      legend_kwds={'orientation': "horizontal", 
                                   'pad': 0.05, 
                                   'aspect': 50, 
                                   'ticks': [0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1]
                                  }
                     );

# for x, y, label in zip(less_69_mil_df.rep_coords.x, less_69_mil_df.rep_coords.y, less_69_mil_df['NAME']):
#     ax.annotate(label, xy=(x, y), xytext=(0, 0), textcoords="offset points", fontsize=10)

# for x, y, val in zip(less_69_mil_df.rep_coords.x, less_69_mil_df.rep_coords.y, less_69_mil_df['2010']):
#     ax.annotate(f'{round(val,2)}', xy=(x, y), xytext=(0, -10), textcoords="offset points")
```


    
![png](https://github.com/Blindusername001/Geospatial-Plotting-in-python-with-geopandas/blob/main/Images/output_59_0.png)
    


## 5.6 Mean per capita world urban population from 1990 to 2010


```python
fig, ax = plt.subplots(1, 1, figsize=(20, 5));
ax.set_title("Mean world per capita urban population from 1990 to 2010", fontsize=20, pad=25)
ax.set_xlabel("Year")
ax.set_ylabel("Mean per capita urban population")

sns.lineplot(x=_1990_2010_combined_means.index, y="mean_per_capita_urban_pop", data=_1990_2010_combined_means)
plt.grid()
plt.show()
```


    
![png](https://github.com/Blindusername001/Geospatial-Plotting-in-python-with-geopandas/blob/main/Images/output_61_0.png)
    


<div class="alert alert-block alert-info">
        <b> Interpretation </b>
    
At a high level, it can be seen that the mean per capita urban population has increased over the entire timeframe between 1990 to 2010.
    
Starting at 1990, about 53% of the world's population was in urban areas and by 2010, it has increased to about 59%.
    
Using the gridlines, it can be seen that there is roughly a 1% increase approximately every three years.
    
</div>

## 5.7 Correlation plot between mean world population and mean per capita world urban population from 1990 to 2010


```python
correlation_matrix = _1990_2010_combined_means.corr(method='pearson')
correlation_matrix
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mean_per_capita_urban_pop</th>
      <th>mean_total_pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>mean_per_capita_urban_pop</th>
      <td>1.000000</td>
      <td>0.998218</td>
    </tr>
    <tr>
      <th>mean_total_pop</th>
      <td>0.998218</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(1, 1, figsize=(10, 10));
ax.set_title("Correlation plot between mean world population and mean per capita world urban population from 1990 to 2010 \n(with regression line)", fontsize=20, pad=25)
ax.set_xlabel("Mean world population")
ax.set_ylabel("Mean per capita urban population")
ax.xaxis.set_major_formatter(FormatStrFormatter('%.0f'))
ax = sns.regplot(x="mean_total_pop", y="mean_per_capita_urban_pop", data=_1990_2010_combined_means)

plt.grid()
plt.show()
```



![png](https://github.com/Blindusername001/Geospatial-Plotting-in-python-with-geopandas/blob/main/Images/output_65_0.png)    

    


<div class="alert alert-block alert-info">
    <b> Interpretation </b>

It can be seen that there is a correlation between mean world population and mean urban population per capita.
    
It is a positive correlation as we can see that mean population per capita has increased with the increase in mean total world population.
    
The interpretation from the plot can also be substantiated by the calculated correlation coefficient which has a value of 0.99 (closer to 1) which indicates a string positive correlation.
    
</div>

# References
- https://geopandas.org/docs/user_guide/mapping.html
- https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.legend.html
- https://matplotlib.org/stable/api/colorbar_api.html
- https://geopandas.org/docs/user_guide/mapping.html
- https://stackoverflow.com/questions/53747298/how-to-format-seaborn-matplotlib-axis-tick-labels-from-number-to-thousands-or-mi
- https://www.earthdatascience.org/courses/scientists-guide-to-plotting-data-in-python/plot-with-matplotlib/introduction-to-matplotlib-plots/customize-plot-colors-labels-matplotlib/
- https://medium.com/analytics-vidhya/the-ultimate-markdown-guide-for-jupyter-notebook-d5e5abf728fd
- https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.corr.html
- https://stackoverflow.com/questions/38899190/geopandas-label-polygons
