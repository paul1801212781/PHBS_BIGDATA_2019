# Big Data Analysis Assignment 1
## Yongqiao Chen 1801212781
### Research Question
For a PKUSZ student, which city in China is most livable after graduation?

### Methodology
  To answer the question, I intend to scrape the wage of different kinds of jobs and housing information (such as price and amenity) in each major city on the internet. The 50th, 75th and 90th quantile of annual income (as expected income for PKUSZ graduates) for each major can be computed in each city. By setting the affordability constraint of total housing price to annual income ratio, feasible houses can be selected. With the data about amenity and the utility survey on the students, I attempt to compute the score for each affordable house. Most livable city is the one with highest average score. Details of methodology is revealed in the following workflow and steps description.
  
### Big Data Properties
  **Volume:** job data and housing data in 19 cities is supposed to be collected. In each city, more than 1500 jobs and more than 5000 houses are available online. Additionally, multiple aspects of data are contained in one piece of the data. For example, I intend to collect wage, location for job and price, location and amenities for housing. Therefore, the data is of large size.
  
  **Velocity**: these data is updated every hour.
  
  **Variety**: a wide range of data will be collected in this project. For example, in job information set, job name, wage and location will be collected. As for housing information set, price, size, location and amenity is of our interest. 
  
### Workflow
![Image of workflow](http://note.youdao.com/noteshare?id=aa1f757e3d568d7b2f4ac67eae7f60b2)
