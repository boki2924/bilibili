
# README

## Overview

This repository contains two Jupyter Notebooks: `live-scrape_translated_v3.ipynb` and `bilibili_translated_v3.ipynb`. These notebooks include scripts to scrape live comments (bullet chats) from Bilibili, a popular Chinese video sharing platform, and analyze the data.

### live-scrape_translated_v3.ipynb

This notebook focuses on scraping live comments from a specified Bilibili live room and processing them.

### bilibili_translated_v3.ipynb

This notebook extracts comments from a Bilibili video, processes them, and performs some data analysis.

## Notebooks Details

### live-scrape_translated_v3.ipynb

1. **Dependencies Installation**
   ```python
   !pip install pyobjc
   ```
   - Installs the `pyobjc` library which is used for macOS Python integrations.

2. **Import Libraries and Set Up**
   ```python
   import time
   import urllib.request
   import json
   import requests
   import os
   import pygetwindow as gw
   ```
   - Imports necessary libraries for handling HTTP requests, JSON data, operating system tasks, and window management.

3. **Initialize Variables**
   ```python
   item = []
   room_id = input('Please enter the room id:')
   ```
   - Initializes an empty list `item` to store comments and prompts the user to enter the Bilibili live room ID.

4. **Create and Open File**
   ```python
   file_path = f'{room_id}_comments.txt'
   with open(file_path, 'a', encoding='utf-8') as fp:
       pass  # Create an empty file
   os.startfile(file_path)
   ```
   - Creates and opens a file to store the comments.

5. **Function: `get_mes`**
   ```python
   def get_mes():
       url = f'https://api.live.bilibili.com/xlive/web-room/v1/dM/gethistory?roomid={room_id}&room_type=0'
       headers = {
           'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36 Edg/117.0.2045.43'
       }
       request = urllib.request.Request(url=url, headers=headers)
       response = json.loads(requests.get(url=url, headers=headers).text)
       return response
   ```
   - Sends a request to the Bilibili API to fetch live comments for the specified room and returns the JSON response.

6. **Function: `process_comments`**
   ```python
   def process_comments(comments):
       # Process bullet comment data
       # Add your processing logic here
       pass
   ```
   - Placeholder for processing the retrieved comments. Users can add their processing logic here.

7. **Function: `close_window`**
   ```python
   def close_window():
       # Close the window, assuming we want to close the first matched window
       windows = gw.getWindowsWithTitle('title of your window')
       if windows:
           windows[0].close()
   ```
   - Closes a window by its title. Assumes it needs to close the first matched window.

8. **Main Script Execution**
   ```python
   while True:
       response = get_mes()
       process_comments(response)
       time.sleep(10)
   ```
   - Continuously fetches and processes comments every 10 seconds.

### bilibili_translated_v3.ipynb

1. **Dependencies Installation**
   ```python
   !pip install beautifulsoup4 lxml
   ```
   - Installs `beautifulsoup4` and `lxml` libraries for parsing XML data.

2. **Import Libraries and Set Up**
   ```python
   import requests
   from bs4 import BeautifulSoup
   import matplotlib.pyplot as plt
   ```
   - Imports necessary libraries for handling HTTP requests, parsing HTML/XML data, and creating plots.

3. **Function: `get_censorscore`**
   ```python
   def get_censorscore(apilink):
       header = {
           'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36 Edg/117.0.2045.43'
       }
       response = requests.get(apilink, headers=header)
       cid = (response.json())['data'][0]['cid']
       cidlink = 'https://comment.bilibili.com/' + str(cid) + '.xml'
       response = requests.get(cidlink, headers=header)
       soup = BeautifulSoup(response.text, "lxml-xml")
       d_tags = soup.find_all('d')
       scores = []
       for tag in d_tags:
           # Split the 'p' attribute and get the last element
           score = tag['p'].split(',')[-1]
           scores.append(int(score))
       return scores
   ```
   - Fetches comments for a given Bilibili video URL, parses the XML data to extract scores, and returns a list of scores.

4. **Example Usage**
   ```python
   s = get_censorscore('https://www.bilibili.com/video/BV1NR4y157DB/?spm_id_from=333.337.search-card.all.click&vd_source=a676e9574ae10f45ae8a73f5c6c428fd')
   ```

5. **Data Visualization**
   ```python
   import matplotlib.pyplot as plt
   counts = {x: s.count(x) for x in set(s)}
   total = len(s)
   percentages = {k: v / total * 100 for k, v in counts.items()}
   x = list(percentages.keys())
   y = list(percentages.values())
   plt.figure(figsize=(10, 6))
   plt.bar(x, y, color='skyblue')
   plt.title('Guanca.cn Censor Score Distribution')
   plt.xlabel('Censor Score')
   plt.ylabel('Percentage (%)')
   plt.xticks(range(0, max(x) + 1))
   plt.xlim(0, max(x) + 1)
   plt.grid(axis='y', linestyle='--', alpha=0.7)
   plt.show()
   ```
   - Visualizes the distribution of the extracted scores using a bar plot.

## Usage

1. Clone the repository and navigate to the directory containing the notebooks.
2. Ensure you have Jupyter Notebook installed and set up.
3. Open the notebooks using Jupyter Notebook:
   ```bash
   jupyter notebook live-scrape_translated_v3.ipynb
   jupyter notebook bilibili_translated_v3.ipynb
   ```
4. Follow the instructions in each notebook to run the scripts and analyze the data.

## Requirements

- Python 3.x
- Jupyter Notebook
- Libraries:
  - pyobjc
  - beautifulsoup4
  - lxml
  - requests
  - matplotlib
  - pygetwindow (for live-scrape)
