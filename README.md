# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positons by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here:
[2_skill_Demand.ipynb](3_project\2_Skills_Demand.ipynb)

### Visualize Data

```python

fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in  enumerate(job_titles):
    df_plot = df_skills_count[df_skills_count['job_title_short'] == job_title].head(5)
    df_plot.plot(kind='barh',x='job_skills', y='skill_count', ax=ax[i], title=job_title)
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].legend().set_visible(False)


fig.suptitle('Counts of Top Skills in Job Postings', fontsize=15)
plt.tight_layout(h_pad=0.5)
plt.show()
```
### Results

![visualization of Top Skills for Data Nerds](3_project\images\skill_demand_data_roles.png)

### Insights 

- python is a versatile skill, highly demanded
  across all three roles, but most prominently for
  Data scientists (72%) and Data Engineers (65%).

- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of j0b postings.

- Data Engineerrs require more specialized technical skills ( AWS, Azure, spark) compared to Data Analysts and Data scientists who are expected to be proficient in more general data management and analysis tools
  (Excel, Tableau).


# The Analysis

## 2. How are in-demand skills trending for Data Analysts?

```python
df_plot = df_DA_US_percent.iloc[:, :5]

sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('likelihood in job posting')
plt.xlabel('2023')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax=plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(
        df_plot.index[-1],
        df_plot.iloc[-1, i],
        df_plot.columns[i],
        va='center'
    )

plt.xlim(0, len(df_plot) -1+ 1)
```

### Results


![Trending Top Skills for Data Analysts in the US](3_project\images\skill_trend_DA.png)


*Bar graph visualizing the trending top skills for data analysts in the US in 2023.*

### Insights:

- SQL remains the most consistently demanded skill throughout the year, although is shows a gradual decline
in damand

- Excel experienced a significant increase in demand starting around september, surpassing both python and tableau by the end of the year.

- Both Python and tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for the data analysts.power BI , while less demanded compared to the others, shows a slight upward trend towards the year's end.

# The Analysis

## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis for Data Nerds
```python

sns.boxplot(data=df_US_top6, x= 'salary_year_avg', y='job_title_short', order= job_order)

plt.title ('salary Distribution in the United States')
plt.xlabel('yearly Salary ($USD)')
plt.ylabel('')
ax= plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
plt.xlim(0, 600000)
plt.show()
```
### results
![salary Distributions of Data jobs in the US](3_project\images\salary_boxplot.png)

* Box plot visualizing the salary distributions for the top 6 data job titles.*

### Insights
- there's siginificant variation in salary ranges across different job titles. senior Data scientist positions tend to have the highest salary potential, with up to $600k, indicating the high value placed on advanced data skills and experience in the industry.

- senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewe ouliers.

- the median salaries increase with the seniority and specialization of the roles. senior roles (senior data scientist, senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.


# The Analysis 

## 4. What is the most optimal skill to learn for Data Analysts?

### visualize Data

```python
from adjustText import adjust_text

#df_plot.plot(kind = 'scatter', x= 'skill_percent', y= 'median_salary')
sns.scatterplot(
    data=df_plot,
    x= 'skill_percent',
    y= 'median_salary',
    hue='technology'
)

sns.despine()
sns.set_theme(style='ticks')
texts=[]

for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], txt))

    adjust_text(texts,
                force_text= 0.01,
                    force_static=0.01,
                    force_pull = 1,
                    max_move=(5, 5),
                    arrowprops=dict(arrowstyle="->", color='gray')
    )




from matplotlib.ticker import PercentFormatter

    
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y , pos:f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))


plt.xlabel('percent of Data Analyst Jobs')
plt.ylabel ("median yearly salary ($USD)")
plt.title('most optimal skills for Data Analysts in the US')
plt.tight_layout()
plt.ylim(80000, 99000)
plt.show()
```
![Most optimal skills for Data Analysts in the US](3_project\images\Most_Optimal_Skills_for_Data_Analysts_in_the_US_with_coloring_by_technology.png)

### Insights

- The scatter plot shows that most of the programming skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.

* Analyst tools (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

* The database skills (colored orange), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry.