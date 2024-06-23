
# News Summarizer

This project is a Python-based news summarizer that fetches news articles on a given topic using the News API and provides a summary of the articles. The summarization utilizes the OpenAI GPT-3.5-turbo-16k model to enhance the content and deliver concise information.

## Features

- Fetches the latest news articles on a specific topic.
- Provides a structured summary of each news article including the title, author, source, description, and URL.
- Uses the News API for retrieving news articles.
- Utilizes OpenAI's GPT model for content processing.

## Installation

1. Clone the repository:
    ```sh
    git clone https://github.com/yourusername/news-summarizer.git
    cd news-summarizer
    ```

2. Create and activate a virtual environment (optional but recommended):
    ```sh
    python3 -m venv venv
    source venv/bin/activate  # On Windows, use `venv\\Scripts\\activate`
    ```

3. Install the required packages:
    ```sh
    pip install -r requirements.txt
    ```

4. Set up environment variables:
    - Create a `.env` file in the root directory of the project.
    - Add your News API key to the `.env` file:
      ```sh
      NEWS_API_KEY=your_news_api_key
      ```

5. Obtain an OpenAI API key and set it as an environment variable:
    ```sh
    export OPENAI_API_KEY=your_openai_api_key
    ```

## Usage

Run the `main` function to fetch and print the news summary:

```sh
python news_summarizer.py
```

```python
import openai
from dotenv import find_dotenv, load_dotenv
import os
import time
import logging
from datetime import datetime
import requests
import json

load_dotenv()
```
### Fetch News Articles

```python
def get_news(topic):
    url = (
        f"https://newsapi.org/v2/everything?q={topic}&apiKey={news_api_key}&pageSize=5"
    )
    try:
        response = requests.get(url)
        if response.status_code == 200:
            news = json.dumps(response.json(), indent=4)
            news_json = json.loads(news)
            
            data = news_json
            
            status = data["status"]
            total_results = data["totalResults"]
            articles = data["articles"]
            final_news = []
            
            for article in articles:
                source_name = article["source"]["name"]
                author = article["author"]
                title = article["title"]
                description = article['description'] 
                url = article["url"]
                content = article["content"]
                title_description = f"""
                    Title: {title},
                    Author: {author},
                    Source: {source_name},
                    Description: {description},
                    URL: {url}
                """
                final_news.append(title_description)
            return final_news
        else:
            return []
    
    except requests.exceptions.RequestException as e:
        print("Error occurred during API request", e)
```

### Main Function

```python
def main():
    news = get_news("bitcoin")
    if news:
        print(news[0])
    else:
        print("No news articles found.")
    
if __name__ == "__main__":
    main()
```
# Contributing
Contributions are welcome! Please fork the repository and create a pull request with your changes.

# License
This project is licensed under the MIT License. See the LICENSE file for details.

# Acknowledgements
OpenAI for their GPT model.
News API for providing the news data.
