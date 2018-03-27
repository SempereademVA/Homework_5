

```python
%matplotlib inline
#import definitions
import pandas as pd
import os
import numpy as np
import matplotlib.pyplot as plt

#Set up the data files

# 1. Identify the data file locations and names

cwd =os.getcwd()
path1 = (cwd, 'raw_data', 'clinicaltrial_data.csv')
trial_data= '\\'.join(path1)
trial_data

path2 = (cwd, 'raw_data', 'mouse_drug_data.csv')
mouse_data= '\\'.join(path2)
#trial_data = r'''D:\GWU\HW #5\raw_data\clinicaltrial_data.csv'''     # Location of the school CSV file
#mouse_data = r'''D:\GWU\HW #5\raw_data\mouse_drug_data.csv'''   # Location of the student CSV file
```


```python
# 2.Read in the Data Sets

# Read the CSV into a Pandas DataFrame
trial_df = pd.read_csv(trial_data)
mouse_df = pd.read_csv(mouse_data)
```


```python
# 3.  View the first five rows using the head method
trial_df.head()
```


```python
mouse_df.head()
```


```python
# 4.  Get the dimensions of the DataFrames

trial_df.shape
```


```python
mouse_df.shape
```


```python
# 5.  List data type of each column, number of missing values, and memory usage using the info method

trial_df.info()
```


```python
mouse_df.info()
```


```python
# 6. Check for duplicate data
mouse_df['Mouse ID'].value_counts(normalize=True)
```


```python
# Notice that the g989 appears more frequently.
```


```python
# Eliminate duplicate mouse ids

mouse_dedup_df= mouse_df.drop_duplicates(['Mouse ID'])

# Check results
mouse_dedup_df['Mouse ID'].value_counts(normalize=True)
```


```python
# Notice that the Length is 249 rather than 250.  All ids are the same frequency. One duplicate eliminated.
```


```python
mouse_dedup_df.info()
```


```python
# Notice above there are 249 entries with the index skipping the duplicate id 
```


```python
#  Eliminate any duplicate entries in the trial data
trial_dedup_df= trial_df.drop_duplicates(['Mouse ID','Timepoint'])
```


```python
trial_dedup_df.info()
```


```python
#Notice there are 1888 entries rather than 1893.  The index is an array with 1888 entries, with the duplicate entries gone
check_trial_index = trial_dedup_df.index
check_trial_index
```


```python
#Initialize variables for components of the deduplicated trial dataframe
trial_index = trial_dedup_df.index               # A pandas.core.indexes.range.RangeIndex type.
trial_columns = trial_dedup_df.columns           # A list containing the column names
trial_values = trial_dedup_df.values               # A numpy n-dimensional array
#Initialize variables for components of the desuplicated mouse dataframe
mouse_index = mouse_dedup_df.index             # A pandas.core.indexes.range.RangeIndex type. 
mouse_columns = mouse_dedup_df.columns         # A list containing the column names
mouse_values = mouse_dedup_df.values           # A numpy n-dimensional array
```


```python
number_of_trials = len(trial_index)         # Inialize the number of trials 
number_of_mice = len(mouse_index)      # Initialize the number of mice
drug_list = ('Stelasyn','Naftisol','Ketapril','Capomulin','Infubinol','Ceftamin','Propriva','Zoniferol','Ramicane','Placebo')
number_of_drugs =len(drug_list)

```


```python
trial_data_df = trial_dedup_df
mouse_data_df = mouse_dedup_df
```


```python
volume_df=pd.DataFrame()
for i in range(len(mouse_values)):
    mouse_trial_df = trial_data_df.loc[trial_dedup_df['Mouse ID'] == mouse_values[i,0],:]
    volume_df = volume_df.append(mouse_trial_df)
volume_df
```


```python
process_df=volume_df.pivot(index='Mouse ID',columns='Timepoint',values='Tumor Volume (mm3)')
process_df.head()
```


```python
#Add a new column to the DataFrame called Drug
process_df['Drug'] ='n/a'
process_df
```


```python
#Now find the Drug that was used to treat each mouse

# Start by looking at the mouse DataFrame and locate what drug the mouse was treated with and then 
# add then value to our working DataFrame.
#
x = mouse_data_df.set_index('Mouse ID')

for i in x.index:
    process_df.loc[i,'Drug']= x.loc[i,'Drug']
```


```python
process_df.head()
```


```python
# Sort by Drug
sorted_results=process_df.sort_values(['Drug'])
sorted_results.head(25)
```


```python
summary_volume_dict ={}
summary_volume_dict['Timepoint'] = [0, 5, 10, 15, 20, 25, 30, 35, 40, 45]
for i in drug_list:
    x_drug_df = drug_prep_df=sorted_results.loc[sorted_results['Drug'] == str(i),:]
    x_drug_results_df = x_drug_df.drop('Drug',1)
    drug_median_series = x_drug_results_df.median()
    drug_median_values_lst = drug_median_series.values.tolist()
    summary_volume_dict[i] = drug_median_values_lst 
```


```python
summary_volume_df= pd.DataFrame.from_dict(summary_volume_dict)
summary_volume_df.tail()
```


```python
#One way to generate a scatterplot
plt.style.use('ggplot')
x_limit = 50
x_axis = np.arange(0,x_limit,5)
fig, ax = plt.subplots()
ax.set_xlabel('Time')
ax.set_ylabel('Capomulin')
ax.grid(True)
ax.set_title("Capomulin Vs. Time")
Capomulin_scatter_plot = ax.scatter(x_axis,summary_volume_df['Capomulin'])

plt.show()
```


```python
#Another way to generate a scatterplot
summary_volume_df.plot(kind="scatter", x= 'Timepoint', y="Capomulin", grid=True, figsize=(20,10),
              title="Capomulin Vs. Time")

plt.show()
```


```python
#Seaborn makes better looking plots than matplotlib
#I will use Seaborn for the rest of the visualizations
```


```python
# library & dataset
import seaborn as sns
df = summary_volume_df

fig, ax = plt.subplots(figsize=(20,10))

# use the function regplot to make a scatterplot
sns.regplot(x=df["Timepoint"], y=df["Placebo"], label= "Placebo")
sns.regplot(x=df["Timepoint"], y=df["Ketapril"],label= "Ketapril")
sns.regplot(x=df["Timepoint"], y=df["Capomulin"],label= "Capomulin")
sns.regplot(x=df["Timepoint"], y=df["Infubinol"], label= "Infubinol")
ax.set_title("Median Volume Vs. Time")
ax.set_xlabel('Time (Days)')
ax.set_ylabel("Median Tumor Volume ")
ax.legend()
# Without regression fit:
#sns.regplot(x=df["Timepoint"], y=df["Placebo"], fit_reg=False)
plt.savefig('Tumor Volume.png')
```


```python
########################################### End of Step 1 #####################################################################
```


```python
########################################### Start of Step 2 #####################################################################
```


```python
# Look at the number of metastic sites.

sites_df = trial_data_df[['Mouse ID','Timepoint','Metastatic Sites']]
sites_df.head()
#Build a trial DataFrame, sorted by Mouse ID, not Timepoint 
```


```python
#Build a trial DataFrame, sorted by Mouse ID, for successive values of Timepoint 
sorted_sites_df = sites_df.sort_values(['Mouse ID', 'Timepoint'])
sorted_sites_df.head(25)
```


```python
#Now pivot the DataFrame so we see the effects of the treatment over the time interval
process_df=sorted_sites_df.pivot(index='Mouse ID',columns='Timepoint',values='Metastatic Sites')
process_df.head()
```


```python
#Add a new column to the DataFrame called Drug
process_df['Drug'] ='n/a'
process_df.head()
```


```python
#Now find the Drug that was used to treat each mouse

# Start by looking at the mouse DataFrame and locate what drug the mouse was treated with and then 
# add then value to our working DataFrame.
#
x = mouse_data_df.set_index('Mouse ID')

for i in x.index:
    process_df.loc[i,'Drug']= x.loc[i,'Drug']
process_df.head()
```


```python
# Sort by Drug
sorted_results_df=process_df.sort_values(['Drug'])
sorted_results_df.head()
```


```python
#Build the parts of the DataFrame to contain the median values for each Drug
#It will be built with a list of dictionaries.
#The first dictionary in the list of dictionaries will be the column containing Timepoint values for our median values DataFrame
#The subsequent dictionaries in the list will contain the site values for each Timepoint in successive order

summary_sites_dict ={}
summary_sites_dict['Timepoint'] = [0, 5, 10, 15, 20, 25, 30, 35, 40, 45]     #Column headings
for i in drug_list:
    drug_prep_df = sorted_results_df.loc[sorted_results['Drug'] == str(i),:]  #Create a data frame for each drug
    x_drug_df = drug_prep_df                                                  
    x_drug_results_df = x_drug_df.drop('Drug',1)                    #Drop the Drug column. Major axis = 1. The column axis
    drug_median_series = x_drug_results_df.median()                 #Compute the median of each Timepoint column for all the mice
    drug_median_values_lst = drug_median_series.values.tolist()     #The values for the median are a series. Make it a list.
    summary_sites_dict[i] = drug_median_values_lst                 #The next dictionary in our list contains the median values
```


```python
summary_sites_dict
```


```python
summary_sites_df= pd.DataFrame.from_dict(summary_sites_dict)
summary_sites_df
```


```python
#summary_sites_df.set_index('Timepoint')
#summary_sites_df= summary_sites_df.set_index('Timepoint')
```


```python
# library & dataset
import seaborn as sns
df = summary_sites_df

fig, ax = plt.subplots(figsize=(20,10))


# use the function regplot to make a scatterplot
sns.regplot(x=df["Timepoint"], y=df["Placebo"], label= "Placebo")
sns.regplot(x=df["Timepoint"], y=df["Ketapril"],label= "Ketapril")
sns.regplot(x=df["Timepoint"], y=df["Capomulin"],label= "Capomulin")
sns.regplot(x=df["Timepoint"], y=df["Infubinol"], label= "Infubinol")
ax.legend()
ax.set_title("Median Metastatic Sites Vs. Time")
ax.set_xlabel('Time (Days)')
ax.set_ylabel("Median Metastatic Sites ")
# Without regression fit:
#sns.regplot(x=df["Timepoint"], y=df["Placebo"], fit_reg=False)
plt.savefig('Metastatic_Sites Volume.png')
```


```python
########################################### End of Step 2 #####################################################################
```


```python
###############################  Start of Step 3 Survival Rate ########################################################
```


```python
process_df
```


```python
x_df=process_df.loc[process_df['Drug'] == 'Zoniferol',:]
```


```python
x_df
```


```python
y_df=x_df.describe()
y_df
```


```python
type(y_df)
```


```python
z_series=y_df.iloc[0,:]
z_series
```


```python
type(z_series)
```


```python
summary_survival_dict ={}
summary_survival_dict['Timepoint'] = [0, 5, 10, 15, 20, 25, 30, 35, 40, 45]     #Column headings
for i in drug_list:
    drug_prep_df = sorted_results_df.loc[sorted_results['Drug'] == str(i),:]  #Create a data frame for each drug
    x_drug_df = drug_prep_df                                                  
    x_drug_results_df = x_drug_df.drop('Drug',1)                    #Drop the Drug column. Major axis = 1. The column axis
    drug_survival_series = x_drug_results_df.count()                 #Compute the median of each Timepoint column for all the mice
    drug_survival_values_lst = drug_survival_series.values.tolist()     #The values for the median are a series. Make it a list.
    summary_survival_dict[i] = drug_survival_values_lst 
```


```python
summary_survival_dict
```


```python
summary_survival_df= pd.DataFrame.from_dict(summary_survival_dict)
summary_survival_df=summary_survival_df.set_index('Timepoint')
summary_survival_df
```


```python
#survival_rate_series = summary_survival_df.apply(np.mean)
#survival_rate_series
plotting_df = summary_survival_df
```


```python
# library & dataset
import seaborn as sns
df = plotting_df

fig, ax = plt.subplots(figsize=(20,10))
x_limit = 50
x_axis = np.arange(0,x_limit,5)

plot_index = plotting_df.index
# use the function regplot to make a scatterplot
sns.regplot(x=x_axis, y=df["Placebo"], label= "Placebo")
sns.regplot(x=x_axis, y=df["Ketapril"],label= "Ketapril")
sns.regplot(x=x_axis, y=df["Capomulin"],label= "Capomulin")
sns.regplot(x=x_axis, y=df["Infubinol"], label= "Infubinol")
ax.legend()
ax.set_title("Survival Number Over Time")
ax.set_xlabel('Time (Days)')
ax.set_ylabel("Number of Survivors ")
# Without regression fit:
#sns.regplot(x=df["Timepoint"], y=df["Placebo"], fit_reg=False)
plt.savefig('Survival_number.png')
```


```python
########################################### End of Step 3 #####################################################################
```


```python
########################################### Start of Step 4 % Total Volume Change         ######################################
```


```python
summary_volume_df
```


```python
analysis_df = summary_volume_df[['Capomulin','Infubinol','Ketapril','Placebo','Timepoint']]
analysis_df
```


```python


#summary_volume_df = summary_volume_df.set_index('Timepoint')

#summary_volume_series = summary_volume_df['Capomulin']
#summary_volume_series

analysis_volume_df = analysis_df.set_index('Timepoint')
analysis_volume_df
```


```python
analysis_list = ('Capomulin','Infubinol','Ketapril', 'Placebo')
change_volume_series=pd.Series([0,0,0,0], index=analysis_list )
for i in analysis_list:
    analysis_volume_series = analysis_volume_df[i]
    change_volume_series[i]= 100*(analysis_volume_series[45] - analysis_volume_series[0])/analysis_volume_series[0]
```


```python
#summary_volume_series=pd.Series([0,0,0,0,0,0,0,0,0,0,], index=drug_list)
change_volume_series
```


```python
type(change_volume_series)
```


```python
#change_volume_series[]
```


```python
#plot_change_vol_series['Ketapril']  = change_volume_series['Ketapril']
#plot_change_vol_series['Ketapril']                                          #,'Capomulin','Infubinol','Placebo']
```


```python
#sns.barplot(x='FinYear', y='Amount', data=invYr)
```


```python
#neg_series=pd.Series([-17,-22])
```


```python
#neg_change_series=pd.Series([])
#pos_change_series=pd.Series([])
```


```python
#pos_change_series
```


```python
#neg_change_series.append(pos_change_series)
#neg_change_series
```


```python
#s1=pd.Series([])
#s2=pd.Series([1,2])
#s1.append(change_volume_series)
#s1
```


```python
#s1
```


```python
#for i in drug_list:
#    if change_volume_series[i] <0:
#        s1.append(change_volume_series[i])
#    else:
#        s2.append(change_volume_series[i])
```


```python
#pd.neg_change_series
```


```python
#y_axis = change_volume_series
#x_axis= np.arange(len(change_volume_series))
#plt.bar(x_axis, y_axis, color ='r', alpha=0.5, align='edge')
```


```python
#y_axis = change_volume_series
#x_axis= np.arange(len(change_volume_series))
#for i in range(len(change_volume_series)):
#    if y_axis[i] <0:
#        plt.bar(x_axis, y_axis[i], color ='r', alpha=0.5, align='edge')
#    else:
#        plt.bar(x_axis, y_axis[i], color ='g', alpha=0.5, align='edge')
```


```python
change_volume_lst=change_volume_series.tolist()

```


```python
labels=analysis_list
labels
change_record=[change_volume_lst]
change_record
```


```python

change_volume_df = pd.DataFrame.from_records(change_record, columns=labels)
change_volume_df
```


```python

#x= change_volume_df.columns

#y=change_volume_df.iloc[0,:]

#sns.axes_style('white')
#sns.set_style('white')

# Sets the y limits of the current chart
#plt.ylim(0, max(y)+20)
#plt.subplots(figsize=(20,10))
#plt.title("% Tumor Volume Change for 45 day trial")
#plt.xlabel("Drug")
#plt.ylabel("X")
#plt.subplots(figsize=(15,15))
#colors = ['red' if _y >=0 else 'green' for _y in y]
#ax = sns.barplot(x, y, palette=colors)
```


```python
y=change_volume_df.iloc[0,:]
#Dataset
bars= change_volume_df.columns
height=change_volume_df.iloc[0,:]

y_pos = np.arange(len(bars))

plt.subplots(figsize=(5,5))
# Create bars and choose color
colors = ['red' if _y >=0 else 'green' for _y in y]
plt.bar(y_pos, height, color = colors)
 
# Add title and axis names
plt.title('% Tumor Volume Change for 45 day trial')
plt.xlabel('Drug')
plt.ylabel('% Tumor Volume Change')
 
# Limits for the Y axis
plt.ylim(-40,70)
 
# Create names
plt.xticks(y_pos, bars)
plt.savefig('Percent Volume Change.png') 
# Show graphic
plt.show()

```
