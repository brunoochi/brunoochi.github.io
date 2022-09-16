---
layout: post
title: Price Volume Mix Analysis
categories: [Analysis]
---

# Processing data for PVM Analysis
The goal of this notebook is to process the sales data so that we can build the PVM analysis for "Desafinados Karaoke Lounge" - a fake karaoke business that operates in Japan). We will first create several supporting columns and then slice and dice the table to build the version of the data that we need for Tableau.

In Tableau, we will build waterfall charts. These charts will show how individual sales drivers have contributed to changes in monthly sales over a period of 1 year. Here's what I mean:

- Sales in April 2020 = X
  -  (Driver 1) Change in sales attributable to change in unit price = p
  -  (Driver 2) Change in sales attributable to change in average visit = v
  -  (Driver 3) Change in sales attributable to change member count = m 
- Sales in April 2021 = X + p + v + m

Ideally, the karaoke business wants to grow its active customer base (m), grow the frequency that customers come to their venue (v), and grow the money spent by selling drinks, snacks, and other optional features (p). 


```python
import pandas as pd

# Load the dummy data
df = pd.read_csv("basic_salesdata.csv")
```


```python
# dummy data looks like this
df.head()
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
      <th>date</th>
      <th>store</th>
      <th>sales</th>
      <th>totalvisit</th>
      <th>membercnt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-04-30</td>
      <td>Shinjuku Central</td>
      <td>163602</td>
      <td>227</td>
      <td>178</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-05-31</td>
      <td>Shinjuku Central</td>
      <td>142562</td>
      <td>281</td>
      <td>146</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-06-30</td>
      <td>Shinjuku Central</td>
      <td>154344</td>
      <td>325</td>
      <td>161</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-07-31</td>
      <td>Shinjuku Central</td>
      <td>166968</td>
      <td>237</td>
      <td>143</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-08-31</td>
      <td>Shinjuku Central</td>
      <td>189017</td>
      <td>286</td>
      <td>162</td>
    </tr>
  </tbody>
</table>
</div>




```python
# "Desafindos" has 10 karaoke lounges in Japan
df["store"].unique()
```




    array(['Shinjuku Central', 'Harajuku Boulevard', 'Shinagawa Square',
           'Tokyo Bay', 'Yokohama Port', 'Osaka Metro', 'Osaka Sunset Plaza',
           'Nagoya Tower', 'Hakata Outlet', 'Okinawa Seaside'], dtype=object)




```python
# This is sales data for 24 months of business. Includes 2 fiscal years (FY21: 2020-4 to 2021-3 and FY22: 2021-4 to 2022-3).
df["date"].unique()
```




    array(['2020-04-30', '2020-05-31', '2020-06-30', '2020-07-31',
           '2020-08-31', '2020-09-30', '2020-10-31', '2020-11-30',
           '2020-12-31', '2021-01-31', '2021-02-28', '2021-03-31',
           '2021-04-30', '2021-05-31', '2021-06-30', '2021-07-31',
           '2021-08-31', '2021-09-30', '2021-10-31', '2021-11-30',
           '2021-12-31', '2022-01-31', '2022-02-28', '2022-03-31'],
          dtype=object)




```python
# Build necessary columns
df["month"] = df["date"].apply(lambda x: x.split("-")[1])
df["yearmonth"] = df["date"].apply(lambda x: (x.split("-")[0])) + df["date"].apply(lambda x: x.split("-")[1])
df["yearmonth"] = df["yearmonth"].astype("int")
df["avgprice"] = df["sales"] / df["totalvisit"]
df["avgvisit"] = df["totalvisit"] / df["membercnt"]
df["FY"] = df["yearmonth"].apply(lambda x: "FY21" if x <= 202103 else "FY22")
df.head()
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
      <th>date</th>
      <th>store</th>
      <th>sales</th>
      <th>totalvisit</th>
      <th>membercnt</th>
      <th>month</th>
      <th>yearmonth</th>
      <th>avgprice</th>
      <th>avgvisit</th>
      <th>FY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-04-30</td>
      <td>Shinjuku Central</td>
      <td>163602</td>
      <td>227</td>
      <td>178</td>
      <td>04</td>
      <td>202004</td>
      <td>720.713656</td>
      <td>1.275281</td>
      <td>FY21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-05-31</td>
      <td>Shinjuku Central</td>
      <td>142562</td>
      <td>281</td>
      <td>146</td>
      <td>05</td>
      <td>202005</td>
      <td>507.338078</td>
      <td>1.924658</td>
      <td>FY21</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-06-30</td>
      <td>Shinjuku Central</td>
      <td>154344</td>
      <td>325</td>
      <td>161</td>
      <td>06</td>
      <td>202006</td>
      <td>474.904615</td>
      <td>2.018634</td>
      <td>FY21</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-07-31</td>
      <td>Shinjuku Central</td>
      <td>166968</td>
      <td>237</td>
      <td>143</td>
      <td>07</td>
      <td>202007</td>
      <td>704.506329</td>
      <td>1.657343</td>
      <td>FY21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-08-31</td>
      <td>Shinjuku Central</td>
      <td>189017</td>
      <td>286</td>
      <td>162</td>
      <td>08</td>
      <td>202008</td>
      <td>660.898601</td>
      <td>1.765432</td>
      <td>FY21</td>
    </tr>
  </tbody>
</table>
</div>



Let's start the data processing work


```python
# Divide the table into two tables: one for each fiscal year
df_FY21 = df[df["FY"] == "FY21"]
df_FY22 = df[df["FY"] == "FY22"]

# Merge fiscal year tables so that all is aligned for easy calculation
df_prepcalc = df_FY21.merge(df_FY22, left_on=["store", "month"], right_on=["store", "month"])
df_prepcalc.head()
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
      <th>date_x</th>
      <th>store</th>
      <th>sales_x</th>
      <th>totalvisit_x</th>
      <th>membercnt_x</th>
      <th>month</th>
      <th>yearmonth_x</th>
      <th>avgprice_x</th>
      <th>avgvisit_x</th>
      <th>FY_x</th>
      <th>date_y</th>
      <th>sales_y</th>
      <th>totalvisit_y</th>
      <th>membercnt_y</th>
      <th>yearmonth_y</th>
      <th>avgprice_y</th>
      <th>avgvisit_y</th>
      <th>FY_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-04-30</td>
      <td>Shinjuku Central</td>
      <td>163602</td>
      <td>227</td>
      <td>178</td>
      <td>04</td>
      <td>202004</td>
      <td>720.713656</td>
      <td>1.275281</td>
      <td>FY21</td>
      <td>2021-04-30</td>
      <td>183799</td>
      <td>282</td>
      <td>145</td>
      <td>202104</td>
      <td>651.769504</td>
      <td>1.944828</td>
      <td>FY22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-05-31</td>
      <td>Shinjuku Central</td>
      <td>142562</td>
      <td>281</td>
      <td>146</td>
      <td>05</td>
      <td>202005</td>
      <td>507.338078</td>
      <td>1.924658</td>
      <td>FY21</td>
      <td>2021-05-31</td>
      <td>149295</td>
      <td>323</td>
      <td>137</td>
      <td>202105</td>
      <td>462.213622</td>
      <td>2.357664</td>
      <td>FY22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-06-30</td>
      <td>Shinjuku Central</td>
      <td>154344</td>
      <td>325</td>
      <td>161</td>
      <td>06</td>
      <td>202006</td>
      <td>474.904615</td>
      <td>2.018634</td>
      <td>FY21</td>
      <td>2021-06-30</td>
      <td>146434</td>
      <td>294</td>
      <td>184</td>
      <td>202106</td>
      <td>498.074830</td>
      <td>1.597826</td>
      <td>FY22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-07-31</td>
      <td>Shinjuku Central</td>
      <td>166968</td>
      <td>237</td>
      <td>143</td>
      <td>07</td>
      <td>202007</td>
      <td>704.506329</td>
      <td>1.657343</td>
      <td>FY21</td>
      <td>2021-07-31</td>
      <td>149463</td>
      <td>266</td>
      <td>127</td>
      <td>202107</td>
      <td>561.890977</td>
      <td>2.094488</td>
      <td>FY22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-08-31</td>
      <td>Shinjuku Central</td>
      <td>189017</td>
      <td>286</td>
      <td>162</td>
      <td>08</td>
      <td>202008</td>
      <td>660.898601</td>
      <td>1.765432</td>
      <td>FY21</td>
      <td>2021-08-31</td>
      <td>201304</td>
      <td>292</td>
      <td>175</td>
      <td>202108</td>
      <td>689.397260</td>
      <td>1.668571</td>
      <td>FY22</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Reordering the columns
df_prepcalc = df_prepcalc[['store', 'month', 'date_x', 'sales_x', 'totalvisit_x', 'membercnt_x',
       'yearmonth_x', 'avgprice_x', 'avgvisit_x', 'FY_x', 'date_y', 'sales_y',
       'totalvisit_y', 'membercnt_y', 'yearmonth_y', 'avgprice_y',
       'avgvisit_y', 'FY_y']]
```


```python
# Calculate the sales effect for each driver
df_prepcalc["member_effect"] = df_prepcalc.apply(lambda x: (x["membercnt_y"] - x["membercnt_x"]) * x["avgvisit_x"] * x["avgprice_x"] , axis=1)
df_prepcalc["avgvisit_effect"] = df_prepcalc.apply(lambda x: (x["avgvisit_y"] - x["avgvisit_x"]) * x["membercnt_y"] * x["avgprice_x"] , axis=1)
df_prepcalc["avgprice_effect"] = df_prepcalc.apply(lambda x: (x["avgprice_y"] - x["avgprice_x"]) * x["membercnt_y"] * x["avgvisit_y"] , axis=1)

```


```python
# Rename columns
newcols = [x.replace("_x", "_1").replace("_y", "_2") for x in df_prepcalc.columns]
newcols_map = dict(zip(df_prepcalc.columns, newcols))
df_prepcalc.rename(columns=newcols_map, inplace=True)

# Keep columns for waterfall chart
df_bridge = df_prepcalc[['store', 'month', 'date_1', 'date_2', 'FY_1', 'FY_2','sales_1', "member_effect", "avgvisit_effect", "avgprice_effect", 'sales_2']]
df_bridge_melt = df_bridge.melt(id_vars=["store", "month", "date_1", "date_2" ,"FY_1", "FY_2"])

# Pick a single store / month and check calculations:
df_bridge_melt[(df_bridge_melt["store"] == "Okinawa Seaside") & (df_bridge_melt["month"] == "04")] 

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
      <th>store</th>
      <th>month</th>
      <th>date_1</th>
      <th>date_2</th>
      <th>FY_1</th>
      <th>FY_2</th>
      <th>variable</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>108</th>
      <td>Okinawa Seaside</td>
      <td>04</td>
      <td>2020-04-30</td>
      <td>2021-04-30</td>
      <td>FY21</td>
      <td>FY22</td>
      <td>sales_1</td>
      <td>61878.000000</td>
    </tr>
    <tr>
      <th>228</th>
      <td>Okinawa Seaside</td>
      <td>04</td>
      <td>2020-04-30</td>
      <td>2021-04-30</td>
      <td>FY21</td>
      <td>FY22</td>
      <td>member_effect</td>
      <td>16524.238636</td>
    </tr>
    <tr>
      <th>348</th>
      <td>Okinawa Seaside</td>
      <td>04</td>
      <td>2020-04-30</td>
      <td>2021-04-30</td>
      <td>FY21</td>
      <td>FY22</td>
      <td>avgvisit_effect</td>
      <td>-30984.858202</td>
    </tr>
    <tr>
      <th>468</th>
      <td>Okinawa Seaside</td>
      <td>04</td>
      <td>2020-04-30</td>
      <td>2021-04-30</td>
      <td>FY21</td>
      <td>FY22</td>
      <td>avgprice_effect</td>
      <td>13996.619565</td>
    </tr>
    <tr>
      <th>588</th>
      <td>Okinawa Seaside</td>
      <td>04</td>
      <td>2020-04-30</td>
      <td>2021-04-30</td>
      <td>FY21</td>
      <td>FY22</td>
      <td>sales_2</td>
      <td>61414.000000</td>
    </tr>
  </tbody>
</table>
</div>



# Build Supporting Table

Necessary data to build the waterfall chart is ready. To support our Tableau dashboard, I will build a KPI table that will show numerically how each driver changed from one period to the other.


```python
# get necessary columns
df_kpi = df_prepcalc[["store", "month", "date_1", "date_2" ,"FY_1", "FY_2", "sales_1", "avgprice_1", "avgvisit_1", "membercnt_1", "sales_2", "avgprice_2", "avgvisit_2", "membercnt_2"]]
df_kpi_melt = df_kpi.melt(id_vars=["store", "month", "date_1", "date_2" ,"FY_1", "FY_2"])
```


```python
# break table into two: one has data from the period 1 and other from period 2
df_kpi_melt2 = df_kpi_melt[df_kpi_melt["variable"].isin(["sales_2", "avgprice_2", "avgvisit_2", "membercnt_2"])]
df_kpi_melt1 = df_kpi_melt[df_kpi_melt["variable"].isin(["sales_1", "avgprice_1", "avgvisit_1", "membercnt_1"])]

# melt both data
df_kpi_melt2["variable"] = df_kpi_melt2.apply(lambda x : x["variable"].replace("_2",""), axis=1)
df_kpi_melt1["variable"] = df_kpi_melt1.apply(lambda x : x["variable"].replace("_1",""), axis=1)

# merge and align
df_kpi_melt_tbl = df_kpi_melt1.merge(df_kpi_melt2,
    left_on=["store","month","date_1","date_2","FY_1","FY_2","variable"],
    right_on=["store","month","date_1","date_2","FY_1","FY_2","variable"])


```

    C:\Users\ochib\AppData\Local\Temp\ipykernel_19232\2572064843.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_kpi_melt2["variable"] = df_kpi_melt2.apply(lambda x : x["variable"].replace("_2",""), axis=1)
    C:\Users\ochib\AppData\Local\Temp\ipykernel_19232\2572064843.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_kpi_melt1["variable"] = df_kpi_melt1.apply(lambda x : x["variable"].replace("_1",""), axis=1)
    


```python
# rename value column
df_kpi_melt_tbl.rename(columns={"value_x":"value_1","value_y":"value_2"}, inplace=True)

# check data for a single store / month
df_kpi_melt_tbl[(df_kpi_melt_tbl["store"] == "Shinjuku Central") & (df_kpi_melt_tbl["month"] == "04")]
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
      <th>store</th>
      <th>month</th>
      <th>date_1</th>
      <th>date_2</th>
      <th>FY_1</th>
      <th>FY_2</th>
      <th>variable</th>
      <th>value_1</th>
      <th>value_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Shinjuku Central</td>
      <td>04</td>
      <td>2020-04-30</td>
      <td>2021-04-30</td>
      <td>FY21</td>
      <td>FY22</td>
      <td>sales</td>
      <td>163602.000000</td>
      <td>183799.000000</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Shinjuku Central</td>
      <td>04</td>
      <td>2020-04-30</td>
      <td>2021-04-30</td>
      <td>FY21</td>
      <td>FY22</td>
      <td>avgprice</td>
      <td>720.713656</td>
      <td>651.769504</td>
    </tr>
    <tr>
      <th>240</th>
      <td>Shinjuku Central</td>
      <td>04</td>
      <td>2020-04-30</td>
      <td>2021-04-30</td>
      <td>FY21</td>
      <td>FY22</td>
      <td>avgvisit</td>
      <td>1.275281</td>
      <td>1.944828</td>
    </tr>
    <tr>
      <th>360</th>
      <td>Shinjuku Central</td>
      <td>04</td>
      <td>2020-04-30</td>
      <td>2021-04-30</td>
      <td>FY21</td>
      <td>FY22</td>
      <td>membercnt</td>
      <td>178.000000</td>
      <td>145.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Export into csv
df_kpi_melt_tbl.to_csv("kpitbl.csv", index=False)
df_bridge_melt.to_csv("bridge.csv", index=False)
```

Next step is to build the waterfall charts in Tableau. Find the dashboard [here](https://public.tableau.com/app/profile/bochi/viz/PVMAnalysis/PVMAnalysis) (hosted in Tableau Public).
