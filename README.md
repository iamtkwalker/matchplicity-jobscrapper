# TK-Digigor

This crawler was created to scrape information from a given job platform URL.
The data fields keeping in mind are the following:

- title : string
- description : string
- application_url : sting
- job_type : array|object
- preferred_years_experience : array|object [min, max]
- salary : numeric
- preferred_certification : array|object
- preferred_previous_job_title : string
- preferred_experience : integer
- preferred_soft_skill : array|object
- preferred_technical_skill : array|object
- job_preferred_major : array|object
- job_gpa : digit
- job_locations : array|object

### Dependencies.

* Python 3.8+ (apt install python3-pip)

* Firefox installed (apt install firefox)

* Geckodriver on PATH (in this case is the project's directory)

### Strategy Used

- I used "Flask" to create an api that receives the parameters, 
make request and return a json with extracted data.
  
- Regarding the crawler, I used different kind of strategies. For each major platform I selected the best strategy. 
For instance, for myworkjobdays I used requests, and for Taleo I used selenium.
  
- The crawler receive a list of dict with key 'url' and execute as many threads configured on the "MAX_WORKERS" on the config.py.

Tested on Python 3.8

### How to use

1) go to the config.py and edit:

-PROXYSOCKET #if needed.
   
-MAX_WORKERS: this is the number of threads to be executed on parallel. 

-PORT: Set the port to be used on the API
    
2) install requirements doing:

```pip install -r requirements.txt```.

3) execute```python3 app.py runserver``` from the project's directory
    
#### Notes or Considerations

- For the taleo platform and the ones using Selenium the server will consume at least 500 mb of ram and as many cpu as needed, per instance.
So,if you have MAX_WORKERS = 10 and you execute 10 taleo urls the crawler will create 10 firefox instances.
  
- For the taleo platform the URL must have the 'job' key with a value at the bottom for instance:
  
```
https://ms.taleo.net/careersection/2/jobdetail.ftl?job=3183894
```

- if the job is no longer available the API will respond a msg on the "description" key called "the job is no longer available".
for instance:
  
```
  {
  "error_code": 0, 
  "msg": "Results obtained", 
  "results": [
    {
      "application_url": null, 
      "description": "the job is no longer available", 
      "job_gpa": null, 
      "job_locations": [], 
      "job_preferred_major": [], 
      "job_type": [], 
      "preferred_certification": [], 
      "preferred_previous_job_title": null, 
      "preferred_soft_skill": [], 
      "preferred_technical_skill": [], 
      "preferred_years_experience": [], 
      "salary": null, 
      "title": null
    }
  ], 
  "success": true
}
```
- If the url is a 404 error or not a 200 http status code, the API will respond: "Error_code": 404.

#### Example of usage:
```
curl --location --request POST 'http://151.80.33.51:6789/get-job' \
--header 'Content-Type: application/json' \
--data-raw '[
{
    "url": "https://workday.wd5.myworkdayjobs.com/en-US/Workday/job/Netherlands-Amsterdam/JR-55589-Senior-Support-Model-and-Governance-Consultant--EMEA---Open-_JR-60428"
},
{
    "url": "https://workday.wd5.myworkdayjobs.com/en-US/Workday/job/USA-MN-Minneapolis/Healthcare-Customer-Base-Account-Executive---Large-Enterprise_JR-61767"
},
{
    "url": "https://ms.taleo.net/careersection/2/jobdetail.ftl?job=3192405"
},
{
    "url": "https://ms.taleo.net/careersection/2/jobdetail.ftl?job=3188186"
}
]'
```
#### Changelog:

09/06/2021:

- data fields for the JSON response refactored.

- Added more tries and exceptions.

- Added "the job is no longer available"

- Regex expressions improved 

09/11/2021 

- added correct data structure for job_location on taleo source.

- added key "source" to the result JSON.

- added "success" key to determine if the scraping process for each job url worked well.

- added more exceptions.

- added checking for "job" key on the taleo URL.

- job_type results improved.

09/11/2021

- added locations asiggn when two results are scraped. This is using the database of locations provided.

09/14/2021

- added more use cases when the location has keywords
- 
09/23/2021

- added validation for "country" , "state" and "city". if there is no match, it shows the same results that the page

'Francisco Battan. Informatics Engineer.'


