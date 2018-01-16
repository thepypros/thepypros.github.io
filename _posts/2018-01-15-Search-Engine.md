---
intro: |
  Have you ever wanted to scrape search engine results from the web? In this project, I use a search engine called StartPage as it provides similar results to Google without the extra headache of bypassing Google's bot detection protocols. Check out the video below to see how quickly you can set it up!
excerpt: |
    Have you ever wanted to scrape search engine results from the web? In this project, I use a search engine called StartPage as it provides similar results to Google without the extra headache of bypassing Google's bot detection protocols.
layout: post
permalink: /search-engine-python/
title: Get Search Engine Results with Python
youtube: EELySnTPeyw
---
Let's start by importing Selenium and creating a function to request a webpage.
```python
//Import Selenium
import selenium.webdriver as webdriver

//Create a function for getting the specified URL
def get_results(search_term):
  url = "https://www.startpage.com"
  browser = webdriver.Firefox()
  browser.get(url)
```
Once we've identified the search form on our search engine, we can find the element on the page like so:

```python
search_box = browser.find_element_by_id("query")
```

Then we can pass our search term into the form input:

```python
search_box.send_keys(search_term)
```

Now we can submit the search form itself:

```python
search_box.submit()
```

At this point, the search form has been submitted, and we receive back our search engine results. We need to parse the results to find the links that the search engine returns. In this particular instance, we don't care about the description or title of the links, just the URLs themselves. But for your search engine scraping project, you might want to parse additional pieces of information.

By using the developer tools in the browser, we notice that the sometimes the results are rendered with advertisements, and sometimes they are not. To account for both scenarios, we're going to use a try/except, to make sure we don't include the ads themselves.

```python
try:
    links = browser.find_elements_by_xpath("//ol[@class='web_regular_results']//h3//a")
except:
    links = browser.find_elements_by_xpath("//h3//a")
```
Next, let's create a list and iterate through the links:

```python
results = []
for link in links:
    //Grabs the href attribute value
    href = link.get_attribute("href")
    //Printing is not necessary, but makes it easier to see each link as it's being iterated on.
    print(href)
    //Add each link to our results list
    results.append(href)
browser.close()
return results
```

Finally, we can call our get_results() function and pass it any search term we desire. We could adapt the design to take command-line arguments, or pass in a list of search terms from a .csv file, but for now we'll keep it simple.

```python
response = get_results("dog")
```

Here's our finished code:

```python
import selenium.webdriver as webdriver

def get_results(search_term):
    url = "https://www.startpage.com/"
    browser = webdriver.Firefox()
    browser.get(url)
    search_box = browser.find_element_by_id("query")
    search_box.send_keys(search_term)
    search_box.submit()
    try:
        links = browser.find_elements_by_xpath("//ol[@class='web_regular_results']//h3//a")
    except:
        links = browser.find_elements_by_xpath('//h3//a')
    results = []
    for link in links:
        href = link.get_attribute("href")
        print(href)
        results.append(href)
    browser.close()
    return results

response = get_results("dog")
```
