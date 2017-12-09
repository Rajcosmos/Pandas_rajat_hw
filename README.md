### PyCity Schools Analysis
### Observed following trends
## 1. Schools in Charter are better than Schools in District in terms of overall passing rate.
## 2. High budget per student doesn't mean it will improve the overall passing rate. Charter's per student budget is less than the District's per student budget despite that Charter beats the District schools in overall passing/performance in reading and maths. 
## 3. Wilson High school (which belongs to Charter) has the highest overall passing rate. Best school to join :)

```python
import pandas as pd
import os
```


```python
#os.listdir()
```


```python
# Create a path to csv and read school and Student data
csv_path_schools=os.path.join('Py','schools_complete.csv')
csv_path_students=os.path.join('Py','students_complete.csv')
```


```python
schools_df=pd.read_csv(csv_path_schools)
students_df=pd.read_csv(csv_path_students)

```


```python
#schools_df
```


```python
#students_df.head()
```


```python
# Calculating total Schools and total Students
total_schools=schools_df["name"].count()
total_students=students_df["name"].count()
```


```python
total_budget=schools_df["budget"].sum()
```


```python
avg_math_score=students_df["math_score"].mean()
```


```python
avg_reading_score=students_df["reading_score"].mean()
```


```python
#Assuming passing score of Math and Readings are 70
pass_math=students_df[students_df.math_score>70]
```


```python
total_pass_math=pass_math["math_score"].count()
```

pass_math


```python
pass_reading=students_df[students_df.reading_score>70]
```


```python
total_pass_reading=pass_reading["reading_score"].count()
```


```python
# Calculate % Math and Reading pass 
math_percent=(total_pass_math/total_students)*100
read_percent=(total_pass_reading/total_students)*100
```


```python
overall_pass=(math_percent+read_percent)/2
```


```python
# Creating a new District Summary table consolidating above calculations
District_Summary = pd.DataFrame({"Total Schools":[total_schools],
                                 "Total Students":[total_students],
                                 "Total Budget":[total_budget],
                                "Average Math Score":[avg_math_score],
                                "Average Reading Score":[avg_reading_score],
                                "% Passing Math":[math_percent],
                                "% Passing Reading":[read_percent],
                                "% Overall Passing Rate":[overall_pass]})
```


```python
#District_Summary
```


```python
District_Summary = District_Summary[["Total Schools",
                                     "Total Students",
                                     "Total Budget",
                                     "Average Math Score",
                                     "Average Reading Score",
                                     "% Passing Math",
                                     "% Passing Reading",
                                     "% Overall Passing Rate"]]
```


```python
# Rouding the Score values to 2 digits
District_Summary = District_Summary.round(2)
#District_Summary
```


```python
def dollar(number):
    string='$'+ str(number)
    return string
```


```python
#District_Summary['Total Budget']=District_Summary.apply(lambda x:"{:,}".format(x['Total Budget']),axis=1)
District_Summary["Total Budget"]=District_Summary["Total Budget"].map(dollar)
#df['Value'] = df.apply(lambda x: "{:,}".format(x['Value']), axis=1)
District_Summary
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
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>$24649428</td>
      <td>78.99</td>
      <td>81.88</td>
      <td>72.39</td>
      <td>82.97</td>
      <td>77.68</td>
    </tr>
  </tbody>
</table>
</div>




```python
# WE will merge the two dataframes to 1 data frame by using "school" as common column
# rename the "name" column to "school" column in schools_df
renamed_school_df=schools_df.rename(columns={"name":"school"})
```


```python
# merge the two data frames
merge_school_table=pd.merge(renamed_school_df,students_df,on="school")
```


```python
# Display the merged table
merge_school_table.head()
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
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Calculating passing math and percentage pass in math
math_pass=merge_school_table.loc[merge_school_table["math_score"]>70]
math_pass_groupby=math_pass.groupby('school')
total_math_pass=math_pass_groupby["math_score"].count()
y=merge_school_table.groupby('school')
total_math=y["math_score"].count()
percent_math_pass=pd.DataFrame(total_math_pass/total_math*100)

```


```python
## Calculating passing reading and percentage pass in reading
read_pass=merge_school_table.loc[merge_school_table["reading_score"]>70]
read_pass_groupby=read_pass.groupby('school')
total_read_pass=read_pass_groupby["reading_score"].count()
y=merge_school_table.groupby('school')
total_read=y["reading_score"].count()
percent_read_pass=pd.DataFrame(total_read_pass/total_read*100)
#percent_read_pass
```


```python
# Calculate overall passing rate
overall_pass_rate=((percent_math_pass["math_score"]/100+percent_read_pass["reading_score"]/100)/2)*100
overall=pd.DataFrame({"Overall_pass_rate":overall_pass_rate})

```


```python
#Calculating Per Student Budget
schools_df_index=schools_df.set_index('name')
Per_student_budget=schools_df_index['budget']/schools_df_index['size']
per_stud_budget=pd.DataFrame({"Per Student Budget":Per_student_budget})

```


```python
# Setting index
school_type = schools_df.set_index('name')['type']
budget=schools_df.set_index('name')['budget']
group_by_school=merge_school_table.groupby('school')

### Creating a summary dataframe
School_Summary=pd.DataFrame({
                            "School Type":school_type,
                            "Total School Budget":budget,
                            "Total Students":group_by_school["name"].count(),
                             "Average Math Score":group_by_school["math_score"].mean(),
                             "Average Reading Score":group_by_school["reading_score"].mean(),
                             "% Passing Math":percent_math_pass["math_score"],
                            "% Passing Reading":percent_read_pass["reading_score"],
                           "% Overall Passing Rate":overall["Overall_pass_rate"],
                            "Per student budget":per_stud_budget["Per Student Budget"]
                             
                            })
#School_Summary
organized_school_summary_df = School_Summary[["School Type","Total Students", "Total School Budget","Per student budget","Average Math Score","Average Reading Score","% Passing Math","% Passing Reading","% Overall Passing Rate"]]
organized_school_summary_df.head()
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
      <th>Per student budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>628.0</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>64.630225</td>
      <td>79.300643</td>
      <td>71.965434</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>89.558665</td>
      <td>93.864370</td>
      <td>91.711518</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>644.0</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>65.753925</td>
      <td>77.510040</td>
      <td>71.631982</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>89.713896</td>
      <td>93.392371</td>
      <td>91.553134</td>
    </tr>
  </tbody>
</table>
</div>




```python
########### Calculating top and bottom 5 performing schools based on overall passing rate###########################
```


```python
## like to preserve the previous df by assigning to new df
top_school_df=organized_school_summary_df
```


```python
#top_school_df.head()
```


```python
## Sort df.sort_values(by='col1', ascending=False)
result_top5=top_school_df.sort_values(by='% Overall Passing Rate', ascending=False).head(5)
```


```python
result_top5
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
      <th>Per student budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
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
      <td>90.932983</td>
      <td>93.254490</td>
      <td>92.093736</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>609.0</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>91.683992</td>
      <td>92.203742</td>
      <td>91.943867</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>90.277778</td>
      <td>93.444444</td>
      <td>91.861111</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>89.558665</td>
      <td>93.864370</td>
      <td>91.711518</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>90.632319</td>
      <td>92.740047</td>
      <td>91.686183</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Sort bottom 5
bottom_school_df=organized_school_summary_df
result_bottom5=(bottom_school_df.sort_values(by='% Overall Passing Rate', ascending=True).head(5))
```


```python
result_bottom5
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
      <th>Per student budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>64.066017</td>
      <td>77.744436</td>
      <td>70.905226</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>63.852132</td>
      <td>78.281874</td>
      <td>71.067003</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
    </tr>
  </tbody>
</table>
</div>




```python
############################# Reading Math Scores by grade level################
import numpy as np
```


```python
math_score_by_grade=students_df.pivot_table(index='school',columns='grade',values='math_score',aggfunc=np.mean)
column_order=['9th','10th','11th','12th']
math_score_by_grade=math_score_by_grade.reindex_axis(column_order,axis=1)
math_score_by_grade=math_score_by_grade.round({'9th':2,'10th':2,'11th':2,'12th':2})
math_score_by_grade
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
      <th>grade</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.08</td>
      <td>77.00</td>
      <td>77.52</td>
      <td>76.49</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.09</td>
      <td>83.15</td>
      <td>82.77</td>
      <td>83.28</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.40</td>
      <td>76.54</td>
      <td>76.88</td>
      <td>77.15</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.36</td>
      <td>77.67</td>
      <td>76.92</td>
      <td>76.18</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.04</td>
      <td>84.23</td>
      <td>83.84</td>
      <td>83.36</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.44</td>
      <td>77.34</td>
      <td>77.14</td>
      <td>77.19</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.79</td>
      <td>83.43</td>
      <td>85.00</td>
      <td>82.86</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.03</td>
      <td>75.91</td>
      <td>76.45</td>
      <td>77.23</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.19</td>
      <td>76.69</td>
      <td>77.49</td>
      <td>76.86</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.63</td>
      <td>83.37</td>
      <td>84.33</td>
      <td>84.12</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.86</td>
      <td>76.61</td>
      <td>76.40</td>
      <td>77.69</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.42</td>
      <td>82.92</td>
      <td>83.38</td>
      <td>83.78</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.59</td>
      <td>83.09</td>
      <td>83.50</td>
      <td>83.50</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.09</td>
      <td>83.72</td>
      <td>83.20</td>
      <td>83.04</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.26</td>
      <td>84.01</td>
      <td>83.84</td>
      <td>83.64</td>
    </tr>
  </tbody>
</table>
</div>




```python
#################################### Reading Scores by grade level################
```


```python
reading_score_by_grade=students_df.pivot_table(index='school',columns='grade',values='reading_score',aggfunc=np.mean)
column_order=['9th','10th','11th','12th']
reading_score_by_grade=reading_score_by_grade.reindex_axis(column_order,axis=1)
reading_score_by_grade=reading_score_by_grade.round({'9th':2,'10th':2,'11th':2,'12th':2})
reading_score_by_grade
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
      <th>grade</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.30</td>
      <td>80.91</td>
      <td>80.95</td>
      <td>80.91</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.68</td>
      <td>84.25</td>
      <td>83.79</td>
      <td>84.29</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.20</td>
      <td>81.41</td>
      <td>80.64</td>
      <td>81.38</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.63</td>
      <td>81.26</td>
      <td>80.40</td>
      <td>80.66</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.37</td>
      <td>83.71</td>
      <td>84.29</td>
      <td>84.01</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.87</td>
      <td>80.66</td>
      <td>81.40</td>
      <td>80.86</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.68</td>
      <td>83.32</td>
      <td>83.82</td>
      <td>84.70</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.29</td>
      <td>81.51</td>
      <td>81.42</td>
      <td>80.31</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.26</td>
      <td>80.77</td>
      <td>80.62</td>
      <td>81.23</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.81</td>
      <td>83.61</td>
      <td>84.34</td>
      <td>84.59</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.99</td>
      <td>80.63</td>
      <td>80.86</td>
      <td>80.38</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.12</td>
      <td>83.44</td>
      <td>84.37</td>
      <td>82.78</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.73</td>
      <td>84.25</td>
      <td>83.59</td>
      <td>83.83</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.94</td>
      <td>84.02</td>
      <td>83.76</td>
      <td>84.32</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.83</td>
      <td>83.81</td>
      <td>84.16</td>
      <td>84.07</td>
    </tr>
  </tbody>
</table>
</div>




```python
############################## Scores by School Spending using bins ############################
x=organized_school_summary_df
#x.head()
```


```python
import pandas as pd
def round2(number):
    # Rounds the number
    rounded_number = round(number, 2)
    # Creates a string
    string = str(rounded_number)
    # Returns the string
    return string

x["Average Math Score"]=x["Average Math Score"].map(round2)
x["Average Reading Score"]=x["Average Reading Score"].map(round2)
x["% Passing Math"]=x["% Passing Math"].map(round2)
x["% Passing Reading"]=x["% Passing Reading"].map(round2)
x["% Overall Passing Rate"]=x["% Overall Passing Rate"].map(round2)
#x.columns
```


```python
#x.head()
```


```python
#Create 4 bins
bins=[0,585,615,645,675]
#Create names for the 4 bins
group_names=['< $585','$585-615','$615-645','$645-675']
```


```python
# Cut the x and place the scores into bins
x["Per student budget"] = pd.cut(x["Per student budget"],bins, labels=group_names)

```


```python
score_by_school_spending=x.groupby('Per student budget')
score_by_school_spend=score_by_school_spending.max()
organized_score_by_school_spending=score_by_school_spend[["Average Math Score","Average Reading Score","% Passing Math","% Passing Reading","% Overall Passing Rate"]]
organized_score_by_school_spending
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
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Per student budget</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt; $585</th>
      <td>83.8</td>
      <td>83.99</td>
      <td>90.93</td>
      <td>93.86</td>
      <td>92.09</td>
    </tr>
    <tr>
      <th>$585-615</th>
      <td>83.84</td>
      <td>84.04</td>
      <td>91.68</td>
      <td>92.62</td>
      <td>91.94</td>
    </tr>
    <tr>
      <th>$615-645</th>
      <td>83.42</td>
      <td>83.85</td>
      <td>90.21</td>
      <td>93.39</td>
      <td>91.56</td>
    </tr>
    <tr>
      <th>$645-675</th>
      <td>77.29</td>
      <td>81.18</td>
      <td>64.75</td>
      <td>78.81</td>
      <td>71.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
###################################### Scores by School Size using bins #################################
By_size=pd.DataFrame({"size":merge_school_table["size"].unique(),
                      "Average Math Score":organized_school_summary_df["Average Math Score"],
                      "Average Reading Score":organized_school_summary_df["Average Reading Score"],
                      "% Passing Math":organized_school_summary_df["% Passing Math"],
                    "% Passing Reading":organized_school_summary_df["% Passing Reading"],
                     "% Overall Passing Rate":organized_school_summary_df["% Overall Passing Rate"]})
By_size=By_size[["size","Average Math Score","Average Reading Score","% Passing Math","% Passing Reading","% Overall Passing Rate"]]
#By_size
```


```python
#Create 4 bins
bins=[0,1000,2000,5000]
#Create names for the 4 bins
group_names=['Small(<1000)','Medium(1000-2000)','Large(2000-5000)']
```


```python
# Cut the By_size dataframe and place the size into bins
By_size["size"] = pd.cut(By_size["size"],bins, labels=group_names)
By_school_size=By_size.groupby('size')
By_school_size=By_school_size.max()
By_school_size=By_school_size[["Average Math Score","Average Reading Score","% Passing Math","% Passing Reading","% Overall Passing Rate"]]
By_school_size
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
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small(&lt;1000)</th>
      <td>83.84</td>
      <td>84.04</td>
      <td>91.68</td>
      <td>92.2</td>
      <td>91.94</td>
    </tr>
    <tr>
      <th>Medium(1000-2000)</th>
      <td>83.8</td>
      <td>83.95</td>
      <td>90.63</td>
      <td>93.44</td>
      <td>91.86</td>
    </tr>
    <tr>
      <th>Large(2000-5000)</th>
      <td>83.42</td>
      <td>83.99</td>
      <td>90.93</td>
      <td>93.86</td>
      <td>92.09</td>
    </tr>
  </tbody>
</table>
</div>




```python
###################################### Scores by School Type using bins #################################
By_type=pd.DataFrame({"School type":organized_school_summary_df["School Type"],
                      "Average Math Score":organized_school_summary_df["Average Math Score"],
                      "Average Reading Score":organized_school_summary_df["Average Reading Score"],
                      "% Passing Math":organized_school_summary_df["% Passing Math"],
                    "% Passing Reading":organized_school_summary_df["% Passing Reading"],
                     "% Overall Passing Rate":organized_school_summary_df["% Overall Passing Rate"]})
                    
By_type=By_type[["School type","Average Math Score","Average Reading Score","% Passing Math","% Passing Reading","% Overall Passing Rate"]]
#By_type
```


```python
By_school_type=By_type.groupby('School type')
By_school_type=By_school_type.max()
By_school_type=By_school_type[["Average Math Score","Average Reading Score","% Passing Math","% Passing Reading","% Overall Passing Rate"]]
By_school_type
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
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.84</td>
      <td>84.04</td>
      <td>91.68</td>
      <td>93.86</td>
      <td>92.09</td>
    </tr>
    <tr>
      <th>District</th>
      <td>77.29</td>
      <td>81.18</td>
      <td>65.75</td>
      <td>79.3</td>
      <td>71.97</td>
    </tr>
  </tbody>
</table>
</div>


