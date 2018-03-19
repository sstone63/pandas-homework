

```python
import pandas as pd

csv_path = "purchase_data.json"

heroes_df = pd.read_json(csv_path)

heroes_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Total Players
heroes_df_SNsubset = heroes_df.drop_duplicates(subset=["SN"])

Total_Players_Count = heroes_df_SNsubset["SN"].count()

Total_Players = pd.DataFrame({"Total Players": Total_Players_Count}, index=[0])


Total_Players

#heroes_df_SNsubset["Gender"].value_counts()


```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchase Analysis\

Total_Unique_Items = heroes_df["Item ID"].drop_duplicates().count()

Average_Purchasing_Price = heroes_df["Price"].mean()

Total_Purchases = len(heroes_df.index)

Total_Revenue = heroes_df["Price"].sum()

Purchase_Analysis = pd.DataFrame({"Total Unique Items": [Total_Unique_Items], "Average Purchase Price": [Average_Purchasing_Price],
                                  "Total Purchases": [Total_Purchases], "Total Revenue": [Total_Revenue]})
Organized_Purchase_Analysis = Purchase_Analysis[["Total Unique Items", "Average Purchase Price",
                                                 "Total Purchases", "Total Revenue"]]

Organized_Purchase_Analysis["Average Purchase Price"] = Organized_Purchase_Analysis["Average Purchase Price"].map("${:.2f}".format)

Organized_Purchase_Analysis["Total Revenue"] = Organized_Purchase_Analysis["Total Revenue"].map("${:.2f}".format)

Organized_Purchase_Analysis
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Unique Items</th>
      <th>Average Purchase Price</th>
      <th>Total Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics Final
Total_Gender_Count = pd.DataFrame(heroes_df_SNsubset["Gender"].value_counts().reset_index())

Total_Gender_Count = Total_Gender_Count.rename(columns={"index": "Gender", "Gender": "Total Count"})

Gender_Percentage = round(((Total_Gender_Count["Total Count"]/Total_Players_Count)*100),2)

Total_Gender_Count["Percentage of Players"] = Gender_Percentage

Total_Gender_Count = Total_Gender_Count.set_index("Gender")

del Total_Gender_Count.index.name

Total_Gender_Count
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>465</td>
      <td>81.15</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>100</td>
      <td>17.45</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
      <td>1.40</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis

heroes_gender_group = heroes_df.groupby(["Gender"])

Purchasing_Analysis = pd.DataFrame(heroes_df["Gender"].value_counts().reset_index())

Purchasing_Analysis = Purchasing_Analysis.rename(columns={"index": "Gender", "Gender": "Total Purchases"})

Purchasing_Analysis = Purchasing_Analysis.set_index("Gender")

Purchasing_Analysis["Average Purchase Price"] = round(heroes_gender_group["Price"].mean(),2)

Purchasing_Analysis["Total Purchase Value"] = heroes_gender_group["Price"].sum()

Purchasing_Analysis["Normalized Totals"] = round(heroes_gender_group["Price"].sum()/heroes_df_SNsubset["Gender"].value_counts(), 2)

Purchasing_Analysis["Average Purchase Price"] = Purchasing_Analysis["Average Purchase Price"].map("${:.2f}".format)

Purchasing_Analysis["Total Purchase Value"] = Purchasing_Analysis["Total Purchase Value"].map("${:.2f}".format)

Purchasing_Analysis["Normalized Totals"] = Purchasing_Analysis["Normalized Totals"].map("${:.2f}".format)

del Purchasing_Analysis.index.name

Purchasing_Analysis
#heroes_df["Gender"].value_counts()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Purchases</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics

bins = [0, 10, 15, 20, 25, 30, 35, 40, 45]

group_names = ["<10", "10-15", "15-20", "20-25", "25-30", "30-35", "35-40", "40+"]

pd.cut(heroes_df_SNsubset["Age"], bins, labels=group_names)

heroes_df_SNsubset["Age Group"] = pd.cut(heroes_df_SNsubset["Age"], bins, labels=group_names)

heroes_df["Age Group"] = pd.cut(heroes_df["Age"], bins, labels=group_names)

heroes_age_groups_normalized = heroes_df_SNsubset.groupby(["Age Group"])

heroes_age_groups = heroes_df.groupby(["Age Group"])

heroes_age_df = pd.DataFrame(heroes_df_SNsubset["Age Group"].value_counts().reset_index())

heroes_age_df = heroes_age_df.rename(columns={"index": "Age Group", "Age Group": "Total Count"})

heroes_age_df = heroes_age_df.sort_values(by="Age Group")

heroes_age_df = heroes_age_df.set_index("Age Group")

heroes_age_df["Percent of Players"] = round(((heroes_age_df["Total Count"]/Total_Players_Count)*100),2)

del heroes_age_df.index.name

heroes_age_df
```

    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:9: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percent of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>22</td>
      <td>3.84</td>
    </tr>
    <tr>
      <th>10-15</th>
      <td>54</td>
      <td>9.42</td>
    </tr>
    <tr>
      <th>15-20</th>
      <td>139</td>
      <td>24.26</td>
    </tr>
    <tr>
      <th>20-25</th>
      <td>234</td>
      <td>40.84</td>
    </tr>
    <tr>
      <th>25-30</th>
      <td>52</td>
      <td>9.08</td>
    </tr>
    <tr>
      <th>30-35</th>
      <td>44</td>
      <td>7.68</td>
    </tr>
    <tr>
      <th>35-40</th>
      <td>25</td>
      <td>4.36</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>3</td>
      <td>0.52</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis(Age)

heroes_age_purchase_df = pd.DataFrame(heroes_df["Age Group"].value_counts().reset_index())

heroes_age_purchase_df = heroes_age_purchase_df.rename(columns={"index": "Age Group", "Age Group": "Purchase Count"})

heroes_age_purchase_df = heroes_age_purchase_df.sort_values(by="Age Group")

heroes_age_purchase_df = heroes_age_purchase_df.set_index("Age Group")

heroes_age_purchase_df["Average Purchase Price"] = heroes_age_groups["Price"].mean()

heroes_age_purchase_df["Total Purchase Value"] = heroes_age_groups["Price"].sum()

heroes_age_purchase_df["Normalized Totals"] = heroes_age_groups_normalized["Price"].mean()

heroes_age_purchase_df["Average Purchase Price"] = heroes_age_purchase_df["Average Purchase Price"].map("${:.2f}".format)

heroes_age_purchase_df["Total Purchase Value"] = heroes_age_purchase_df["Total Purchase Value"].map("${:.2f}".format)

heroes_age_purchase_df["Normalized Totals"] = heroes_age_purchase_df["Normalized Totals"].map("${:.2f}".format)

del heroes_age_purchase_df.index.name

heroes_age_purchase_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$3.16</td>
    </tr>
    <tr>
      <th>10-15</th>
      <td>78</td>
      <td>$2.87</td>
      <td>$224.15</td>
      <td>$2.89</td>
    </tr>
    <tr>
      <th>15-20</th>
      <td>184</td>
      <td>$2.87</td>
      <td>$528.74</td>
      <td>$2.90</td>
    </tr>
    <tr>
      <th>20-25</th>
      <td>305</td>
      <td>$2.96</td>
      <td>$902.61</td>
      <td>$2.99</td>
    </tr>
    <tr>
      <th>25-30</th>
      <td>76</td>
      <td>$2.89</td>
      <td>$219.82</td>
      <td>$2.92</td>
    </tr>
    <tr>
      <th>30-35</th>
      <td>58</td>
      <td>$3.07</td>
      <td>$178.26</td>
      <td>$3.33</td>
    </tr>
    <tr>
      <th>35-40</th>
      <td>44</td>
      <td>$2.90</td>
      <td>$127.49</td>
      <td>$2.87</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>3</td>
      <td>$2.88</td>
      <td>$8.64</td>
      <td>$2.88</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders

heroes_spenders_df = pd.DataFrame(heroes_df["SN"].value_counts().reset_index())

heroes_spenders_groups = heroes_df.groupby(["SN"])

heroes_spenders_df = heroes_spenders_df.rename(columns={"index": "SN", "SN": "Purchase Count"})

heroes_spenders_df = heroes_spenders_df.set_index("SN")

heroes_spenders_df["Average Purchase Price"] = round(heroes_spenders_groups["Price"].mean(), 2)

heroes_spenders_df["Total Purchase Value"] = round(heroes_spenders_groups["Price"].sum(), 2)

heroes_spenders_df = heroes_spenders_df.sort_values(by="Total Purchase Value", ascending=False)

heroes_spenders_df["Average Purchase Price"] = heroes_spenders_df["Average Purchase Price"].map("${:.2f}".format)

heroes_spenders_df["Total Purchase Value"] = heroes_spenders_df["Total Purchase Value"].map("${:.2f}".format)

heroes_spenders_df.head(10)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
    <tr>
      <th>Isursti83</th>
      <td>3</td>
      <td>$3.68</td>
      <td>$11.05</td>
    </tr>
    <tr>
      <th>Isurria36</th>
      <td>3</td>
      <td>$3.67</td>
      <td>$11.01</td>
    </tr>
    <tr>
      <th>Eusri70</th>
      <td>3</td>
      <td>$3.52</td>
      <td>$10.55</td>
    </tr>
    <tr>
      <th>Aerithllora36</th>
      <td>3</td>
      <td>$3.48</td>
      <td>$10.45</td>
    </tr>
    <tr>
      <th>Yasriphos60</th>
      <td>3</td>
      <td>$3.47</td>
      <td>$10.40</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Items

heroes_pg_unlimited = heroes_df.groupby(["Item ID", "Item Name"])

heroes_popular_group = heroes_df.groupby(["Item ID", "Item Name"]).size()

heroes_popular_df = pd.DataFrame(heroes_popular_group)

heroes_popular_df = heroes_popular_df.rename(columns={0: "Purchase Count"})

heroes_popular_df["Item Price"] = round(heroes_pg_unlimited["Price"].mean(),2)

heroes_popular_df["Total Purchase Value"] = heroes_pg_unlimited["Price"].sum()

heroes_popular_df_sorted = heroes_popular_df.sort_values(by="Purchase Count", ascending=False)

heroes_popular_df_sorted["Item Price"] = heroes_popular_df_sorted["Item Price"].map("${:.2f}".format)

heroes_popular_df_sorted["Total Purchase Value"] = heroes_popular_df_sorted["Total Purchase Value"].map("${:.2f}".format)

heroes_popular_df_sorted.head(10)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>65</th>
      <th>Conqueror Adamantite Mace</th>
      <td>8</td>
      <td>$1.96</td>
      <td>$15.68</td>
    </tr>
    <tr>
      <th>152</th>
      <th>Darkheart</th>
      <td>8</td>
      <td>$3.15</td>
      <td>$25.20</td>
    </tr>
    <tr>
      <th>44</th>
      <th>Bonecarvin Battle Axe</th>
      <td>8</td>
      <td>$2.46</td>
      <td>$19.68</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable Items

heroes_profitable_df = heroes_popular_df.sort_values(by="Total Purchase Value", ascending=False)\

heroes_profitable_df["Item Price"] = heroes_profitable_df["Item Price"].map("${:.2f}".format)

heroes_profitable_df["Total Purchase Value"] = heroes_profitable_df["Total Purchase Value"].map("${:.2f}".format)

heroes_profitable_df.head(10)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
    <tr>
      <th>101</th>
      <th>Final Critic</th>
      <td>6</td>
      <td>$4.62</td>
      <td>$27.72</td>
    </tr>
    <tr>
      <th>7</th>
      <th>Thorn, Satchel of Dark Souls</th>
      <td>6</td>
      <td>$4.51</td>
      <td>$27.06</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>6</td>
      <td>$4.45</td>
      <td>$26.70</td>
    </tr>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>152</th>
      <th>Darkheart</th>
      <td>8</td>
      <td>$3.15</td>
      <td>$25.20</td>
    </tr>
  </tbody>
</table>
</div>


