---
title: API Reference for Article Recommendation

language_tabs: # must be one of https://git.io/vQNgJ
  - python

toc_footers:
  - <a href='#'>Ask Neptune Team for a API Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Northwestern Mutual LearnVest Article Recommendation API! You can use our API to access Recommender POST/GET endpoints, which can get article recommendations based on the articles id or article JSON in Neptune Content API. Two UI debugging sites are also available to use to demonstrate the result of recommendations.

We have language bindings Python, you can view code examples in the dark area to the right.

This API documentation page was created with [Slate](https://github.com/lord/slate).

# Authentication

Recommender uses API keys to allow access to the API. Neptune Team created all the API keys for us and since right now Recommender API is only used by Neptune Team, the API keys are not distributed here.
Recommender expects for the API key to be included in all API requests to the server in a header that looks like the following:

`apikey: include-api-key-here`

<aside class="notice">
You must replace <code>include-api-key-here</code> with Recommender API key.
</aside>

# Endpoints

## Get Recommended Articles By Article ID

```python
import requests
headers = {
        'apikey': 'include-api-key-here',
    }

params = (
    ('organization','LV'),
    ('article_id','feac293f-d095-4e73-83e2-6e9deae638f9'),
    ('num_of_rec','5')
)

response = requests.get('https://api-int.nmlv.nml.com/api/v1/nmlv-article-recommender/tfidf_recommend', params=params, headers = headers)

recommended_json = json.loads(response.content)
```

> The above command returns JSON structured : Json of num_of_rec articles that are most similar to the article that belongs to the article_id. 

```json
{
    "0": {
        "id": "feac293f-d095-4e73-83e2-6e9deae638f9",
        "similarity_score": 1,
        "text": "HOW I WENT BANKRUPT AT 23.\n For as long as I can remember, my father warned me about the dangers of credit cards..."
    },
    "1": {
        "id": "87599c2e-ea76-4136-9d66-edb443ed083e",
        "similarity_score": 0.36307323604179664,
        "text": "HOW MANY CREDIT CARDS SHOULD I GET?.\n Take a look at your wallet. If you’re like most Americans, you have a credit card or two (or three) to your name..."
    },
    "2": {
        "id": "13296d2f-91e0-4925-8dab-1fed7b605103",
        "similarity_score": 0.34950453457934527,
        "text": "WHAT TO KNOW BEFORE OPENING YOUR FIRST CREDIT CARD.\n I didn’t get my first credit card until after I graduated college..."
    },
    "3": {
        "id": "abf1f974-09d0-4895-9125-5ec3a6b4fc73",
        "similarity_score": 0.30905705116686355,
        "text": "7 CREDIT CARD TERMS TO KNOW BEFORE YOU SWIPE .\n It’s unlikely that a day goes by where you don’t reach for your credit card.\nBut how many of us truly understand all the basics before we apply for new cards?..."
    },
    "4": {
        "id": "0eab66f9-4413-4cf2-8005-05c0bf182303",
        "similarity_score": 0.2937517603460955,
        "text": "I MAXED OUT A $10K CREDIT CARD WHEN I WAS 18 — AND SPENT MY 20S PAYING FOR IT.\n We all have regrets ..."
    }
}
```

This endpoint recommended five articles from either Northwestern Mutual or LearnVest, depends on which organization parameter and article id you passed in. For testing purpose, the first article will always be the article that has the id that passed in. 

### HTTP Request

INT: `GET https://api-int.nmlv.nml.com/api/v1/nmlv-article-recommender/tfidf_recommend`

QA: `GET https://api-qa.nmlv.nml.com/api/v1/nmlv-article-recommender/tfidf_recommend`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
organization | NM | The organization of articles that the recommender returns. The values to pass in could be either 'NM' or 'LV', which represent articles from Northwestern Mutual and LearnVest.
article_id | No Default | The Neptune article ID that represents a unique ID assigned to article.
num_of_rec | 5 | The number of recommendation that the recommender gave out. The number of recommendation could be either 1 to 20 (The maximum number could be changed if needed)

<aside class="success">
Remember — Only use the id of the article that exists in Northwestern Mutual or LearnVest published article. The CMS article id is stored in NEPTUNE_CONTENT_API, and this endpoint would interact with NEPTUNE_CONTENT_API to get recommended articles
</aside>

### Output Json Keys

Keys | Description
--------- | -----------
id | ID of the articles that are recommended. 
similarity_score | The cosine similarity score of the recommended article's bag of words with the object article's bag of words (between 0 to 1, with 1 the most similar and 0 not similar at all
text |Content of the article recommended, for testing purpose, might be removed in the future. 

## POST Article Json and Return Recommender Articles

```python
import requests
headers = {
        'apikey': 'include-api-key-here',
        'Content-Type':'application/json',
        'Accepts': 'application/json'
    }

params = (
    ('organization','LV'),
    ('num_of_rec','5')
)

article_json = 'the-article-json-from-Neptune-content-api'

response = requests.post('https://api-int.nmlv.nml.com/api/v1/nmlv-article-recommender/tfidf_recommend', params=params, json=article_json, headers = headers)

recommended_json = json.loads(response.content)
```

> The above command returns JSON structured like this:

```json
{
    "0": {
        "id": "feac293f-d095-4e73-83e2-6e9deae638f9",
        "similarity_score": 1,
        "text": "HOW I WENT BANKRUPT AT 23.\n For as long as I can remember, my father warned me about the dangers of credit cards..."
    },
    "1": {
        "id": "87599c2e-ea76-4136-9d66-edb443ed083e",
        "similarity_score": 0.36307323604179664,
        "text": "HOW MANY CREDIT CARDS SHOULD I GET?.\n Take a look at your wallet. If you’re like most Americans, you have a credit card or two (or three) to your name..."
    },
    "2": {
        "id": "13296d2f-91e0-4925-8dab-1fed7b605103",
        "similarity_score": 0.34950453457934527,
        "text": "WHAT TO KNOW BEFORE OPENING YOUR FIRST CREDIT CARD.\n I didn’t get my first credit card until after I graduated college..."
    },
    "3": {
        "id": "abf1f974-09d0-4895-9125-5ec3a6b4fc73",
        "similarity_score": 0.30905705116686355,
        "text": "7 CREDIT CARD TERMS TO KNOW BEFORE YOU SWIPE .\n It’s unlikely that a day goes by where you don’t reach for your credit card.\nBut how many of us truly understand all the basics before we apply for new cards?..."
    },
    "4": {
        "id": "0eab66f9-4413-4cf2-8005-05c0bf182303",
        "similarity_score": 0.2937517603460955,
        "text": "I MAXED OUT A $10K CREDIT CARD WHEN I WAS 18 — AND SPENT MY 20S PAYING FOR IT.\n We all have regrets ..."
    }
}

```

This endpoint recommended five articles from either Northwestern Mutual or LearnVest, depends on which organization parameter and article JSON you passed in. For testing purpose, the first article will always be the article that has the id that passed in. 

### HTTP Request

INT: `POST https://api-int.nmlv.nml.com/api/v1/nmlv-article-recommender/tfidf_recommend`

QA: `POST https://api-qa.nmlv.nml.com/api/v1/nmlv-article-recommender/tfidf_recommend`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
organization | NM | The organization of articles that the recommender returns. The values to pass in could be either 'NM' or 'LV', which represent articles from Northwestern Mutual and LearnVest.
num_of_rec | 5 | The number of recommendation that the recommender gave out. The number of recommendation could be either 1 to 20 (The maximum number could be changed if needed)

<aside class="success">
Remember — to include Content-Type and Accepts as application/json in the headers.
</aside>

### Output Json Keys

Keys | Description
--------- | -----------
id | ID of the articles that are recommended. 
similarity_score | The cosine similarity score of the recommended article's bag of words with the object article's bag of words (between 0 to 1, with 1 the most similar and 0 not similar at all
text |Content of the article recommended, for testing purpose, might be removed in the future. 

## articleRec User Interface Debugging Site

The UI that will have drop down of the headlines of the articles in Northwestern Mutual. Choosing one and click the button 'get recommendation', 5 most similar articles' headlines and the similarity scores will show up.

### HTTP
INT: `https://api-int.nmlv.nml.com/api/v1/nmlv-article-recommender/articleRec`

QA: `https://api-qa.nmlv.nml.com/api/v1/nmlv-article-recommender/articleRec`

## textboxRec User Interface Debugging Site

The UI that will have a textbox for inputing. Input the body of the article and click the button 'get recommendation', 5 most similar articles' headlines and the similarity scores will show up.

### HTTP
INT: `https://api-int.nmlv.nml.com/api/v1/nmlv-article-recommender/textboxRec`

QA: `https://api-qa.nmlv.nml.com/api/v1/nmlv-article-recommender/textboxRec`

## notifyme Endpoint to register with Neptune content API

When `notifyme` endpoint is called by `content-api/destination`, and the body of the request has `contentType: article`, the endpoint will re-fetch data and return immediately with the JSON input for debugging purposes. The endpoint will fit model in the background using thread. Recommender will check if the api is registered with destination endpoint, if not recommender will send a POST request to destination endpoint to register.

### HTTP
INT: `https://api-int.nmlv.nml.com/api/v1/nmlv-article-recommender/notifyme`

QA: `https://api-qa.nmlv.nml.com/api/v1/nmlv-article-recommender/notifyme`

## dash T-SNE visualization User Interface Debugging Site

The UI that have TF-IDF document vectors visualization for NM articles. User could interact with the visualization by choosing Perplexity and learning rate of T-SNE. Clicking on the points, the title of article will show up, and recommendations will pop up at the end of page. The color of the points corresponds to the parent section of articles, and similar articles are closer in the document vectors map. we can also see that points in same color are closer in the map as well.

### HTTP
INT: `https://api-int.nmlv.nml.com/api/v1/nmlv-article-recommender/dash/dash`

QA: `https://api-qa.nmlv.nml.com/api/v1/nmlv-article-recommender/dash/dash`
