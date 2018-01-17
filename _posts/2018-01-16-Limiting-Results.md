---
intro: |
  You might find that you want to limit the number of search engine results returned, rather than the default ten. In this second post of the series on [search engine scraping with Python]({{ site.baseurl }}{% post_url 2018-01-15-Search-Engine %}), we implement an easy way to limit results.
excerpt: |
  You might find that you want to limit the number of search engine results returned, rather than the default ten. In this second post of the series on search engine scraping, we implement an easy way to limit results.
layout: post
permalink: /limiting-search-engine-results/
title: Limiting Search Engine Results for Scraping
youtube: c8tWv9oYMgQ
---
Let's start by adding an additional parameter to our scraping function, the number of results we want returned. Keep in mind, for this use case, we are limiting the results returned from a single page, so it has to be a number less than 10. If you want to specify a large number of results, i.e. 50, this would need to be implemented a different way as that would require additional page requests.

Let's go ahead and set a default value for the parameter, so if we don't set the value when we call the function, we'll automatically return 3 results.

```python
def get_results(search_term, num_results=3):
```

Now we can simply use slice notation to iterate only through the first num_results (3) search_terms. If slice notation is new to you, check out [this tutorial](https://www.oreilly.com/learning/how-do-i-use-the-slice-notation-in-python).

```python
for link in links[:num_results]:
```

Here's our updated code:

```python
import selenium.webdriver as webdriver

#Added num_results
def get_results(search_term, num_results):
    url = "https://www.startpage.com/"
    browser = webdriver.Firefox()
    browser.get(url)
    search_box = browser.find_element_by_id("query")
    search_box.send_keys(search_term)
    search_box.submit()
    try:
        links = browser.find_elements_by_xpath("//ol[@class='web_regular_results']//h3//a")
    except:
        links = browser.find_elements_by_xpath("//h3//a")
    results = []
    #Added slice
    for link in links[:num_results]:
        href = link.get_attribute("href")
        print(href)
        results.append(href)
        browser.close()
        return results

response = get_results("dog")
```
