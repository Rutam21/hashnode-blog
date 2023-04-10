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

In today's information age, we are overwhelmed with an abundance of information that can be difficult to digest. As a result, text summarization has become increasingly important. Text summarization refers to the process of reducing a large amount of text to a shorter, more concise version while retaining the most essential information. This tutorial will explore how to use MindsDB Cloud Editor and OpenAI GPT-4 to summarize BBC News articles.

# **Data Setup**

There are two ways to retrieve BBC news articles for this tutorial. We can either use the [**BBC News API**](https://newsapi.org/s/bbc-news-api) to fetch the articles in JSON format or we can simply gather the [articles](https://www.kaggle.com/datasets/pariza/bbc-news-summary) from Kaggle and combine them to form a CSV file for our use.

### Connecting the data

So, now we have a CSV file that contains the News articles. The next thing we want to do is to create a Table using this data. The following steps should create the table successfully.

* Click on the `Add` button on the MindsDB Cloud Editor and then tap the `Upload File` button from the dropdown menu.
    
* Now select the CSV file that we have created, provide a Table name in the `Datasource Name` field and then click on the `Save and Continue` button.
    

Once the table is created successfully, execute the first query written on the MindsDB Cloud Editor to fetch all available tables.

![Query to fetch list of Tables](https://cdn.hashnode.com/res/hashnode/image/upload/v1681123134483/e8683363-ecad-43d3-b151-7a97d73b7984.png align="center")

```sql
SHOW TABLES FROM files;
```

The result should contain the list of tables available.

![List of Tables](https://cdn.hashnode.com/res/hashnode/image/upload/v1681123297590/fa510ed3-5e05-4c4e-99a3-6fb018577789.png align="center")

### **Understanding the data**

Let's explore the data now in the table we just created to find out more details about it. Simply, execute the second query i.e., `Select` query, to get the records present in the table.

```sql
Select * from files.BBCNews LIMIT 10;
```

The first 10 records from the table `BBCNews` are returned as shown below.

![First 10 records of the Table](https://cdn.hashnode.com/res/hashnode/image/upload/v1681123554900/370a3ac8-659d-49f2-81a1-b971900d1800.png align="center")

The table only contains one column.

* news\_articles: This column contains the complete news article.
    

We will try to summarize the text in the `news_articles` column using the model we will be creating below.

# **Creating an OpenAI GPT-4 Model**

We can now create an OpenAI model to extract summaries from news articles. However, `gtp-3.5-turbo` gets used by default for the model. So, when we write the query for model creation, we need to pass an extra parameter namely `model-name` to specify the model to use `gpt-4` explicitly.

So, let's look at the SQL statement now.

```sql
CREATE MODEL MODEL_NAME
PREDICT Target_Column_Name
USING
    engine = 'name_of_the_engine',
    model-name = 'name_of_the_model',              
    prompt_template = 'Provide an informative summary of the text text:{{input_column_name}} using full sentences.';
```

The actual query will become like this after replacing the placeholders with appropriate values.

```sql
CREATE MODEL news_summarizer
PREDICT summary
USING
    engine = 'openai',
    model-name = 'gpt-4'              
    prompt_template = 'Provide an informative summary of the text text:{{article}} using full sentences';
```

This should return the row record of the specific model that we just created from the \`models\` table upon successful execution.

# **Status of the Model**

The model might take a while to be ready for use. In the meantime, we can check it's status with the query below.

```sql
SELECT status FROM models
WHERE name = 'news_summarizer';
```

> ***MindsDB has recently organized a Hackathon in collaboration with Hashnode. You can check all the details by clicking on the banner below.***
> 
> [![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681064378865/db525fc7-637f-4600-b152-ffb278ab6e52.png align="center")](https://hashnode.com/hackathons/mindsdb)

[![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681064428416/ffe7278b-26fa-4844-90ec-6582dda88ab2.png align="center")](https://github.com/sponsors/Rutam21)