# Overview

This project explores the UK data job market with a particular focus on data analyst roles. The research was driven by a genuine interest in pursuing a career in data analytics as a graduate. By looking into the highest-paying positions and the most in-demand skills, the analysis aims to better understand what employers are looking for and to highlight the best opportunities available to aspiring data analysts across the UK.

The data used in this project is sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python), which provides a strong foundation for the analysis, including detailed information on job titles, salaries, locations, and essential skills. Working through his YouTube course also helped develop practical Python skills, which were directly applied throughout this project. Based on this experience, the course is highly recommended to others looking to build or strengthen their Python skills. Using this dataset, the analysis explores key questions such as the most in-demand skills, salary trends, and how skill demand aligns with salary levels within data analytics. 

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying) 

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyse the data and find critical information. I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter UK Jobs

To focus my analysis on the U.K. job market, I apply filters to the dataset, narrowing down to roles based in the United Kingdom.

```python
df_US = df[df['job_country'] == 'United Kingdom']

```

# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting. 

View my notebook with detailed steps here: [2_Skill_Demand](2_Skill_Demand.ipynb).

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().set_visible(False)
    ax[i].set_xlim(0, 75)

    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')
        if i != len(job_titles) - 1:
            ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in UK Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5) # fix the overlap
plt.show()
```

### Results

![Likelihood of Skills Requested in the UK Job Postings](Python_Data_Project/3_Project/images/likelihood_skills_uk.png)



*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

### Insights:

- SQL is the most requested skill for Data Analysts and Data Engineers, with it in over half the job postings for both Data Engineers and over 40% for Data Analysts. For Data Scientists, Python is the most sought-after skill, appearing in 69% of job postings.

- Data Engineers and Data Scientists require more specialised technical skills which include AWS and Azure compared to Data Analysts who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).

- Python is a versatile skill, highly demanded across for Data Engineer (55%) and Data Scientists (69%) but surprisingly low for Data Analysts (20%).

## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_Skills_Trend](3_Skills_Trend.ipynb).

### Visualize Data

```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_UK_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine() # remove top and right spines

plt.title('Trending Top Skills for Data Analysts in the UK')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

# annotate the plot with the top 5 skills using plt.text()
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')

plt.savefig("trending_top_skills_uk.png", dpi=300, bbox_inches="tight")
plt.show()

```

### Results

![Trending Top Skills for Data Analysts in the UK](Python_Data_Project/3_Project/images/trending_top_skills_uk.png)  
*Bar graph visualizing the trending top skills for data analysts in the UK in 2023.*

### Insights:
SQL

- SQL is the most in-demand skill throughout the year, consistently leading all others. Demand peaks in mid and late 2023, showing its continued importance across roles.
Insight: SQL is a non-negotiable core skill for data analysts in the UK.

Excel

- Excel demand remains steady across the year, with only minor fluctuations. Despite newer tools, it continues to be widely requested by employers.
Insight: Excel is still essential, especially for business-focused and reporting tasks.

Python

- Python shows an overall upward trend, with demand increasing through mid to late 2023. This reflects growing emphasis on automation and advanced analysis.
Insight: Python is a strong value-adding skill that helps analysts stand out.

Power BI

- Power BI demand rises notably in the second half of the year, peaking in autumn. This suggests increasing use of dashboarding and BI tools.
Insight: Power BI is a fast-growing skill with strong relevance in the UK market.

Tableau

- Tableau demand is lower than Power BI but shows gradual growth towards the end of the year. It remains relevant in certain organisations.
Insight: Tableau is useful but more niche compared to Power BI in the UK.

## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in the United Kingdom and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most. 

View my notebook with detailed steps here: [4_Salary_Analysis](4_Salary_Analysis.ipynb).

#### Visualize Data 

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

#### Results

![Salary Distributions of Data Jobs in the UK](Python_Data_Project/3_Project/images/trending_top_skills_uk.png) 
*Box plot visualizing the salary distributions for the top 6 data job titles.*

#### Insights

Data Analyst

- Salaries are mostly concentrated in the lower range compared to other roles, with a moderate spread and a few high-end outliers. This suggests some opportunities for higher pay, but they are less common.
Insight: Data analyst roles offer solid entry-level salaries, but pay growth is more limited without progression.

Data Scientist

- The salary distribution is wider, with a higher median and a long upper tail. This indicates greater variability depending on experience, industry, and skill set.
Insight: Data scientist roles provide strong earning potential but show significant salary dispersion.

Data Engineer

- Data engineers show a relatively high median salary and a wide spread, including a very high outlier. This reflects strong demand and premium pay for specialised skills.
Insight: Data engineering is one of the highest-paying non-senior data roles in the UK.

Senior Data Analyst

- The box plot is very thin, indicating a small number of job postings in the dataset. Salaries are tightly clustered around the upper analyst range.
Insight: Senior analyst roles appear less frequently but offer a clear salary step up from standard analyst positions.

Senior Data Engineer

- This plot is also thin, suggesting limited data or fewer advertised roles. Salaries are high, with several outliers, reflecting senior-level responsibility and scarcity.
Insight: Senior data engineers are highly valued, with strong salaries driven by specialised expertise.

Senior Data Scientist

- Salaries are consistently high with a relatively wide interquartile range. This suggests strong demand across industries with variation by organisation.
Insight: Senior data scientist roles offer some of the most competitive salaries in the UK data job market.

### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

#### Visualize Data

```python

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()
# original code:
# df_DA_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False) 
ax[0].set_title('Highest Paid Skills for Data Analysts in the UK')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'£{int(x/1000)}K'))


# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()
# original code:
# df_DA_skills[::-1].plot(kind='barh', y='median', ax=ax[1], legend=False)
ax[1].set_title('Most In-Demand Skills for Data Analysts in the UK')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

sns.set_theme(style='ticks')
plt.tight_layout()
plt.savefig("highest_paid_skills_Data_Analyst_uk.png", dpi=300, bbox_inches="tight")
plt.show()

```

#### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the UK:

![The Highest Paid & Most In-Demand Skills for Data Analysts in the UK](Python_Data_Project/3_Project/images/highest_paid_skills_Data_Analyst_uk.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the UK.*

#### Insights:

Here’s a **clear, structured analysis with key insights**, focusing on what the two charts together reveal about the UK data analyst market.


---

 **Highest-Paid Skills for Data Analysts**

- The top-paying skills are heavily **technical and specialised**, with **C++, TensorFlow, PyTorch, NumPy, and Pandas** commanding the highest median salaries. These skills are more commonly associated with advanced analytics, machine learning, or data engineering tasks rather than traditional analyst roles.
**Insight:** Higher salaries are linked to niche, technically demanding skills that push analysts closer to data science or engineering responsibilities.

---

 **Most In-Demand Skills for Data Analysts**

- The most requested skills are **Tableau, SQL, Looker, Power BI, and Python**, reflecting a strong focus on data querying, visualisation, and business intelligence. These skills are widely applicable across industries and roles.
**Insight:** Employers prioritise practical, business-facing tools over highly specialised technical skills when hiring data analysts.

---

 **Demand vs Salary Trade-Off**

- There is a clear gap between **what pays the most** and **what is most commonly required**. Highly paid skills tend to be less frequently requested, while the most in-demand skills offer more moderate salaries.
**Insight:** The optimal strategy is to master high-demand tools (SQL, BI tools, Python) while selectively adding high-paying skills to differentiate and increase earning potential.

---

 **Graduate-Level Takeaway**

- For graduates entering the UK market, focusing on **SQL, Python, and a BI tool** offers the best employability, while developing advanced technical skills can unlock higher salary ceilings later.
**Insight:** Strong fundamentals open doors; specialised skills drive long-term salary growth.


## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn. 

View my notebook with detailed steps here: [5_Optimal_Skills](5_Optimal_Skills.ipynb).

#### Visualize Data

```python

from adjustText import adjust_text

# Scatter plot
df_DA_skills_high_demand.plot(kind='scatter', x='skill_percent', y='median_salary')

# Prepare texts for adjustText
texts = []  # make sure this matches the variable name
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(
        plt.text(
            df_DA_skills_high_demand['skill_percent'].iloc[i],
            df_DA_skills_high_demand['median_salary'].iloc[i],
            txt  # add the skill name as label
        )
    )

# Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary (£)')
plt.title('Most Optimal Skills for Data Analysts in the UK')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'£{int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
plt.tight_layout()
plt.savefig("most_optimal_skills_uk.png", dpi=300, bbox_inches="tight")
plt.show()

```

#### Results
![Most Optimal Skills for Data Analysts in the UK](Python_Data_Project/3_Project/images/most_optimal_skills_uk.png)    
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the UK.*

#### Insights:

- SQL and Python dominate demand in UK Data Analyst roles, appearing in over a third of job postings, but their median salaries indicate they are baseline requirements rather than differentiators.

- Specialised enterprise and workflow tools (e.g: SQL Server, Flow) command the highest median salaries despite low demand, reflecting a strong premium for niche, senior-level expertise.

- BI and Cloud Platforms (Tableau, Looker, Azure) offer the strongest balance between demand and salary, highlighting their importance in business-facing and production analytics roles.

- Traditional and Office-based tools (Excel, Word and Outlook) show high or moderate demand but significantly lower salaries, indicating limited impact on salary progression.



### Visualizing Different Techonologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

#### Visualize Data

```python

from adjustText import adjust_text

# Scatter plot
# df_DA_skills_high_demand.plot(kind='scatter', x='skill_percent', y='median_salary')
sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

sns.despine()
sns.set_theme(style='ticks')

# Prepare texts for adjustText
texts = []  # make sure this matches the variable name
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(
        plt.text(
            df_DA_skills_high_demand['skill_percent'].iloc[i],
            df_DA_skills_high_demand['median_salary'].iloc[i],
            txt  # add the skill name as label
        )
    )

# Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary (£)')
plt.title('Most Optimal Skills for Data Analysts in the UK')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'£{int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
plt.tight_layout()
plt.savefig("most_optimal_skills_uk_colour.png", dpi=300, bbox_inches="tight")
plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in the UK with Coloring by Technology](Python_Data_Project/3_Project/images/most_optimal_skills_uk_colour.png)  
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the UK with color labels for technology.*

#### Insights:

Programmning

- Programming skills (SQL, Python, R, Go,, VBA) dominate job demand, with SQL and Python appearing in the highest proportion of roles.

- Despite high demand, median salaries are moderate relative to niche tools, indicating programming skills are core requirements rather than salary differentiators.

Analyst Tools

- BI and Analyst Tools (Tableau, Power BI, Looker, Excrl, Sheets) show strong variation in salary outcomes.

- Modern BI platforms such as Tableau and Looker offer higher median salaries, while wiley adopted tools like Excel are highly demanded but associated with lower pay.

Cloud

- Cloud skills (e.g: Azure) appear in a smaller share of job postings but are linked to above average median salaries.

- This suggests cloud expertise enhances analyst roles by supporting production and enterprise data environments.

Databases

- Databases technologies show the highest salary premiums, particularly SQL Server, despite relatively low demand.

- This reflects the value employees have on specialised data infrastructure skills used in enterprise and senior data analyst roles.

Other

- Workflow and general office tools (Flow, Word, Outlook) show low demand overall, with a clear split in value.

- This shows that scarce workflow automation tools reflect skill scarcity, whereas basic office tools are expected and add minimal value to pay.



# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualisation. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualisation, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.


# Insights

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and SQL often lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.


# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data visualisation**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.


# Conclusion

This exploration of the data analyst job market has provided valuable insight into the key skills and trends shaping the field. The findings deepen understanding of the market while offering practical guidance for those looking to build or progress a career in data analytics. As the industry continues to evolve, regularly revisiting and analysing these trends will be crucial to staying competitive. This project serves as a strong foundation for future work and highlights the importance of continuous learning and adaptability within the data profession.
