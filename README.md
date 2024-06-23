# News_Summarizer
This project is a Python-based news summarizer that fetches news articles on a given topic using the News API and provides a summary of the articles. The summarization utilizes the OpenAI GPT-3.5-turbo-16k model to enhance the content and deliver concise information.

Features
Fetches the latest news articles on a specific topic.
Provides a structured summary of each news article including the title, author, source, description, and URL.
Uses the News API for retrieving news articles.
Utilizes OpenAI's GPT model for content processing.

Installation
Clone the repository:

sh
Copy code
git clone https://github.com/yourusername/news-summarizer.git
cd news-summarizer
Create and activate a virtual environment (optional but recommended):

sh
Copy code
python3 -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
Install the required packages:

sh
Copy code
pip install -r requirements.txt
Set up environment variables:

Create a .env file in the root directory of the project.
Add your News API key to the .env file:
sh
Copy code
NEWS_API_KEY=your_news_api_key
Obtain an OpenAI API key and set it as an environment variable:

sh
Copy code
export OPENAI_API_KEY=your_openai_api_key
Usage
Run the main function to fetch and print the news summary:

sh
Copy code
python news_summarizer.py
The main function in news_summarizer.py fetches news articles related to the topic "bitcoin" and prints the summary of the first article.

Code Overview
Imports and Setup
python
Copy code
import openai
from dotenv import find_dotenv, load_dotenv
import os
import time
import logging
from datetime import datetime
import requests
import json

load_dotenv()
This section imports the necessary libraries and loads environment variables from the .env file.

Configuration
python
Copy code
news_api_key = os.environ.get("NEWS_API_KEY")
client = openai.OpenAI()
model = "gpt-3.5-turbo-16k"
This section configures the News API key and initializes the OpenAI client.

Fetch News Articles
python
Copy code
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
This function get_news retrieves news articles from the News API for a given topic and structures the information.

Main Function
python
Copy code
def main():
    news = get_news("bitcoin")
    print(news[0])

if __name__ == "__main__":
    main()
The main function fetches news articles about "bitcoin" and prints the first article's summary.

Contributing
Contributions are welcome! Please fork the repository and create a pull request with your changes.

License
This project is licensed under the MIT License. See the LICENSE file for details.

Acknowledgements
OpenAI for their GPT model.
News API for providing the news data.
Replace yourusername with your GitHub username and your_news_api_key and your_openai_api_key with your actual API keys. Ensure you create a requirements.txt file with the necessary dependencies for the project.
