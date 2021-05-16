# School District Analysis

## Overview 

In this project, student testing data was taken from 15 schools contained within a single school-district. After cleaning and reviewing the data, analysis was performed on the dataset to create a series of metrics that could be utilized to guide overall strategy and spending allocations. However, during analysis, information came forward that the math and reading scores for the ninth-grade students at Thomas High School had been altered. Therefore, a second analysis was conducted removing the math and reading scores for the ninth graders for Thomas High School to determine the impact and scope of the potential academic dishonesty. 

## Resources Utilized

* Data Sources (Resource Folder):
    * schools_complete.csv - CSV file containing information about the schools in the school district (Name, type, size and budget)
    * students_complete.csv - CSV file containing all student information from the school district (Name, gender, grade, school name, reading score and math score)
* Software Utilized: Python 3.7.6, Anaconda 4.10.1, Jupyter Notebook

## Results:

Before a secondary analysis could be completed, it was necessary to remove the ninth-graders math and reading scores from the student_data_df DataFrame created from the data sources present within the Resources folder. To do so, the following method was utilized after cleaning the data sources:
```
student_data_df.loc[(student_data_df['school_name'] == 'Thomas High School') & (student_data_df['grade'] == "9th"),'reading_score'] = np.nan
    
student_data_df.loc[(student_data_df['school_name'] == 'Thomas High School') & (student_data_df['grade'] == "9th"),'math_score'] = np.nan
```
As indicated in the code above, the loc attribute was utilized in the student_data_df DataFrame to find rows where the school's name was "Thomas High School”, and the grade was "9th". From there, the reading (or math score) was referenced and ```np.nan``` was utilized to change the value to NaN to ensure that the score was removed. 

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
As we are concerned about the impact of the passing rates of students, we would utilize this updated student count to determine the new rate of students who passed their reading and math sections. From there, we reviewed the resulting dataframes to answer the following questions:

* How was the district summary affected?
    * Looking at our new district summary, we can assume that the resulting DataFrame has been impacted simply from looking at our school summary. While the resulting averages would not be impacted (as the new averages would simply be calculated from the students in the dataset where data was present), the percentages of passing students would be affected. 
    
    After the updated analysis, the following is observed:
    
    ![New District Summary Analysis](https://github.com/jo-robles/School_District_Analysis/blob/7f53592aec95657ea9b0bc765f71364e7d365c43/Resources/new_district_summary.PNG)
    
    As demonstrated, the overall average math and reading scores changed very little however the percentage of students passing is somewhat impacted.  
    
* How was the school summary affected?
    * The school summary is where we can immediately see the impact of the removal of the ninth-grade math and reading scores. Here we can see the old school summary (Thomas High School highlighted):
    
    ![Old school summary](https://github.com/jo-robles/School_District_Analysis/blob/7f53592aec95657ea9b0bc765f71364e7d365c43/Resources/old_school_summary.PNG)
    
    And now the new school summary (Thomas High School highlighted):
    
    ![New school summary](https://github.com/jo-robles/School_District_Analysis/blob/7f53592aec95657ea9b0bc765f71364e7d365c43/Resources/new_school_summary.PNG)
    
    We can see, there is a dramatic difference in the passing percentages between the two DataFrames. In terms of an analysis of the data itself, it appears as though many of the students in the ninth-grade dataset for Thomas High School may have not met the criteria of 70% or higher. A deeper diver into the dataset (such as a statistical analysis) would be warranted to determine the exact cause however and that would be outside the scope of this analysis. 

* How did replacing the ninth graders' math and reading scores affect Thomas High School's performance relative to the other schools?
    * In this case, by replacing the math and reading scores for Thomas High School in the ninth grade, greatly improved the performance of the school relative to others. For example, now Thomas High School is in the top 5 schools:
    
    ![New top five](https://github.com/jo-robles/School_District_Analysis/blob/7f53592aec95657ea9b0bc765f71364e7d365c43/Resources/new_top_five.PNG)
    
* How did replacing the ninth-grade math and reading scores affect the following:
    * Math and reading scores by grade.
        * To answer this, let's take a look at what the old analysis yielded for the reading scores by grade DataFrame:
    
    ![Old reading by grade](https://github.com/jo-robles/School_District_Analysis/blob/7f53592aec95657ea9b0bc765f71364e7d365c43/Resources/old_reading_by_grade.PNG)
    
    And when we compare this to the new DataFrame:
    
    ![New reading by grade](https://github.com/jo-robles/School_District_Analysis/blob/7f53592aec95657ea9b0bc765f71364e7d365c43/Resources/new_reading_by_grade.PNG)
    
    We see that replacing the scores in the ninth grade for Thomas High School (beyond just not being able to compare them for that grade) did little for the impact. The results are the same for the math scores. Additionally, since we only removed one set of data from the 9th graders, it would stand to reason that other grades would not be impacted.  

    * Scores by school spending
        * In terms of spending by school size, we can assume that while averages would not be impacted (as they simply wouldn't be in the calculation of the averages (as demonstrated earlier)), the passing percentages would be. To demonstrate this, we created the bins and averages of the dataframe:  ```per_school_summary_df``` utilizing the following code:
        
        ```
        # Establish the spending bins and group names.

        spending_bins = [0,585,630,645,675]
        spending_names = ['<$584', '$585-629', '$630-644', '$645-675']

        # Categorize spending based on the bins.
        per_school_summary_df['Spending Ranges (Per Student)'] = pd.cut(per_school_capita, spending_bins, labels=spending_names)
        
        #Averaging based on column
        spending_math_scores = per_school_summary_df.groupby(['Spending Ranges (Per Student)']).mean()['Average Math Score']
        ```
        The results is the following:
        
        ![School spending](https://github.com/jo-robles/School_District_Analysis/blob/7f53592aec95657ea9b0bc765f71364e7d365c43/Resources/new_spending.PNG)
        
        As we know that the overall percentages were impacted from Thomas High School, and further, from the previous dataframe, we know that the school spends $638.00 per student, we can make an assumption that the impact of removing the ninth-graders from Thomas High School which increased the overall passing percentage, we can make the assumption that it will be the spending range - "$630 - 644" that would be the one impacted and that the impact would likely have raised the overall passing percentages. 
        
    * Scores by school size
        * As with the spending categories, we once again had to utilize ```pd.cut``` to categorize our dataframe based on three bins: Small (<1000 students), Medium (1000 - 2000 students) and Large (2000-5000 students). Like how we knew what type of spending Thomas High School has per student, we also know the number of students who attend Thomas High School - 1635. As a result, after creating our dataframe:
        
        ![School size](https://github.com/jo-robles/School_District_Analysis/blob/7f53592aec95657ea9b0bc765f71364e7d365c43/Resources/new_school_size.PNG)
        
        We can assume that it would likely the passing percentages for the medium schools that would likely be impacted by the change in Thomas High School. Further, as we know that the percentages increased for Thomas High School, we can assume that the percentages increased. 
        
    * Scores by school type
        * In this case, we know that Thomas HIgh School is classified as a charter. However, as we already have a "School Type" column in our ```per_school_summary_df``` dataframe, we can simply utilize ```.groupby```. We did so by utilizing the following code:
        
        ```
        new_type_math_scores = per_school_summary_df.groupby(['School Type']).mean()['Average Math Score']
        ```
        This code allowed us to pull out the average math score based on the type of school that was present. We repeated this step on each of the columns of data we needed and compiled this into a dataframe:
        
        ![school type](https://github.com/jo-robles/School_District_Analysis/blob/7f53592aec95657ea9b0bc765f71364e7d365c43/Resources/new_type_summary.PNG)
        
        Once again, as before, we know that since the average passing rates of Thomas High School increased, it stands to reason that the passing percentages of the group that the school belongs to would also be impacted. In this case, it is likely that the Charter school type passing percentages rose. 

## Summary

In conclusion, manipulating and changing the data for Thomas High School would likely lead to the following changes in analysis:

1. The biggest most immediate change to the analysis would be the top 5 schools within the district. When removing the results from the 9th grade, the overall passing percentage rose dramatically. This can certainly lead to a further investigation of the underlying factors of how the 9th grade is doing and, perhaps most importantly, a statistical analysis of data present within the 9th grade testing scores. 
2. Another change that can be observed is in terms of the overall district. While the averages themselves were not impacted much, the passing percentages were. When considering strategy for the school district, understanding the tolerances that they may have towards budget and funding purposes would need to be investigated further. 
3. In consideration for the groups that were presented (e.g., scores by school type) further analysis would be warranted as we consider the impact of Thomas High School on the passing metrics presented. One question that would need to be considered is simply how far does Thomas High School skew the data?
4. Finally, as we already know that the top 5 schools were changed by the data present within Thomas High School, we can also assume that likely the bottom 5 schools have changed as well. If this is the case, we would need to verify this information by comparing the two dataframes to see if this is indeed the case. Finally, if it is, then understanding how this impacts our strategy for the school district itself would require further investigation.
