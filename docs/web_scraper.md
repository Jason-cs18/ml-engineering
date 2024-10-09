---
title: Web Scraper
layout: default
parent: Engineer
nav_order: 11
---
In daily jobs, we often need to scrap some data from the web. Here is a simple example to show how to use `BeautifulSoup` to scrap articles from [MIT Technology Review](https://www.technologyreview.com/).

`BeautifulSoup` is a lightweight python library for web scraping. It is easy to use and can parse HTML and XML documents. It can also extract data from the parsed documents.

On Linux, you can simply install it by `pip install beautifulsoup4`. 

## Scraping all article urls 
```python
import requests
from bs4 import BeautifulSoup

# Send a GET request to the website
response = requests.get('https://www.technologyreview.com/')

# Parse the HTML content of the page
soup = BeautifulSoup(response.content, 'html.parser')

# Find all the article urls on the page
urls = []
for link in soup.find_all('a', href=True):
    # print(link)
    href = link['href']
    # print(href)
    if href.startswith('https://www.technologyreview.com/2024/'):
        urls.append(href)

np.save("existing_urls.npy", exist_urls)
```

## Scraping a article with url
```python
import random

import numpy as np
import requests
from bs4 import BeautifulSoup

exist_urls = np.load("existing_urls.npy")
url = random.sample(urls, 1)[0]

response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')

title = soup.title.string
story = soup.find(id="content--body")
story_element = story.find_all('p')

content = ""
for para_idx in range(len(story_element)):
    paragraph = story_element[para_idx]
    if paragraph.find_parent('div', class_='afternote') or paragraph.find_parent('div', class_='hero-unit hero-foot') or paragraph.find_parent("div", class_="related__header") or paragraph.find_parent('div', class_="related__deck"):
        continue
    
    if paragraph.find_parent("div", class_="deepDiveItem*"):
        break

    content += paragraph.get_text() + "\n"

print("-----------------")
print(f"title: {title}")
print(f"content: {content}")

```