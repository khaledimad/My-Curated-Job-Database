# My-Curated-Job-Database
Looking for jobs like a Data Analyst

The current options for finding an analytics job today are inefficient and scattered. A job seeker
must search for multiple job titles across several job websites in a process that is very manual and timeconsuming.

To solve this problem, our team researched and identified the best job websites and scraped them
for four key analytics and data science roles across the top eleven job markets in the US. We then cleaned,
combined, and ordered this data into a repository using the DynamoDB database. The result was one
unified job database that contained more than 25000 highly relevant jobs that were focused on the needs
of the business analytics and data science job seeker. This makes the analytics job hunt much more
convenient and efficient, eliminating searches for multiple job titles in multiple cities on several websites, saving job seekers valuable time and energy. 

# Data Sources
We researched several possible data sources from which to gather jobs, including LinkedIn, Glassdoor, Dice, Indeed, and Monster. LinkedIn and Glassdoor have APIs, but they were only available to established websites that could partner with LinkedIn and Glassdoor to list job search results on the partner’s website. The selection of jobs on Dice was too small for our purpose. However, Indeed and Monster proved to have plenty of analytics and data science jobs so, we scraped these.

# Description of Web Scraping Routines and the Dataset
We decided to look into the job postings for four titles: “Data Analyst”, “Business Analyst”, “Product Analyst”, and “Data Scientist”. We restricted our search to the top 11 largest cities in the US.

We extracted the following pieces of information: the “Job Title”, the “Company Name”, and the location as “City”, “State”, and “ZipCode” (if available), “Reviews” (if available). Then we consolidated the rest of the contents into a single text blob named as the “Job Description”. We then parsed the description to extract the pay information as “Salary”, if provided. The result was a database that contained over 25000 opportunities for the selected four titles . 

# Explanation of the Database Design Choices and Advantages Over Alternatives
We chose to use DynamoDB, a NoSQL database, because of the nature of the data that we worked with. As it was text-intensive data, we selected a NoSQL format over a traditional relational database.

NoSQL gave us the flexibility to add and remove attributes as we did not have to define the structure
beforehand. Most of the job descriptions contained data for fields like title, company name, city/state,
and few of them had information about the zip code, salary, reviews, etc. The NoSQL database enabled
us to store the entire job description in a column, which would be a bad design choice for a structured
database.

We had to define the hash key and range key for the table in DynamoDB which is analogous to the primary key in SQL. We had to combine columns that would be unique for each record, and we also had to consider the ways in which we would access the data to decide our hash and range keys. Since we will primarily be working with job titles, we chose that to be our hash key. The other key column we will access is the location so we could define that as our range key. But the issue is that the combination of title and location will not be unique for each record. So, we decided to combine the company name with the location for our range key. We could create secondary indexes for other columns if we needed later. 
