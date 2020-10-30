# Solar Panel EDA

## Python Models 
* Pandas 
* Matplotlib
* Seaborn
* Numpy
* Scipy
* Statmodels
    
## Data
   Data is taken from Berkeley Lab [Tracking the Sun Report](https://emp.lbl.gov/tracking-the-sun), where csv files has data for 1998-2018 year across the 28 states. Data is collected from US government agencies. 
### Clean up and transformations
None values were give as -9999, those points were cleaned up and four more columns were added. Two most important calculated columns are:

 cost_per_KW = total installation cost over system size\
 cost_per_KW_with_rebate = (total installation cost - rebate) over system size


## Explaitory Data Analysis
   Jupyter notebook was used to create EDA focused on Costs, States, Customer Segments, Module Technologies and Efficiencies. 
   
### States
   Out of 1.5 million installations more than 50%  occured in California during 1998-2018. Reasons for this could be that installations started in California earlier than in other states, population as it is most populous state, and incetive programms offered by state.
   
![States](https://github.com/aydin-hasanli/Solar-Panel-EDA/blob/main/Images/number_of_install_by_state.png)

### Segments
   Plot below demostrates that mean cost per KW is not different for most of the segments. Also 94.4% of the segments are Resedential, so futher analysis is not going differentiate between segements. Especially average cost per KW converges as time passes. 
   
    df_rep['Customer Segment'].value_counts().cumsum()/1187660

    RES           0.944009
    COM           0.961766
    NON-RES       0.972001
    GOV           0.975557
    NON-PROFIT    0.978000
    SCHOOL        0.979582
    Name: Customer Segment, dtype: float64

![Segments](https://github.com/aydin-hasanli/Solar-Panel-EDA/blob/main/Images/cost%20per%20size%20change%20by%20segment.png)


### Module Technologies
   Exploring module technologies reveals that mostly Monocrystalline and Polycrystalline Silicon Solar PV were used. 

![ModulesTechs](https://github.com/aydin-hasanli/Solar-Panel-EDA/blob/main/Images/number_Module_Technology_1.png)

Abbreviations for some of techologies are given below.

|abbreviation |           Full name |
|-----------|-----------------------------------------------|
| Mono      | Monocrystalline Silicon Solar PV              |
| Poly      | Polycrystalline Silicon Solar PV              |
| Thin Film | Thin Film Solar PV                            |
| CdTe      | Thin Film Solar Cadmium telluride photovoltaics |
| CIGS      | copper indium gallium diselenide              |
| a-Si      | amorphous thin-film silicon                   |
| CIS       | Copper, Indium and Selenium                   | 

### Hypotethis testing Kruskal–Wallis test 
Kruskal–Wallis test is non-parametric method for testing that compares two or more independent samples of equal or different sample sizes.
Kruskal–Wallis test was used to perform Hypotethis testing of between several types of module technology. \
\
Null hypothesys: Cost per KW is same the for all module technology types.\
Alternative hypothesys: Cost per KW is same the for all module technology types.\
Signnificance level threshold is 5%.



First several dataframes were created for each technology type with only column 'cost_per_KW_with_rebate'.

```   
    df_rep_CIS=df_rep[df_rep['Module Technology #1']=='CIS']['cost_per_KW_with_rebate']
    df_rep_CdTe=df_rep[df_rep['Module Technology #1']=='CdTe']['cost_per_KW_with_rebate']
    df_rep_Mono=df_rep[df_rep['Module Technology #1']=='Mono']['cost_per_KW_with_rebate']
    df_rep_MonoSci=df_rep[df_rep['Module Technology #1']=='Mono + a-Si']['cost_per_KW_with_rebate']
    df_rep_Poly=df_rep[df_rep['Module Technology #1']=='Poly']['cost_per_KW_with_rebate']
    df_rep_Thin_Film=df_rep[df_rep['Module Technology #1']=='Thin Film']['cost_per_KW_with_rebate']
    df_rep_crystalline=df_rep[df_rep['Module Technology #1']=='crystalline']['cost_per_KW_with_rebate']
    df_rep_multiple=df_rep[df_rep['Module Technology #1']=='multiple']['cost_per_KW_with_rebate']
  
 ```

Then Kruskal–Wallis test from scipy.stats module was performed on all groups
      
stat=%.0f, p-value=%.3f' % (stat, p))


   
```
    stat, p = stats.kruskal(df_rep_CIS,df_rep_CdTe,df_rep_Mono,df_rep_MonoSci,df_rep_Poly, df_rep_Thin_Film, df_rep_crystalline, df_rep_multiple)
    print('stat=%.0f, p-value=%.3f' % (stat, p))
```
Output: \
    ```
    stat=16444, p-value=0.000
    ```\
\
**P-value is 0 so we can reject the null hypotethis.**

Also Kruskal–Wallis test was performed only on mono and poly technology groups. As those  two groups are most common. The result is same, as p-value is zero. 
```
    stat, p = stats.kruskal(df_rep_CIS,df_rep_CdTe,df_rep_Mono,df_rep_MonoSci,df_rep_Poly, df_rep_Thin_Film, df_rep_crystalline, df_rep_multiple)
    print('stat=%.0f, p-value=%.3f' % (stat, p))
```
Output: \
    ```
    stat=16444, p-value=0.000
    ```\

If we perform Mann–Whitney U test on mono and poly technology groups, we would get the same results.

Statistically all groups are different. How about practically? To anwsers this question the some statisctis were calculated for cost per KW. 

```
def percentile_75(x):
    return np.percentile(x,75)
def percentile_25(x):
    return np.percentile(x,25)
df_rep.groupby('Module Technology #1')['cost_per_KW_with_rebate'].agg([np.mean, np.median, percentile_25, percentile_75, 'count'])

```

| Module Technology          | Mean | Median | P25| P75 | Count    |
|----------------|-------------|----------|-----------|-----------|--------|
| CIGS           | 4543.995769 | 4149.680 | 2901.1700 | 5627.3750 | 891    |
| CIS            | 4126.262222 | 3984.200 | 3115.4500 | 5526.1100 | 81     |
| CdTe           | 6728.226571 | 6539.185 | 5414.0800 | 8882.4975 | 630    |
| Mono           | 4151.998150 | 4055.220 | 3298.2500 | 4931.4350 | 529920 |
| Mono + a-Si    | 4559.878288 | 4021.065 | 3250.0600 | 5767.3200 | 14122  |
| Poly           | 6154.772923 | 4522.500 | 3538.0000 | 5120.0000 | 438113 |
| Thin Film      | 4913.078374 | 4993.580 | 3456.4800 | 6639.1325 | 406    |
| a-Si           | 5651.991894 | 5270.290 | 3539.0025 | 6519.1000 | 322    |
| a-Si + Micro-c | 5141.099281 | 4766.200 | 3628.0200 | 6217.8300 | 139    |
| crystalline    | 4585.426336 | 4400.000 | 3500.0000 | 5256.0000 | 24797  |
| multiple       | 6860.432288 | 4512.360 | 3607.5900 | 5397.7200 | 2443   |

From table above one can observe that distributions are different , especially if considering Mono and Poly we can see say mony is cheaper.

### Efficiencies

The code below is used to mean and median efficiency for technology types from all time data. 
```
df_rep.groupby('Module Technology #1')['Module Efficiency #1'].agg([np.mean,np.median, 'count'])
```
The below is a short analysis on efficiencies. We can observe that Monocrystaline PVs are the most efficent. 

|    Module Technology       | mean | median | count    |
|:--------------:|----------|----------|--------|
|       CIS      | 0.150983 | 0.152381 | 81     |
|      CdTe      | 0.104439 | 0.104167 | 627    |
|      Mono      | 0.181952 | 0.181847 | 496038 |
|   Mono + a-Si  | 0.186594 | 0.194611 | 14029  |
|      Poly      | 0.156783 | 0.158683 | 398734 |
|    Thin Film   | 0.153520 | 0.139860 | 406    |
|      a-Si      | 0.061981 | 0.062000 | 312    |
| a-Si + Micro-c | 0.092638 | 0.094156 | 35     | 
|   CIGS      | 0.117233 | 0.128440 | 862    |
