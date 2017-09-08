
# Pymaceuticals
- Treatment including Capomulin exhibits a decrease in tumor size.
- Treatment including Capomulin exhibits slower spread of metastatic sites. 
- Treatment including Capomulin exhibits slower decrease in survival rates.


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns #Samuel Norman Seaborn
```


```python
mouseFile = "resources/mouse_drug_data.csv"
drugFile = "resources/clinicaltrial_data.csv"
mouseDF = pd.read_csv(mouseFile)
drugDF = pd.read_csv(drugFile)
```


```python
mouseDF[mouseDF["Drug"]=="Capomulin"].sort_values(by="Mouse ID").head(3)
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
      <th>Mouse ID</th>
      <th>Drug</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>75</th>
      <td>b128</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>80</th>
      <td>b742</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>81</th>
      <td>f966</td>
      <td>Capomulin</td>
    </tr>
  </tbody>
</table>
</div>




```python
drugDF[(drugDF["Metastatic Sites"]==0)&(drugDF["Timepoint"]==0)].head()
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
      <th>Mouse ID</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>b128</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>f932</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>g107</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>a457</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>c819</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# select only the 4 drugs we are comparing
drugs4 = mouseDF[(mouseDF["Drug"]=="Capomulin")|(mouseDF["Drug"]=="Infubinol")|(mouseDF["Drug"]=="Ketapril")|(mouseDF["Drug"]=="Placebo")].merge(drugDF)
```

### Tumor Response to Treatment


```python
# get the mean tumor volume by drug by timepoint
TumorVolume = drugs4.groupby(["Drug","Timepoint"])["Tumor Volume (mm3)"].mean().to_frame("Tumor Volume (mm3)")
# pivot the results so each drug is a column
TumorVolumePivot = pd.pivot_table(TumorVolume, index=["Timepoint"], values=["Tumor Volume (mm3)"], columns="Drug")
# get the sem for tumor volume for each drug by timepoint
TumorVolumeSEM = drugs4.groupby(["Drug","Timepoint"])["Tumor Volume (mm3)"].sem().to_frame("Tumor Volume (mm3)")
# pivot the results so each drug is a column
TumorVolumeSEMPivot = pd.pivot_table(TumorVolumeSEM, index=["Timepoint"], values=["Tumor Volume (mm3)"], columns="Drug")
TumorVolumePivot

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
    <tr>
      <th></th>
      <th colspan="4" halign="left">Tumor Volume (mm3)</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Placebo</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>44.266086</td>
      <td>47.062001</td>
      <td>47.389175</td>
      <td>47.125589</td>
    </tr>
    <tr>
      <th>10</th>
      <td>43.084291</td>
      <td>49.403909</td>
      <td>49.582269</td>
      <td>49.423329</td>
    </tr>
    <tr>
      <th>15</th>
      <td>42.064317</td>
      <td>51.296397</td>
      <td>52.399974</td>
      <td>51.359742</td>
    </tr>
    <tr>
      <th>20</th>
      <td>40.716325</td>
      <td>53.197691</td>
      <td>54.920935</td>
      <td>54.364417</td>
    </tr>
    <tr>
      <th>25</th>
      <td>39.939528</td>
      <td>55.715252</td>
      <td>57.678982</td>
      <td>57.482574</td>
    </tr>
    <tr>
      <th>30</th>
      <td>38.769339</td>
      <td>58.299397</td>
      <td>60.994507</td>
      <td>59.809063</td>
    </tr>
    <tr>
      <th>35</th>
      <td>37.816839</td>
      <td>60.742461</td>
      <td>63.371686</td>
      <td>62.420615</td>
    </tr>
    <tr>
      <th>40</th>
      <td>36.958001</td>
      <td>63.162824</td>
      <td>66.068580</td>
      <td>65.052675</td>
    </tr>
    <tr>
      <th>45</th>
      <td>36.236114</td>
      <td>65.755562</td>
      <td>70.662958</td>
      <td>68.084082</td>
    </tr>
  </tbody>
</table>
</div>




```python
# plot the results

# TumorVolumePivot.index.get_level_values(0) is equal to the TumorVolumePivot["Timepoint"] if we don't reset the index
# TumorVolumePivot[('Tumor Volume (mm3)','Capomulin')] - need to specify both levels of index (tumor volume and drug) to get drug column since tumor level was created as level when creating pivot using pivot_table
plt.errorbar(TumorVolumePivot.index.get_level_values(0), TumorVolumePivot[('Tumor Volume (mm3)','Capomulin')], color="xkcd:hot pink", marker="X", label="Capomulin", yerr=TumorVolumeSEMPivot[('Tumor Volume (mm3)','Capomulin')])
plt.errorbar(TumorVolumePivot.index.get_level_values(0), TumorVolumePivot[('Tumor Volume (mm3)','Infubinol')],  marker="s", color="m", label="Infubinol", yerr=TumorVolumeSEMPivot[('Tumor Volume (mm3)','Infubinol')])
plt.errorbar(TumorVolumePivot.index.get_level_values(0), TumorVolumePivot[('Tumor Volume (mm3)','Ketapril')], marker = "o", color="c", label="Ketapril", yerr=TumorVolumeSEMPivot[('Tumor Volume (mm3)','Ketapril')])
plt.errorbar(TumorVolumePivot.index.get_level_values(0), TumorVolumePivot[('Tumor Volume (mm3)','Placebo')], color="k", marker="D", label="Placebo", yerr=TumorVolumeSEMPivot[('Tumor Volume (mm3)','Placebo')])

plt.title("Capomulin Exhibits Reduction in Tumor Size")
plt.xlabel("Days of Treatment")
plt.ylabel("Tumor Volume (mm3)")   
plt.legend()

plt.show()
```


![png](output_8_0.png)


### Metastatic Response to Treatment


```python
# get the mean metastatic sites per drug per timepoint
MetastaticSites = drugs4.groupby(["Drug","Timepoint"])["Metastatic Sites"].mean().to_frame("Metastatic Sites")
# pivot the results so that each drug is a column
#  NOTE: if values is in brackets, it creates another level and must be called in addition to column
MetastaticSitesPivot = pd.pivot_table(MetastaticSites, index="Timepoint", columns="Drug", values=["Metastatic Sites"])
# get the sem of the metastatic sites
MetastaticSitesSEM = drugs4.groupby(["Drug","Timepoint"])["Metastatic Sites"].sem().to_frame("Metastatic Sites")
# pivot the results so that each drug is a column
MetastaticSitesSEMPivot = pd.pivot_table(MetastaticSitesSEM, index="Timepoint", columns="Drug", values="Metastatic Sites")
MetastaticSitesPivot.reset_index(inplace=True)
MetastaticSitesSEMPivot
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
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Placebo</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.074833</td>
      <td>0.091652</td>
      <td>0.098100</td>
      <td>0.100947</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.125433</td>
      <td>0.159364</td>
      <td>0.142018</td>
      <td>0.115261</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.132048</td>
      <td>0.194015</td>
      <td>0.191381</td>
      <td>0.190221</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.161621</td>
      <td>0.234801</td>
      <td>0.236680</td>
      <td>0.234064</td>
    </tr>
    <tr>
      <th>25</th>
      <td>0.181818</td>
      <td>0.265753</td>
      <td>0.288275</td>
      <td>0.263888</td>
    </tr>
    <tr>
      <th>30</th>
      <td>0.172944</td>
      <td>0.227823</td>
      <td>0.347467</td>
      <td>0.300264</td>
    </tr>
    <tr>
      <th>35</th>
      <td>0.169496</td>
      <td>0.224733</td>
      <td>0.361418</td>
      <td>0.341412</td>
    </tr>
    <tr>
      <th>40</th>
      <td>0.175610</td>
      <td>0.314466</td>
      <td>0.315725</td>
      <td>0.297294</td>
    </tr>
    <tr>
      <th>45</th>
      <td>0.202591</td>
      <td>0.309320</td>
      <td>0.278722</td>
      <td>0.304240</td>
    </tr>
  </tbody>
</table>
</div>




```python
# plot the results
#Note: can use MetastaticSitesPivot["Timepoint"] because we reset the index above
plt.errorbar(MetastaticSitesPivot["Timepoint"], MetastaticSitesPivot[("Metastatic Sites","Capomulin")], color="xkcd:hot pink", marker="X", label="Capomulin",yerr=MetastaticSitesSEMPivot["Capomulin"])
plt.errorbar(MetastaticSitesPivot["Timepoint"], MetastaticSitesPivot[("Metastatic Sites","Infubinol")], color="m", marker="s", label="Infubinol",yerr=MetastaticSitesSEMPivot["Infubinol"])
plt.errorbar(MetastaticSitesPivot["Timepoint"], MetastaticSitesPivot[("Metastatic Sites","Ketapril")], color="c", marker="o", label="Ketapril",yerr=MetastaticSitesSEMPivot["Ketapril"])
plt.errorbar(MetastaticSitesPivot["Timepoint"], MetastaticSitesPivot[("Metastatic Sites","Placebo")], color="k", marker="D", label="Placebo",yerr=MetastaticSitesSEMPivot["Placebo"])

plt.title("Capomulin Exhibits Slower Spread of Metastatic Sites")
plt.xlabel("Days of Treatment")
plt.ylabel("Metastatic Sites")       
plt.legend()

plt.show()
```


![png](output_11_0.png)


### Survival Rates


```python
# get the survival rates per drug per timepoint
SurvivalRates = drugs4.groupby(["Drug","Timepoint"])["Mouse ID"].count().to_frame("Survival Rates")
# pivot the results so that each drug is a column
SurvivalRatesPivot = pd.pivot_table(SurvivalRates, index="Timepoint", columns="Drug", values="Survival Rates")
# Two methods to calculate percent change of each value in table
# option 1: with lambda
# SurvivalRatesPivot= round(SurvivalRatesPivot.apply(lambda c: c / c.max() * 100, axis=0),2) 
# option 2: without lambda
SurvivalRatesPivot = SurvivalRatesPivot/SurvivalRatesPivot.max()*100
SurvivalRatesPivot
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
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Placebo</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>92.0</td>
      <td>96.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>100.0</td>
      <td>84.0</td>
      <td>88.0</td>
      <td>96.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>96.0</td>
      <td>84.0</td>
      <td>76.0</td>
      <td>80.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>92.0</td>
      <td>80.0</td>
      <td>76.0</td>
      <td>76.0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>88.0</td>
      <td>72.0</td>
      <td>76.0</td>
      <td>68.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>88.0</td>
      <td>68.0</td>
      <td>72.0</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>88.0</td>
      <td>48.0</td>
      <td>68.0</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>40</th>
      <td>84.0</td>
      <td>40.0</td>
      <td>60.0</td>
      <td>48.0</td>
    </tr>
    <tr>
      <th>45</th>
      <td>84.0</td>
      <td>36.0</td>
      <td>44.0</td>
      <td>44.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#plot the results
plt.plot(SurvivalRatesPivot.index.get_level_values(0), SurvivalRatesPivot["Capomulin"], color="xkcd:hot pink", marker="X", label="Capomulin")
plt.plot(SurvivalRatesPivot.index.get_level_values(0), SurvivalRatesPivot["Infubinol"], color="m", marker="s", label="Infubinol")
plt.plot(SurvivalRatesPivot.index.get_level_values(0), SurvivalRatesPivot["Ketapril"], color="c", marker="o", label="Ketapril")
plt.plot(SurvivalRatesPivot.index.get_level_values(0), SurvivalRatesPivot["Placebo"], color="k", marker="D", label="Placebo")

plt.title("Capomulin Exhibits Slower Decline in Survival Rates")
plt.xlabel("Days of Treatment")
plt.ylabel("Survival Rates Percentage")        
plt.legend()

plt.show()
```


![png](output_14_0.png)


### Percentage of Tumor Change


```python
# get the change in tumor volume for each drug
#let's use pivot instead of pivot_table so we avoid multiple indexes (TumorVolumePivot was made via pivot_table from TumorVolume)
TumorVolume.reset_index(inplace=True) #this should only be run once
TumorVolumeChangePivot = TumorVolume.pivot(index="Timepoint", columns="Drug")["Tumor Volume (mm3)"]
#subtract the first volume (iloc[0]) from the last volume (iloc[-1]), divide by the first volume (iloc[0]), multiply by 100
TumorChangePercent = (((TumorVolumeChangePivot.iloc[-1]-TumorVolumeChangePivot.iloc[0])/TumorVolumeChangePivot.iloc[0])*100).to_frame("% Change")
# Loop through % change and determine color of bar
color=[]
for p in TumorChangePercent["% Change"]:
    if p < 0:
        color.append("green")
    else:
        color.append("red")
    
TumorChangePercent.plot(kind="bar", color=color, alpha=.65, rot=0)
#remove the legend
plt.legend("")
# add the percentages on the bars
x_axis = np.arange(len(TumorChangePercent))
for a,b in zip(x_axis, TumorChangePercent["% Change"]):
    if b<0:
        plt.text(a-.1, b-b - 3, "{:.0f}%".format(b))
    else:
        plt.text(a-.075, b - b + 1, "{:.0f}%".format(b))

plt.title("Capomulin Exhibits Tumor Shrinkage Over 45 Day Treatment")
plt.xlabel("Drug")
plt.ylabel("% Tumor Volume Change")  
plt.axhline(y=0, c="gray", alpha=.45)

    
plt.show()
```


![png](output_16_0.png)

