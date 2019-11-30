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
![QZ3uUf.jpg](https://s2.ax1x.com/2019/11/30/QZ3uUf.jpg)

### Steps
#### 1 Collection of job data in each city (Implemented in the Python course last module)
Data: job name, wage (convert to annual), Company, Location

Job type: Finance, Programmer, Engineer, Lawyer

City: 

Tier 1: Beijing, Shanghai, Guangzhou, Shenzhen. 

Tier 2: Chengdu, Hangzhou, Chongqing, Wuhan, Xi’an, Suzhou, Tianjin, Nanjing, Changsha, Zhengzhou, Dongguan, Qingdao, Shenyang, Ningbo, Kunming

Source: https://www.51job.com/

#### 2 Collect housing information
Data: price (yuan per square meter), size (rooms and area), location, amenity (number of hospitals, schools, shopping malls and bus/Metro stations etc)

Source: https://bj.fang.com/

#### 3 Selection for feasible houses
In each city, I compute the annual income of different quantile and the total value of each house. Feasible houses will be selected if the total value of house to annual income ratio is less than 30 (criterion hasn’t been decided yet).

#### 4 Utility Surveys
In order to measure the total utility of the feasible houses, utility surveys will be conducted among PKUSZ students. Basically, I need to decide the weights for different kinds of amenities.

Definition of each indicator:

Commutation: the distance between job location and house location (with Baidu map api). 

Distance: the distance between house location and city center (with Baidu map api).

Hospital: number of hospital in the neighborhood (the proximity not decided yet).

Shopping malls: number of shopping malls in the neighborhood (the proximity not decided yet).

Restaurants: number of restaurants in the neighborhood (the proximity not decided yet).

#### 5 Score estimation
With the parameter estimated in the utility surveys, I can compute a score for each feasible house.

#### 6&7 Decision making
After calculating the average score of feasible houses, city of the highest average score is most livable. 

### Database choice: I will choose relational database since the data is structural in the form of table. 
