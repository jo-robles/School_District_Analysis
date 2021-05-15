# School District Analysis

## Overview 

In this project, student testing data was taken from 15 schools contained within a single school-district. After cleaning and reviewing the data, analysis was performed on the dataset to create a series of metrics that could be utilized to guide overall strategy and spending allocations. However, in the course of analysis, information came forward that the math and reading scores for the ninth grade students at Thomas High School had been altered. Therefore, a second analysis was conducted removing the math and reading scores for the ninth-graders for Thomas High School to determine the impact and scope of the potential academic dishonesty. 

## Resources  Utilized

* Data Soucres (Resource Folder):
    * schools_complete.csv - CSV file containing information about the schools in the school district (Name, type, size and budget)
    * students_complete.csv - CSV file containing all student information from the school district (Name, gender, grade, school name, reading score and math score)
* Software Utilized: Python 3.7.6, Anaconda, Jupyter Notebook

## Results:

Before a secondary analysis could be completed, it was necessary to remove the ninth-graders math and reading scores from the student_data_df DataFrame created from the data sources present within the Resources folder. To do so, the following method was utilized after cleaning the datasources:
```
student_data_df.loc[(student_data_df['school_name'] == 'Thomas High School') & (student_data_df['grade'] == "9th"),'reading_score'] = np.nan
    
student_data_df.loc[(student_data_df['school_name'] == 'Thomas High School') & (student_data_df['grade'] == "9th"),'math_score'] = np.nan
```
As indicated in the code above, the loc attribute was utilized in the student_data_df DataFrame to find rows where the school's name was "Thomas High School" and the grade was "9th". From there, the reading (or math score) was referenced and ```np.nan``` was utilized to change the value to NaN to ensure that the score was removed. 

After this step was completed, it was necessary to recalculate the averages based on the new number of students that would be accounted for in Thomas High School. To begin, we calculated the number of ninth graders in Thomas High School:

```
thomas_ninth = len(school_data_complete_df.loc[(school_data_complete_df['school_name'] == 'Thomas High School') & (school_data_complete_df['grade'] == '9th')])

```
We once again utilized the loc attributed to locate the data that met our criteria and len to determine the number of rows of students that met that criteria - which happened to be 461 total students. After this step, we simply removed this set of students from a new total student count to determine the passing math, reading and overall rates within the school district. One important item to note here is that when calculating the number of students without Thomas High School in it, we did not change the ```student_count``` but instead created a new variable. As the district still contains the same number of students (regardless of academic integrity), for the analysis to be correct, we still had to include the total number of students within our summary. We did so by doing the following:

```
# Get the total student count 
student_count = school_data_complete_df["Student ID"].count()

# Subtract the number of students that are in ninth grade at Thomas High School from the total student count to get the new total student count.

new_student_count = student_count - thomas_ninth
```
After new average scores were calculated utilizing the updated student count, we were able to aggregate our data and compile it once in the course of the secondary analysis to answer the following questions:

* How was the district summary affected?
    * Originally, our district summary looked like:
    
    INSERT IMAGE HERE
    
    After the updated analysis, the following is observed:
    
    INSERT IMAGE HERE
    
    As demonstrated, the overall average math and reading scores changed very little and the same is true for the percentage of students passing within the three categories. 
    
* How was the school summary affected?
    * The school summary is where we can immediately see the impact of the removal of the ninth-grade math and reading scores. Here we can see the old school summary (Thomas High School highlighted):
    
    INSERT IMAGE HERE
    
    And now the new school summary (Thomas High School highlighted):
    
    We we can see, there is a dramatic difference in the passing percentages between the two DataFrames. In terms of an analysis of the data itself, it seems that a large portion of ninth-grade students did not actually pass their math and reading sections (which, when removed, impacted the percentage of passing students dramatically). We raise two questions based on this information:
    
        1) Based on the impact of the removal of ninth-grade reading and math scores, it appears as though many of the scores removed were less than passing. If students were altering their scores to improve them,  in the ninth grade, A very large number of students in the ninth grade had actually not passed 
        2) 

* How did replacing the ninth graders' math and reading scores affect Thomas High School's performance relative to the other schools?
* How did replacing the ninth grade math and reading scores affect the following:
    * Math and reading scores by grade
    * Scores by school spending
    * Scores by school size
    * Scores by school type

* The top performing school based on overall percentage of students who passed math and reading - Cabrera High School
* The lowest performing school based on overall percentage of students who passed math and reading - Rodriguez High School



## Summary
Summarizing four changes to the school district analysis after reading and math scores have been replaced.
