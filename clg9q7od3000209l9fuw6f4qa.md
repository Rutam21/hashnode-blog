---
title: "Text Summarization using MindsDB and OpenAI GPT-4"
seoTitle: "BBC News Text Summarization"
datePublished: Sun Apr 09 2023 18:16:27 GMT+0000 (Coordinated Universal Time)
cuid: clg9q7od3000209l9fuw6f4qa
slug: text-summarization-using-mindsdb-and-openai-gpt-4
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1681063781698/4c8717d9-e7ab-4e7b-8cee-ef116bc65e21.png
tags: 2articles1week, openai, mindsdb, gpt-4, mindsdbhackathon

---

# Introduction

In today's information age, we are overwhelmed with an abundance of information that can be difficult to digest. As a result, text summarization has become increasingly important. Text summarization refers to the process of reducing a large amount of text to a shorter, more concise version while retaining the most essential information. In this tutorial, we will explore how to use MindsDB Cloud Editor and OpenAI GPT-4 to summarize BBC News articles.

## **Data Setup**

There are two ways to retrieve BBC news articles for this tutorial. We can either use the [**BBC News API**](https://newsapi.org/s/bbc-news-api) to fetch the articles in JSON format or we can simply gather the [articles](https://www.kaggle.com/datasets/pariza/bbc-news-summary) from Kaggle and combine them to form a CSV file for our use.

### Connecting the data

So, now we have a CSV file now that contains the News articles. The next thing we want to do is to create a Table using this data. The following steps should create the table successfully.

* Click on the `Add` button on the MindsDB Cloud Editor and then tap the `Upload File` button from the dropdown menu.
    
* Now select the CSV file that we have created, provide a Table name in the `Datasource Name` field and then click on the `Save and Continue` button.
    
* Once the table is created successfully, execute the first query written on the MindsDB Cloud Editor to fetch all available tables.
    
    ```sql
    SHOW TABLES FROM files;
    ```