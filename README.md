# Linkedin-Scraper
LinkedIn Scraper is a collection of Python Scrapy spiders designed to extract job data, people profiles, and company profiles from LinkedIn.com. These spiders provide a convenient way to gather valuable information from LinkedIn for various purposes, such as data analysis, research, or building your own job board.
<br><br>
This Scrapy project contains 3 seperate spiders:

| Spider  |      Description      |
|----------|-------------|
| `linkedin_people_profile` |  Scrapes people data from LinkedIn people profile pages. | 
| `linkedin_jobs` |  Scrapes job data from LinkedIn (https://www.linkedin.com/jobs/search) | 
| `linkedin_company_profile` |  Scrapes company data from LinkedIn company profile pages. | 


<br>

## Getting Started
These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

1. Clone this project: `git clone https://github.com/iAlex0/Linkedin-Scrapy.git`
2. Create a Python Virtual Environment: `python3 -m venv venv`
3. Activate the Python Virtual Environment: `venv/Scripts/activate`
4. Install the project requirements: `pip install -r requirements.txt`
5. Change working directory to the project folder: `cd linkedin-python-scrapy-scraper`
6. Add your API key to the settings.py file:

    - SCRAPEOPS_API_KEY = 'YOUR_API_KEY'

7. Run the following command: `scrapy list` 
- Returns the following:

    - linkedin_people_profile
    - linkedin_jobs
    - linkedin_company_profile

<br>

## ScrapeOps Proxy
This LinkedIn spider uses [ScrapeOps Proxy](https://scrapeops.io/proxy-aggregator/) as the proxy solution. ScrapeOps has a free plan that allows you to make up to 1,000 requests per month which makes it ideal for the development phase, but can be easily scaled up to millions of pages per month if needs be.

You can [sign up for a free API key here](https://scrapeops.io/app/register/main).


<br>


## Running the linkedin_people_profile scraper

Set the search query parameters you want to search for by updating the `profile_list`:

For example:
```python

def start_requests(self):
    profile_list = ['reidhoffman', 'other_person']
    for profile in profile_list:
        linkedin_people_url = f'https://www.linkedin.com/in/{profile}/' 
        yield scrapy.Request(url=linkedin_people_url, callback=self.parse_profile, meta={'profile': profile, 'linkedin_url': linkedin_people_url})


```

Then to run the spider, enter the following command:
    
```python
scrapy crawl linkedin_people_profile
```
<br>

### Extract More/Different Data
LinkedIn People Profile pages contain a lot of useful data, however, in this spider is configured to only parse:

- Name
- Description
- Number of followers
- Number of connections
- Location
- About
- Experienes - organisation name, organisation profile link, position, start & end dates, description.
- Education - organisation name, organisation profile link, course details, start & end dates, description.

You can expand or change the data that gets extract by adding additional parsers and adding the data to the `item` that is yielded in the `parse_profiles` method.

<br>
---------------------------------------
<br>

## Running the linkedin_jobs scraper
Set the keyword you want to search by updating the following field: 

- keyword = 'Enter Keyword Here'

For example:

```python
class LinkedJobsSpider(scrapy.Spider):
    name = "linkedin_jobs"
    keyword = 'rn nurse'
    api_url = f'https://www.linkedin.com/jobs-guest/jobs/api/seeMoreJobPostings/search?keywords={keyword}&location=United%2BStates&start=' 

```

Then to run the spider, enter the following command:
        
```python
scrapy crawl linkedin_jobs
```
<br>
---------------------------------------
<br>

## Running the linkedin_company_profile scraper
Set the keyword you want to search by updating the following fields:

- keyword = 'Enter Keyword Here'
- company_name = 'Enter Company Name Here'

For example:

```python
class LinkedCompanySpider(scrapy.Spider):
    name = "linkedin_company_profile"
    keyword = 'developer'
    api_url = f'https://www.linkedin.com/jobs-guest/jobs/api/seeMoreJobPostings/search?keywords={keyword}&location=United%2BStates&geoId=103644278&trk=public_jobs_jobs-search-bar_search-submit&start=' 

    company_name = 'google'
    company_pages = [
        f'https://www.linkedin.com/company/{company_name}?trk=public_jobs_jserp-result_job-search-card-subtitle',
        ]

```
    
Then to run the spider, enter the following command:
            
```python
scrapy crawl linkedin_company_profile
```

<br>
---------------------------------------
<br>

### Speeding Up The Crawl
The spiders are set to only use 1 concurrent thread in the ``settings.py`` file as the ScrapeOps Free Proxy Plan only gives you 1 concurrent thread.

However, if you upgrade to a paid ScrapeOps Proxy plan you will have more concurrent threads. Then you can increase the concurrency limit in your scraper by updating the `CONCURRENT_REQUESTS` value in your ``settings.py`` file.

```python
# settings.py

CONCURRENT_REQUESTS = 32
```
<br>
---------------------------------------
<br>

## License
MIT © iAlex0

