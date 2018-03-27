

```python
#import definitions
import pandas as pd
import os
import numpy as np
from prettypandas import PrettyPandas


#Set up the data files

#First identify the data file locations and names
#school_data = r'''C:\Users\Marc\Desktop\GW Class\HW #4\raw_data\schools_complete.csv'''     # Location of the school CSV file
#student_data = r'''C:\Users\Marc\Desktop\GW Class\HW #4\raw_data\students_complete.csv'''   # Location of the student CSV file

school_data = r'''C:\Users\Marc\Desktop\GW Class\HW #4\Staging\raw_data\schools_complete.csv'''     # Location of the school CSV file
student_data = r'''C:\Users\Marc\Desktop\GW Class\HW #4\Staging\raw_data\students_complete.csv'''   # Location of the student CSV file

# Read the CSV into a Pandas DataFrame
school_df = pd.read_csv(school_data)
student_df = pd.read_csv(student_data)

#Initialize variables for components of the school dataframe
school_index = school_df.index               # A pandas.core.indexes.range.RangeIndex type. RangeIndex(start=0, stop=15, step=1)
school_columns = school_df.columns           # A list containing the column names
school_data = school_df.values               # A numpy n-dimensional array

#Initialize variables for components of the student dataframe
student_index = student_df.index             # A pandas.core.indexes.range.RangeIndex type. RangeIndex(start=0, stop=39170, step=1
student_columns = student_df.columns         # A list containing the column names
student_data = student_df.values             # A numpy n-dimensional array

number_of_schools = len(school_data)         # Inialize the number of schools 

number_of_students = len(student_index)      # Initialize the number of students


total_budget = school_df['budget'].sum()     # The total budget for all the schools. (Budget for the entire school system)
                                             # Sums the budget column in the school dataframe

average_math_score = student_df['math_score'].mean()  # Average math score for a student in the school system
                                                      # The mean of the values in the math scores column in the data frame

average_reading_score = student_df['reading_score'].mean()    # Average reading score for a student in the school system
                                                              # Mean of the values in the reading scores column in the data frame
#Constants
Passing_Score = 65             # Establishes the value for a passing score.  Passing is defined by this number.
    

########################################### Step 1: Create a District Summary ###############################################

# The approach here is to create a dictionary of lists which contain the district summary data.  These lists will contain
# one element.

################################################
# Compute the percent of students passing math #
################################################

# Initialize the variables
total = 0
number_passing_math = 0
number_passing_reading = 0
number_passing_math =0

Location_of_Reading_Score = 5  # Location 5 in the student_data array. Determined by the CSV file. (First location is 0) 
Location_of_Math_Score = 6     # Location 6 in the student_data array Determine by the CSV file. (First location is 0)


# Loop through all the students have count the number who have grades greater than or equal to the passing grades
# Note:  student_data is a numpy multi-dimensional array 
for i in range(number_of_students):
    if student_data[i,Location_of_Reading_Score] >= Passing_Score:  # Check to see if the student passed
        number_passing_reading = number_passing_reading + 1         # Increment the total number of students passing reading
    if student_data[i,Location_of_Math_Score] >= Passing_Score:     # Check to see if the student passed
        number_passing_math = number_passing_math + 1               # Increment the total number of students passing math
    

#Compute perecentages of students passing reading and math
percent_passing_math = float(number_passing_math)/float(number_of_students) * 100

percent_passing_reading = float(number_passing_reading)/(number_of_students) * 100
#print('Percent Passing Reading: ' + str(percent_passing_reading))
#print('Percent Passing Math: ' + str(percent_passing_math))

#Computer percent overall passing  The average of the passing averages
percent_overall_passing = float((number_passing_math + number_passing_reading))/float(2*number_of_students) * 100

#print('Percent Overall Passing Rate: ' + str(percent_overall_passing))


##################################################
# Create a dataframe to hold the summary data    #
# for Step 1 of the homework- Distict Summary    #
#                                                #
# The dataframe will be created from a dictioary #
# of lists.  The key values of the dictionary    #
# are the column headings of the dataframe.      # 
# The values corresponding to the keys are in a  #
# single value list.                             #
##################################################

summary_dict = {
    'Total Schools': [number_of_schools],
    'Total_Students':[number_of_students],
    'Total Budget':[total_budget],
    'Average Math Score': [average_math_score],
    'Average Reading Score':[average_reading_score],
    '%Passing Math':[percent_passing_math],
    '% Passing Reading':[percent_passing_reading],
    '% Overall Passing Rate': [percent_overall_passing]}

school_snapshot_df = pd.DataFrame(data = summary_dict, index = ['Summary'])

# The dataframe needs to be reordered with the desire order for the column headings.
#  Using the reindex method on the dataframe

school_snapshot_df = school_snapshot_df.reindex_axis(['Total Schools','Total_Students','Total Budget',
                                                      'Average Math Score','Average Reading Score','%Passing Math',
                                                      '% Passing Reading','% Overall Passing Rate'], axis=1)

#Display the dataframe


       
school_snapshot_df

PrettyPandas(school_snapshot_df).as_currency(subset='Total Budget')     #Use PrettyPandas
########################################################################################################################
#This is an alternate approach to computing the percentages of students passing.  It uses values directly out of the 
#the student dataframe
#
#total = 0
#number_passing_math = 0
#number_passing_reading = 0
#number_passing_math =0
#for i in range(number_of_students):
#    if student_df.iloc[i,5] >= 65:
#        number_passing_reading = number_passing_reading + 1
#    if student_df.iloc[i,6] >= 65:
#        number_passing_math = number_passing_math + 1
#    total = total + 1

########################################### End of Step 1 District Summary  ################################################

# 
```

    C:\Users\Marc\Anaconda3\lib\site-packages\pandas\formats\style.py:6: FutureWarning: Styler has been moved from pandas.formats.style.Styler to pandas.io.formats.style.Styler. This shim will be removed in pandas 0.21
      FutureWarning)
    




<style  type="text/css" >
    #T_ec0345f0_2bee_11e8_9808_1cb72ce72c22 th {
          background: #eee;
          font-weight: 500;
    }    #T_ec0345f0_2bee_11e8_9808_1cb72ce72c22 td {
          text-align: right;
          min-width: 3em;
    }    #T_ec0345f0_2bee_11e8_9808_1cb72ce72c22 * {
          border-color: #c0c0c0;
    }</style>  
<table id="T_ec0345f0_2bee_11e8_9808_1cb72ce72c22" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Schools</th> 
        <th class="col_heading level0 col1" >Total_Students</th> 
        <th class="col_heading level0 col2" >Total Budget</th> 
        <th class="col_heading level0 col3" >Average Math Score</th> 
        <th class="col_heading level0 col4" >Average Reading Score</th> 
        <th class="col_heading level0 col5" >%Passing Math</th> 
        <th class="col_heading level0 col6" >% Passing Reading</th> 
        <th class="col_heading level0 col7" >% Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_ec0345f0_2bee_11e8_9808_1cb72ce72c22level0_row0" class="row_heading level0 row0" >Summary</th> 
        <td id="T_ec0345f0_2bee_11e8_9808_1cb72ce72c22row0_col0" class="data row0 col0" >15</td> 
        <td id="T_ec0345f0_2bee_11e8_9808_1cb72ce72c22row0_col1" class="data row0 col1" >39170</td> 
        <td id="T_ec0345f0_2bee_11e8_9808_1cb72ce72c22row0_col2" class="data row0 col2" >$24,649,428.00</td> 
        <td id="T_ec0345f0_2bee_11e8_9808_1cb72ce72c22row0_col3" class="data row0 col3" >78.9854</td> 
        <td id="T_ec0345f0_2bee_11e8_9808_1cb72ce72c22row0_col4" class="data row0 col4" >81.8778</td> 
        <td id="T_ec0345f0_2bee_11e8_9808_1cb72ce72c22row0_col5" class="data row0 col5" >84.7281</td> 
        <td id="T_ec0345f0_2bee_11e8_9808_1cb72ce72c22row0_col6" class="data row0 col6" >96.1986</td> 
        <td id="T_ec0345f0_2bee_11e8_9808_1cb72ce72c22row0_col7" class="data row0 col7" >90.4634</td> 
    </tr></tbody> 
</table> 




```python
######################################### Start of Step 2  School Summary#######################################################

# The following code provides the school summary.
# For the solution in Step 2 the summary DataFrame for display will be constructed from a Dictionary of Lists.
# To that end the following method will be used on 
# the DataFrame:  pd.DataFrame(data = "dictionary of lists", index = "list of index values") 

#Define the column headings
column_headings = ['School Type', 'Total Students', 'Total School Budget', 'Per Student Budget', 'Average Math Score',
                   'Average Reading Score', '% Passing Math', '% Passing Reading', '% Overall Passing' ]

# Create the list of index values.  Each element in the list is a school.
# One way to create the list of index values using the dataframe and the method .tolist()
school_name_list = school_df['name'].tolist()

# An alternate way to build this list is to loop through all the school_data which that is in a two dimensional array. This is
# more work
#school_summary = {}
#school_index =[]
#for i in range(number_of_schools):
#    school_index.append(school_data[i,1])
#        
#school_index

# Create the list of school types
# This approach uses the numpy n-dimensional array called school_data. To that end, values are copied from the array 
# and appended to the list.  This list will then be a value in the dictionary for "School Type".
school_type_list =[]
Column_location_for_types = 2
for i in range(number_of_schools):
    school_type_list.append(school_data[i,Column_location_for_types])

# Create the dictionary and add the list of school types

school_summary_dict={}  # Start with an empty dictionary  This is the dictionary that will contain school summary
school_summary_dict[column_headings[0]] = school_type_list

# Create the list of school sizes
# This is a different (alternate) approach than the one used to create the school type list.  This approach works directly on
# the school dataframe.  If one uses the .values method an array with all the  values for a particular column is returned.  
# Therefore, one must use the .tolist() method to get a list of all the values.
# More detail:
#              my_list = school_df["size"].values
#              my_list contains this:  array([2917, 2949, 1761, 4635, 1468, 2283, 1858, 4976,  427,  962, 1800,
#                                      3999, 4761, 2739, 1635], dtype=int64) 
#           It's not a list so it can't be put in the dictionary,  Need to use .tolist()

#Add the list of total students for each school to the dictionary for the next value.

school_df['size'].tolist()
school_summary_dict[column_headings[1]]= school_df['size'].tolist()

#The dictionary has now grown to two key value pairs. Now adding the third. 
school_df['budget'].tolist()
school_summary_dict[column_headings[2]]= school_df['budget'].tolist()
```


```python
# The remaing lists to be added to the dictionary are the result of additonal computations on the values in the dataframes
# Compute the Per Student Budget for each school.  The values for each school need to be added to a list.
```


```python
# To determine the Per Student Budget divide budget by total students.
# Zip and list comprehension enables the division of two lists.
a = school_summary_dict[column_headings[2]]
b = school_summary_dict[column_headings[1]]
#c = [(x*1.0)/y for x, y in zip(a,b)]
school_summary_dict[column_headings[3]] =  [(x*1.0)/y for x, y in zip(a,b)]
```


```python
# Reorder the dataframe with the school names as the new index 
reorder_student_df = student_df.set_index('school')
```


```python
#school_average = reorder_student_df.loc['Thomas High School', 'math_score'].mean()
```


```python
#Create the list of average math scores
#Locate all the math scores for each school using .loc and take the mean of all the scores with .mean()
math_list =[]
for i in range(number_of_schools):
    school_average = reorder_student_df.loc[school_name_list[i], 'math_score'].mean()
    math_list.append(school_average)  
```


```python
#Now add the list of average math scores to the dictionary
school_summary_dict[column_headings[4]] = math_list
```


```python
#Create the list of average reading scores
#Locate all the reading scores for each school using .loc and take the mean of all the scores with .mean()
reading_list =[]
for i in range(number_of_schools):
    school_average = reorder_student_df.loc[school_name_list[i], 'reading_score'].mean()
    reading_list.append(school_average)
```


```python
#Now add the list of average reading scores to the dictionary
school_summary_dict[column_headings[5]] = reading_list
```


```python
#Create the list of percent passing math
math_passing_list =[]
students_passing_math = 0

for i in range(number_of_schools):
    # Need to reorder the student data fram with the key being the school name and one being the reading scores
    x_series= reorder_student_df.loc[school_name_list[i], 'math_score'] #Returns a pandas.core.series.Series for x_series
    total_students_school = len(x_series)    
    y = x_series.values     # y is a numpy.ndarray  We went from a series to an array 
    y = y[y >= Passing_Score]  # Create a series that only has reading scores that have passing grades
    students_passing_math = len(y)
    percent_students_passsing_math = float(students_passing_math/total_students_school) *100
    math_passing_list.append(percent_students_passsing_math)

#Add the percent passing math to the dictionary
school_summary_dict[column_headings[6]] = math_passing_list

#Create the list of percent passing reading

reading_passing_list =[]
students_passing_reading = 0

for i in range(number_of_schools):
    # Need to reorder the student data fram with the key being the school name and one being the reading scores
    x_series= reorder_student_df.loc[school_name_list[i], 'reading_score'] #Returns a pandas.core.series.Series for x_series
    total_students_school = len(x_series)
    y = x_series.values     # y is a numpy.ndarray  
    y = y[y >= Passing_Score]  # Create a series that only has reading scores that have passing grades
    students_passing_reading = len(y)
    percent_students_passsing_reading = float(students_passing_reading/total_students_school) *100
    reading_passing_list.append(percent_students_passsing_reading)

#Add the percent passing reading to the dictionary
school_summary_dict[column_headings[7]] = reading_passing_list

# To determine the Percent Overall Passing Rate an average of the percent math and percent reading needs to be performed.
# Once agian zip and list comprehension enables the division of two lists.
a = school_summary_dict[column_headings[6]]
b = school_summary_dict[column_headings[7]]
#c = [(x*1.0)/y for x, y in zip(a,b)]
school_summary_dict[column_headings[8]] =  [(((x*1.0) + y)/2) for x, y in zip(a,b)]

step2_df = pd.DataFrame(data= school_summary_dict, index=school_name_list)
step2_df = step2_df.reindex_axis([column_headings[0], column_headings[1], column_headings[2],column_headings[3],
                                      column_headings[4],column_headings[5],column_headings[6],column_headings[7],
                                      column_headings[8]], axis=1)
step2_df


PrettyPandas(step2_df).as_currency(subset=['Total School Budget','Per Student Budget'])
########################################### End of Step 2 #####################################################################
#
```




<style  type="text/css" >
    #T_ece3df1a_2bee_11e8_a154_1cb72ce72c22 th {
          background: #eee;
          font-weight: 500;
    }    #T_ece3df1a_2bee_11e8_a154_1cb72ce72c22 td {
          text-align: right;
          min-width: 3em;
    }    #T_ece3df1a_2bee_11e8_a154_1cb72ce72c22 * {
          border-color: #c0c0c0;
    }</style>  
<table id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row0" class="row_heading level0 row0" >Huang High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row0_col0" class="data row0 col0" >District</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row0_col1" class="data row0 col1" >2917</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row0_col2" class="data row0 col2" >$1,910,635.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row0_col3" class="data row0 col3" >$655.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row0_col4" class="data row0 col4" >76.6294</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row0_col5" class="data row0 col5" >81.1827</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row0_col6" class="data row0 col6" >77.7168</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row0_col7" class="data row0 col7" >94.4806</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row0_col8" class="data row0 col8" >86.0987</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row1" class="row_heading level0 row1" >Figueroa High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row1_col0" class="data row1 col0" >District</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row1_col1" class="data row1 col1" >2949</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row1_col2" class="data row1 col2" >$1,884,411.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row1_col3" class="data row1 col3" >$639.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row1_col4" class="data row1 col4" >76.7118</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row1_col5" class="data row1 col5" >81.158</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row1_col6" class="data row1 col6" >77.1787</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row1_col7" class="data row1 col7" >94.5405</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row1_col8" class="data row1 col8" >85.8596</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row2" class="row_heading level0 row2" >Shelton High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row2_col0" class="data row2 col0" >Charter</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row2_col1" class="data row2 col1" >1761</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row2_col2" class="data row2 col2" >$1,056,600.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row2_col3" class="data row2 col3" >$600.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row2_col4" class="data row2 col4" >83.3595</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row2_col5" class="data row2 col5" >83.7257</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row2_col6" class="data row2 col6" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row2_col7" class="data row2 col7" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row2_col8" class="data row2 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row3" class="row_heading level0 row3" >Hernandez High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row3_col0" class="data row3 col0" >District</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row3_col1" class="data row3 col1" >4635</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row3_col2" class="data row3 col2" >$3,022,020.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row3_col3" class="data row3 col3" >$652.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row3_col4" class="data row3 col4" >77.2898</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row3_col5" class="data row3 col5" >80.9344</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row3_col6" class="data row3 col6" >77.7346</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row3_col7" class="data row3 col7" >94.6063</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row3_col8" class="data row3 col8" >86.1704</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row4_col1" class="data row4 col1" >1468</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row4_col2" class="data row4 col2" >$917,500.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row4_col3" class="data row4 col3" >$625.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row4_col4" class="data row4 col4" >83.3515</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row4_col5" class="data row4 col5" >83.8168</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row4_col6" class="data row4 col6" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row4_col7" class="data row4 col7" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row4_col8" class="data row4 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row5" class="row_heading level0 row5" >Wilson High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row5_col0" class="data row5 col0" >Charter</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row5_col1" class="data row5 col1" >2283</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row5_col2" class="data row5 col2" >$1,319,574.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row5_col3" class="data row5 col3" >$578.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row5_col4" class="data row5 col4" >83.2742</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row5_col5" class="data row5 col5" >83.9895</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row5_col6" class="data row5 col6" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row5_col7" class="data row5 col7" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row5_col8" class="data row5 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row6" class="row_heading level0 row6" >Cabrera High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row6_col0" class="data row6 col0" >Charter</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row6_col1" class="data row6 col1" >1858</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row6_col2" class="data row6 col2" >$1,081,356.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row6_col3" class="data row6 col3" >$582.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row6_col4" class="data row6 col4" >83.0619</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row6_col5" class="data row6 col5" >83.9758</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row6_col6" class="data row6 col6" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row6_col7" class="data row6 col7" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row6_col8" class="data row6 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row7" class="row_heading level0 row7" >Bailey High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row7_col0" class="data row7 col0" >District</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row7_col1" class="data row7 col1" >4976</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row7_col2" class="data row7 col2" >$3,124,928.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row7_col3" class="data row7 col3" >$628.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row7_col4" class="data row7 col4" >77.0484</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row7_col5" class="data row7 col5" >81.034</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row7_col6" class="data row7 col6" >77.914</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row7_col7" class="data row7 col7" >94.5539</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row7_col8" class="data row7 col8" >86.2339</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row8" class="row_heading level0 row8" >Holden High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row8_col0" class="data row8 col0" >Charter</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row8_col1" class="data row8 col1" >427</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row8_col2" class="data row8 col2" >$248,087.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row8_col3" class="data row8 col3" >$581.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row8_col4" class="data row8 col4" >83.8033</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row8_col5" class="data row8 col5" >83.815</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row8_col6" class="data row8 col6" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row8_col7" class="data row8 col7" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row8_col8" class="data row8 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row9_col0" class="data row9 col0" >Charter</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row9_col1" class="data row9 col1" >962</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row9_col2" class="data row9 col2" >$585,858.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row9_col3" class="data row9 col3" >$609.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row9_col4" class="data row9 col4" >83.8399</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row9_col5" class="data row9 col5" >84.0447</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row9_col6" class="data row9 col6" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row9_col7" class="data row9 col7" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row9_col8" class="data row9 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row10" class="row_heading level0 row10" >Wright High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row10_col0" class="data row10 col0" >Charter</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row10_col1" class="data row10 col1" >1800</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row10_col2" class="data row10 col2" >$1,049,400.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row10_col3" class="data row10 col3" >$583.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row10_col4" class="data row10 col4" >83.6822</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row10_col5" class="data row10 col5" >83.955</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row10_col6" class="data row10 col6" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row10_col7" class="data row10 col7" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row10_col8" class="data row10 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row11" class="row_heading level0 row11" >Rodriguez High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row11_col0" class="data row11 col0" >District</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row11_col1" class="data row11 col1" >3999</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row11_col2" class="data row11 col2" >$2,547,363.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row11_col3" class="data row11 col3" >$637.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row11_col4" class="data row11 col4" >76.8427</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row11_col5" class="data row11 col5" >80.7447</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row11_col6" class="data row11 col6" >77.9445</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row11_col7" class="data row11 col7" >94.6237</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row11_col8" class="data row11 col8" >86.2841</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row12" class="row_heading level0 row12" >Johnson High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row12_col0" class="data row12 col0" >District</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row12_col1" class="data row12 col1" >4761</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row12_col2" class="data row12 col2" >$3,094,650.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row12_col3" class="data row12 col3" >$650.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row12_col4" class="data row12 col4" >77.0725</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row12_col5" class="data row12 col5" >80.9664</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row12_col6" class="data row12 col6" >77.9668</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row12_col7" class="data row12 col7" >94.476</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row12_col8" class="data row12 col8" >86.2214</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row13" class="row_heading level0 row13" >Ford High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row13_col0" class="data row13 col0" >District</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row13_col1" class="data row13 col1" >2739</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row13_col2" class="data row13 col2" >$1,763,916.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row13_col3" class="data row13 col3" >$644.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row13_col4" class="data row13 col4" >77.1026</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row13_col5" class="data row13 col5" >80.7463</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row13_col6" class="data row13 col6" >78.2037</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row13_col7" class="data row13 col7" >93.8664</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row13_col8" class="data row13 col8" >86.035</td> 
    </tr>    <tr> 
        <th id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22level0_row14" class="row_heading level0 row14" >Thomas High School</th> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row14_col0" class="data row14 col0" >Charter</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row14_col1" class="data row14 col1" >1635</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row14_col2" class="data row14 col2" >$1,043,130.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row14_col3" class="data row14 col3" >$638.00</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row14_col4" class="data row14 col4" >83.4183</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row14_col5" class="data row14 col5" >83.8489</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row14_col6" class="data row14 col6" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row14_col7" class="data row14 col7" >100</td> 
        <td id="T_ece3df1a_2bee_11e8_a154_1cb72ce72c22row14_col8" class="data row14 col8" >100</td> 
    </tr></tbody> 
</table> 




```python
#########################################  Start of Step 3 ###################################################################
```


```python
# Determine the 5 top scoring schools based on % Overall Passing. This can be obtained by sorting the % Overall Passing
# column in the school summary dataframe and taking looking at the first 5 rows in the head.  In the event of a tie, then
# sort on the individual subject scores.

top_sorted_df = step2_df.sort_values(by=[column_headings[8]],ascending = False)

# Look at the top sorted datafram

top_sorted_df

# There are 8 schools tied with the 100% overall passing. 
top_schools_df = top_sorted_df.head(8)

# There are the top schools:
top_schools_df

PrettyPandas(top_schools_df).as_currency(subset=['Total School Budget','Per Student Budget'])
```




<style  type="text/css" >
    #T_ed09983a_2bee_11e8_ad03_1cb72ce72c22 th {
          background: #eee;
          font-weight: 500;
    }    #T_ed09983a_2bee_11e8_ad03_1cb72ce72c22 td {
          text-align: right;
          min-width: 3em;
    }    #T_ed09983a_2bee_11e8_ad03_1cb72ce72c22 * {
          border-color: #c0c0c0;
    }</style>  
<table id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22level0_row0" class="row_heading level0 row0" >Shelton High School</th> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row0_col0" class="data row0 col0" >Charter</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row0_col1" class="data row0 col1" >1761</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row0_col2" class="data row0 col2" >$1,056,600.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row0_col3" class="data row0 col3" >$600.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row0_col4" class="data row0 col4" >83.3595</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row0_col5" class="data row0 col5" >83.7257</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row0_col6" class="data row0 col6" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row0_col7" class="data row0 col7" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row0_col8" class="data row0 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22level0_row1" class="row_heading level0 row1" >Griffin High School</th> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row1_col0" class="data row1 col0" >Charter</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row1_col1" class="data row1 col1" >1468</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row1_col2" class="data row1 col2" >$917,500.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row1_col3" class="data row1 col3" >$625.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row1_col4" class="data row1 col4" >83.3515</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row1_col5" class="data row1 col5" >83.8168</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row1_col6" class="data row1 col6" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row1_col7" class="data row1 col7" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row1_col8" class="data row1 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22level0_row2" class="row_heading level0 row2" >Wilson High School</th> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row2_col0" class="data row2 col0" >Charter</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row2_col1" class="data row2 col1" >2283</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row2_col2" class="data row2 col2" >$1,319,574.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row2_col3" class="data row2 col3" >$578.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row2_col4" class="data row2 col4" >83.2742</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row2_col5" class="data row2 col5" >83.9895</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row2_col6" class="data row2 col6" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row2_col7" class="data row2 col7" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row2_col8" class="data row2 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22level0_row3" class="row_heading level0 row3" >Cabrera High School</th> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row3_col0" class="data row3 col0" >Charter</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row3_col1" class="data row3 col1" >1858</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row3_col2" class="data row3 col2" >$1,081,356.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row3_col3" class="data row3 col3" >$582.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row3_col4" class="data row3 col4" >83.0619</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row3_col5" class="data row3 col5" >83.9758</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row3_col6" class="data row3 col6" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row3_col7" class="data row3 col7" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row3_col8" class="data row3 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22level0_row4" class="row_heading level0 row4" >Holden High School</th> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row4_col1" class="data row4 col1" >427</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row4_col2" class="data row4 col2" >$248,087.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row4_col3" class="data row4 col3" >$581.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row4_col4" class="data row4 col4" >83.8033</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row4_col5" class="data row4 col5" >83.815</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row4_col6" class="data row4 col6" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row4_col7" class="data row4 col7" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row4_col8" class="data row4 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22level0_row5" class="row_heading level0 row5" >Pena High School</th> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row5_col0" class="data row5 col0" >Charter</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row5_col1" class="data row5 col1" >962</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row5_col2" class="data row5 col2" >$585,858.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row5_col3" class="data row5 col3" >$609.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row5_col4" class="data row5 col4" >83.8399</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row5_col5" class="data row5 col5" >84.0447</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row5_col6" class="data row5 col6" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row5_col7" class="data row5 col7" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row5_col8" class="data row5 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22level0_row6" class="row_heading level0 row6" >Wright High School</th> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row6_col0" class="data row6 col0" >Charter</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row6_col1" class="data row6 col1" >1800</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row6_col2" class="data row6 col2" >$1,049,400.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row6_col3" class="data row6 col3" >$583.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row6_col4" class="data row6 col4" >83.6822</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row6_col5" class="data row6 col5" >83.955</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row6_col6" class="data row6 col6" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row6_col7" class="data row6 col7" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row6_col8" class="data row6 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22level0_row7" class="row_heading level0 row7" >Thomas High School</th> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row7_col0" class="data row7 col0" >Charter</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row7_col1" class="data row7 col1" >1635</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row7_col2" class="data row7 col2" >$1,043,130.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row7_col3" class="data row7 col3" >$638.00</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row7_col4" class="data row7 col4" >83.4183</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row7_col5" class="data row7 col5" >83.8489</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row7_col6" class="data row7 col6" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row7_col7" class="data row7 col7" >100</td> 
        <td id="T_ed09983a_2bee_11e8_ad03_1cb72ce72c22row7_col8" class="data row7 col8" >100</td> 
    </tr></tbody> 
</table> 




```python
# Sort the top schools by math and reading scores
tie_breaker_df = top_schools_df.sort_values(by=[column_headings[4], column_headings[5]], ascending = False)
tie_breaker_df =tie_breaker_df.head()
tie_breaker_df
PrettyPandas(tie_breaker_df).as_currency(subset=['Total School Budget','Per Student Budget'])
```




<style  type="text/css" >
    #T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22 th {
          background: #eee;
          font-weight: 500;
    }    #T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22 td {
          text-align: right;
          min-width: 3em;
    }    #T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22 * {
          border-color: #c0c0c0;
    }</style>  
<table id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22level0_row0" class="row_heading level0 row0" >Pena High School</th> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row0_col0" class="data row0 col0" >Charter</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row0_col1" class="data row0 col1" >962</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row0_col2" class="data row0 col2" >$585,858.00</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row0_col3" class="data row0 col3" >$609.00</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row0_col4" class="data row0 col4" >83.8399</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row0_col5" class="data row0 col5" >84.0447</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row0_col6" class="data row0 col6" >100</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row0_col7" class="data row0 col7" >100</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row0_col8" class="data row0 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22level0_row1" class="row_heading level0 row1" >Holden High School</th> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row1_col0" class="data row1 col0" >Charter</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row1_col1" class="data row1 col1" >427</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row1_col2" class="data row1 col2" >$248,087.00</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row1_col3" class="data row1 col3" >$581.00</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row1_col4" class="data row1 col4" >83.8033</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row1_col5" class="data row1 col5" >83.815</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row1_col6" class="data row1 col6" >100</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row1_col7" class="data row1 col7" >100</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row1_col8" class="data row1 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22level0_row2" class="row_heading level0 row2" >Wright High School</th> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row2_col0" class="data row2 col0" >Charter</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row2_col1" class="data row2 col1" >1800</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row2_col2" class="data row2 col2" >$1,049,400.00</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row2_col3" class="data row2 col3" >$583.00</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row2_col4" class="data row2 col4" >83.6822</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row2_col5" class="data row2 col5" >83.955</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row2_col6" class="data row2 col6" >100</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row2_col7" class="data row2 col7" >100</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row2_col8" class="data row2 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22level0_row3" class="row_heading level0 row3" >Thomas High School</th> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row3_col0" class="data row3 col0" >Charter</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row3_col1" class="data row3 col1" >1635</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row3_col2" class="data row3 col2" >$1,043,130.00</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row3_col3" class="data row3 col3" >$638.00</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row3_col4" class="data row3 col4" >83.4183</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row3_col5" class="data row3 col5" >83.8489</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row3_col6" class="data row3 col6" >100</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row3_col7" class="data row3 col7" >100</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row3_col8" class="data row3 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22level0_row4" class="row_heading level0 row4" >Shelton High School</th> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row4_col1" class="data row4 col1" >1761</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row4_col2" class="data row4 col2" >$1,056,600.00</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row4_col3" class="data row4 col3" >$600.00</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row4_col4" class="data row4 col4" >83.3595</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row4_col5" class="data row4 col5" >83.7257</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row4_col6" class="data row4 col6" >100</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row4_col7" class="data row4 col7" >100</td> 
        <td id="T_ed1795ec_2bee_11e8_9a50_1cb72ce72c22row4_col8" class="data row4 col8" >100</td> 
    </tr></tbody> 
</table> 




```python
########################################### End of Step 3 #####################################################################
#

#########################################  Start of Step 4 ###################################################################

#
# Determine the 5 lowest scoring schools based on % Overall Passing. This can be obtained by sorting the % Overall Passing
# column in the school summary dataframe and taking looking at the first 5 rows in the head.  In the event of a tie, then
# sort on the individual subject scores.

bottom_sorted_df = step2_df.sort_values(by=[column_headings[8]])

# Look at the bottom sorted datafram

bottom_sorted_df =bottom_sorted_df.head()

bottom_sorted_df   # The worst school is at the top of the list
PrettyPandas(bottom_sorted_df).as_currency(subset=['Total School Budget','Per Student Budget'])
```




<style  type="text/css" >
    #T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22 th {
          background: #eee;
          font-weight: 500;
    }    #T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22 td {
          text-align: right;
          min-width: 3em;
    }    #T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22 * {
          border-color: #c0c0c0;
    }</style>  
<table id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22level0_row0" class="row_heading level0 row0" >Figueroa High School</th> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row0_col0" class="data row0 col0" >District</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row0_col1" class="data row0 col1" >2949</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row0_col2" class="data row0 col2" >$1,884,411.00</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row0_col3" class="data row0 col3" >$639.00</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row0_col4" class="data row0 col4" >76.7118</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row0_col5" class="data row0 col5" >81.158</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row0_col6" class="data row0 col6" >77.1787</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row0_col7" class="data row0 col7" >94.5405</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row0_col8" class="data row0 col8" >85.8596</td> 
    </tr>    <tr> 
        <th id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22level0_row1" class="row_heading level0 row1" >Ford High School</th> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row1_col0" class="data row1 col0" >District</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row1_col1" class="data row1 col1" >2739</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row1_col2" class="data row1 col2" >$1,763,916.00</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row1_col3" class="data row1 col3" >$644.00</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row1_col4" class="data row1 col4" >77.1026</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row1_col5" class="data row1 col5" >80.7463</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row1_col6" class="data row1 col6" >78.2037</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row1_col7" class="data row1 col7" >93.8664</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row1_col8" class="data row1 col8" >86.035</td> 
    </tr>    <tr> 
        <th id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22level0_row2" class="row_heading level0 row2" >Huang High School</th> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row2_col0" class="data row2 col0" >District</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row2_col1" class="data row2 col1" >2917</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row2_col2" class="data row2 col2" >$1,910,635.00</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row2_col3" class="data row2 col3" >$655.00</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row2_col4" class="data row2 col4" >76.6294</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row2_col5" class="data row2 col5" >81.1827</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row2_col6" class="data row2 col6" >77.7168</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row2_col7" class="data row2 col7" >94.4806</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row2_col8" class="data row2 col8" >86.0987</td> 
    </tr>    <tr> 
        <th id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22level0_row3" class="row_heading level0 row3" >Hernandez High School</th> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row3_col0" class="data row3 col0" >District</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row3_col1" class="data row3 col1" >4635</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row3_col2" class="data row3 col2" >$3,022,020.00</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row3_col3" class="data row3 col3" >$652.00</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row3_col4" class="data row3 col4" >77.2898</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row3_col5" class="data row3 col5" >80.9344</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row3_col6" class="data row3 col6" >77.7346</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row3_col7" class="data row3 col7" >94.6063</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row3_col8" class="data row3 col8" >86.1704</td> 
    </tr>    <tr> 
        <th id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22level0_row4" class="row_heading level0 row4" >Johnson High School</th> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row4_col0" class="data row4 col0" >District</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row4_col1" class="data row4 col1" >4761</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row4_col2" class="data row4 col2" >$3,094,650.00</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row4_col3" class="data row4 col3" >$650.00</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row4_col4" class="data row4 col4" >77.0725</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row4_col5" class="data row4 col5" >80.9664</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row4_col6" class="data row4 col6" >77.9668</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row4_col7" class="data row4 col7" >94.476</td> 
        <td id="T_ed2fb700_2bee_11e8_acaf_1cb72ce72c22row4_col8" class="data row4 col8" >86.2214</td> 
    </tr></tbody> 
</table> 




```python
########################################### End of Step 4 #####################################################################
```


```python
###############################  Start of Step 5 Math Scores by Grade ########################################################
#
```


```python
#Create a dataframe with the data equal to a dictionary of lists with average reading scores for each grade. 
#
#Define the column headings:

column_headings = ['9th', '10th', '11th', '12th' ]

# Create the list of index values.  Each element in the list is a school.
# One way to create the list of index values using the dataframe and the method .tolist()
#school_name_list = school_df['name'].tolist()   #This will be the index for the step 5 dataframe. Created in Step 2

#
# Need to Reorder the dataframe with the school names as the new index.  Already done in step 2 
# Use reorder_student_df that was created from step 2 reorder_student_df= student_df.set_index('school')

#Create the dataframe of schools and grades
#Locate all the math scores for each school using .loc and take the mean of all the scores with .mean()

#Create a dictionary of lists for mean scores for math for each grade

grade_math_means_dict={}
for j in column_headings:   #Loop through all the grades.  Create a dataframe for each grade with the scores
    grade_x_df = reorder_student_df.loc[reorder_student_df['grade'] ==j,:]  #Dataframe of all students in the grade j
    
    #Determine mean of the math scores for the particular grade for each school (i) for the grade (j)
    math_list = []
    for i in range(number_of_schools):
        school_average_x = grade_x_df.loc[school_name_list[i], 'math_score'].mean()
        math_list.append(school_average_x)
    grade_math_means_dict[j] = math_list
    
step5_df = pd.DataFrame(data= grade_math_means_dict, index=school_name_list)
step5_df = step5_df.reindex_axis([column_headings[0], column_headings[1], column_headings[2], column_headings[3]], axis=1)
step5_df
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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
  </tbody>
</table>
</div>




```python
########################################### End of Step 5 #####################################################################
```


```python
###############################  Start of Step 6 Reading Scores by Grade ########################################################
#
```


```python
#Create a dataframe with the data equal to a dictionary of lists with average reading scores for each grade. 
#
#Define the column headings
# Use the same column headings from Step 5
#column_headings = ['9th', '10th', '11th', '12th' ]

# Create the list of index values.  Each element in the list is a school.
# One way to create the list of index values using the dataframe and the method .tolist()

# school_name_list = school_df['name'].tolist()   #This will be the index for the step 6 dataframe.  Created in step 2
#
# Create a dictionary with reading scores by grade for each school
#


# Need to Reorder the dataframe with the school names as the new index.  Already done in step 2 
# Use reorder_student_df that was created from step 2 reorder_student_df= student_df.set_index('school')

#Create the dataframe of schools and grades
#Locate all the math scores for each school using .loc and take the mean of all the scores with .mean()

#Create a dictionary of lists for mean scores for reading for each grade

grade_reading_means_dict={}
for j in column_headings:   #Loop through all the grades.  Create a dataframe for each grade with the scores
    grade_x_df = reorder_student_df.loc[reorder_student_df['grade'] ==j,:]  #Dataframe of all students in the grade j
    
    #Determine mean of the reading score for the particular grade for each school (i) for the grade (j)
    reading_list =[]
    for i in range(number_of_schools):
        school_average_x = grade_x_df.loc[school_name_list[i], 'reading_score'].mean()
        reading_list.append(school_average_x)

    grade_reading_means_dict[j] = reading_list
    
step6_df = pd.DataFrame(data= grade_reading_means_dict, index=school_name_list)
step6_df = step6_df.reindex_axis([column_headings[0], column_headings[1], column_headings[2], column_headings[3]], axis=1)
step6_df


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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
  </tbody>
</table>
</div>




```python
########################################### End of Step 6 #####################################################################
```


```python
###############################  Start of Step 7 Scores by Spending ########################################################
#
```


```python
column_headings = ['Average Math Score', 'Average Reading Score', '% Passing Math', '% Passing Reading', '% Overall Passing' ]

spending_index = [ 'Extreme (> $650)',  'High ($625-$650)', 'Medium ($600-$625)', 'Low (< $600)']

school_spending_dict ={}    #The dictionary of lists that contains the average scores for the three spending categories
average_score_list = []     #The list of scores for each spending category

low_df = step2_df.loc[step2_df['Per Student Budget'] < 600,:]  #Dataframe of all low student spending

criteria_1 = step2_df['Per Student Budget'] >= 600
criteria_2 = step2_df['Per Student Budget'] < 625
criteria_all = criteria_1 & criteria_2
med_df = step2_df.loc[criteria_all,:]  

criteria_1 = step2_df['Per Student Budget'] >= 625
criteria_2 = step2_df['Per Student Budget'] < 650
criteria_all = criteria_1 & criteria_2
high_df = step2_df.loc[criteria_all,:]

extreme_df = step2_df.loc[step2_df['Per Student Budget'] >= 650,:]
extreme_df
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>77.716832</td>
      <td>94.480631</td>
      <td>86.098732</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>77.734628</td>
      <td>94.606257</td>
      <td>86.170442</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>77.966814</td>
      <td>94.475950</td>
      <td>86.221382</td>
    </tr>
  </tbody>
</table>
</div>




```python
low_df
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
for i in column_headings:

    average_score_list=[]
    average_score = extreme_df.loc[:, i].mean()
    average_score_list.append(average_score)


    average_score = high_df.loc[:, i].mean()
    average_score_list.append(average_score)


    average_score = med_df.loc[:, i].mean()
    average_score_list.append(average_score)


    average_score = low_df.loc[:, i].mean()
    average_score_list.append(average_score)

    
    school_spending_dict[i]= average_score_list
```


```python
step7_df = pd.DataFrame(data= school_spending_dict, index=spending_index)
step7_df = step7_df.reindex_axis([column_headings[0], column_headings[1], column_headings[2],
                                  column_headings[3],column_headings[4]], axis=1)
step7_df

table = PrettyPandas(step7_df) 
table
```




<style  type="text/css" >
    #T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22 th {
          background: #eee;
          font-weight: 500;
    }    #T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22 td {
          text-align: right;
          min-width: 3em;
    }    #T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22 * {
          border-color: #c0c0c0;
    }</style>  
<table id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >% Overall Passing</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22level0_row0" class="row_heading level0 row0" >Extreme (> $650)</th> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row0_col0" class="data row0 col0" >76.9972</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row0_col1" class="data row0 col1" >81.0278</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row0_col2" class="data row0 col2" >77.8061</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row0_col3" class="data row0 col3" >94.5209</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row0_col4" class="data row0 col4" >86.1635</td> 
    </tr>    <tr> 
        <th id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22level0_row1" class="row_heading level0 row1" >High ($625-$650)</th> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row1_col0" class="data row1 col0" >79.0792</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row1_col1" class="data row1 col1" >81.8914</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row1_col2" class="data row1 col2" >85.2068</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row1_col3" class="data row1 col3" >96.2641</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row1_col4" class="data row1 col4" >90.7354</td> 
    </tr>    <tr> 
        <th id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22level0_row2" class="row_heading level0 row2" >Medium ($600-$625)</th> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row2_col0" class="data row2 col0" >83.5997</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row2_col1" class="data row2 col1" >83.8852</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row2_col2" class="data row2 col2" >100</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row2_col3" class="data row2 col3" >100</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row2_col4" class="data row2 col4" >100</td> 
    </tr>    <tr> 
        <th id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22level0_row3" class="row_heading level0 row3" >Low (< $600)</th> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row3_col0" class="data row3 col0" >83.4554</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row3_col1" class="data row3 col1" >83.9338</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row3_col2" class="data row3 col2" >100</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row3_col3" class="data row3 col3" >100</td> 
        <td id="T_ee3af2da_2bee_11e8_8c36_1cb72ce72c22row3_col4" class="data row3 col4" >100</td> 
    </tr></tbody> 
</table> 




```python
########################################### End of Step 7 #####################################################################
```


```python
###############################  Start of Step 8 Scores by School Size ########################################################
#
```


```python
column_headings = ['Average Math Score', 'Average Reading Score', '% Passing Math', '% Passing Reading', '% Overall Passing' ]


size_index = [ 'Large (> 2500)', 'Medium (1000 - 2500)', 'Low (< 1000)']
school_size_dict ={}    #The dictionary of lists that contains the average scores for the three size categories
average_score_list = []     #The list of scores for eachsize category

low_df = step2_df.loc[step2_df['Total Students'] < 1000,:]

criteria_1 = step2_df['Total Students'] >= 1000
criteria_2 = step2_df['Total Students'] < 2500
criteria_all = criteria_1 & criteria_2
med_df = step2_df.loc[criteria_all,:]  

high_df = step2_df.loc[step2_df['Total Students'] >= 2500,:]
```


```python
for i in column_headings:

    average_score_list=[]
    average_score = high_df.loc[:, i].mean()
    average_score_list.append(average_score)


    average_score = med_df.loc[:, i].mean()
    average_score_list.append(average_score)


    average_score = low_df.loc[:, i].mean()
    average_score_list.append(average_score)
 
    school_size_dict[i]= average_score_list
```


```python
#school_size_dict
step8_df = pd.DataFrame(data= school_size_dict, index=size_index)
step8_df = step8_df.reindex_axis([column_headings[0], column_headings[1], column_headings[2],
                                  column_headings[3],column_headings[4]], axis=1)
step8_df
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Large (&gt; 2500)</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>77.808454</td>
      <td>94.449607</td>
      <td>86.12903</td>
    </tr>
    <tr>
      <th>Medium (1000 - 2500)</th>
      <td>83.357937</td>
      <td>83.885280</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.00000</td>
    </tr>
    <tr>
      <th>Low (&lt; 1000)</th>
      <td>83.821598</td>
      <td>83.929843</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.00000</td>
    </tr>
  </tbody>
</table>
</div>




```python
########################################### End of Step 8 #####################################################################
```


```python
###############################  Start of Step 9 Scores by School Type ########################################################
#
```


```python
#Step 9

column_headings = ['Average Math Score', 'Average Reading Score', '% Passing Math', '% Passing Reading', '% Overall Passing' ]


type_index = [ 'Charter', 'District']
school_type_dict ={}    #The dictionary of lists that contains the average scores for the categories
average_score_list = []     #The list of scores for each category

charter_df = step2_df.loc[step2_df['School Type'] == 'Charter', column_headings]
district_df = step2_df.loc[step2_df['School Type'] == 'District', column_headings]
```


```python
charter_df
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Shelton High School</th>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
district_df
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>77.716832</td>
      <td>94.480631</td>
      <td>86.098732</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>77.178705</td>
      <td>94.540522</td>
      <td>85.859613</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>77.734628</td>
      <td>94.606257</td>
      <td>86.170442</td>
    </tr>
    <tr>
      <th>Bailey High School</th>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>77.913987</td>
      <td>94.553859</td>
      <td>86.233923</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>77.944486</td>
      <td>94.623656</td>
      <td>86.284071</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>77.966814</td>
      <td>94.475950</td>
      <td>86.221382</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>78.203724</td>
      <td>93.866375</td>
      <td>86.035049</td>
    </tr>
  </tbody>
</table>
</div>




```python
for i in column_headings:

    average_score_list=[]
    average_score = charter_df.loc[:, i].mean()
    average_score_list.append(average_score)


    average_score = district_df.loc[:, i].mean()
    average_score_list.append(average_score)

    school_type_dict[i]= average_score_list
```


```python
step9_df = pd.DataFrame(data= school_type_dict, index=type_index)
step9_df = step9_df.reindex_axis([column_headings[0], column_headings[1], column_headings[2],
                                  column_headings[3],column_headings[4]], axis=1)
step9_df
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.00000</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>77.808454</td>
      <td>94.449607</td>
      <td>86.12903</td>
    </tr>
  </tbody>
</table>
</div>


