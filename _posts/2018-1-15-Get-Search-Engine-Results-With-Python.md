---
layout: post
title: Get Search Engine Results with Python
youtube: EELySnTPeyw
published: false
intro: Have you ever wanted to scrape search engine results from the web? In this project, I use a search engine called StartPage as it provides similar results to Google without the extra headache of bypassing Google's bot detection protocols. Check out the video below to see how I did it:
---
1. Let's start by importing Selenium and creating a function to request a webpage.
```python
#Import Selenium
import selenium.webdriver as webdriver

#Create a function for getting the specified URL
def get_results(search_term):
  url = "https://www.startpage.com"
  browser = webdriver.Firefox()
  browser.get(url)
```
2. Once we've identified the search form on our search engine, we can find the element on the page like so:

```Python
search_box = browser.find_element_by_id("query")
```

3. Then we can pass our search term into the form input:

```Python
search_box.send_keys(search_term)
```

4. Now we can submit the search form itself:

```python
search_box.submit()
```
