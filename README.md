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

