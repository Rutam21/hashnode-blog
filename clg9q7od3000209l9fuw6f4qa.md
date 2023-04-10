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
Select * from 
CREATE MODEL news_summarizer
PREDICT summary
USING
    engine = 'openai',
    model_name = 'gpt-4',              
    prompt_template = 'Provide an informative summary of the text text:{{news_articles}} using full sentences';
```

This should return the row record of the specific model that we just created from the \`models\` table upon successful execution.

# **Status of the Model**

The model might take a while to be ready for use. In the meantime, we can check its status with the query below.

```sql
SELECT status FROM models
WHERE name = 'news_summarizer';
```

Please note that we have to wait until the status is `complete` before we can start using the model.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681129288159/40e7f6fa-ef92-420d-bf90-574385013362.png align="center")

# Making Text Summarization

The model is now ready to summarize long texts/paragraphs into shorter versions making our reading lives easier. We will see two ways in which we can do this one after the other.

### Making a Single Summarization

Here we will provide the model with a larger piece of text and then ask it to return us a short summary of it. Let's see how we can do that below.

```sql
SELECT news_articles, summary
FROM news_summarizer
WHERE news_articles = "Ink helps drive democracy in Asia
The Kyrgyz Republic, a small, mountainous state of the former Soviet republic, is using invisible ink and ultraviolet readers in the country's elections as part of a drive to prevent multiple voting.
This new technology is causing both worries and guarded optimism among different sectors of the population. In an effort to live up to its reputation in the 1990s as an island of democracy, the Kyrgyz President, Askar Akaev, pushed through the law requiring the use of ink during the upcoming Parliamentary and Presidential elections. The US government agreed to fund all expenses associated with this decision.
The Kyrgyz Republic is seen by many experts as backsliding from the high point it reached in the mid-1990s with a hastily pushed through referendum in 2003, reducing the legislative branch to one chamber with 75 deputies. The use of ink is only one part of a general effort to show commitment towards more open elections - the German Embassy, the Soros Foundation and the Kyrgyz government have all contributed to purchase transparent ballot boxes.
The actual technology behind the ink is not that complicated. The ink is sprayed on a person's left thumb. It dries and is not visible under normal light.
However, the presence of ultraviolet light (of the kind used to verify money) causes the ink to glow with a neon yellow light. At the entrance to each polling station, one election official will scan voter's fingers with UV lamp before allowing them to enter, and every voter will have his/her left thumb sprayed with ink before receiving the ballot. If the ink shows under the UV light the voter will not be allowed to enter the polling station. Likewise, any voter who refuses to be inked will not receive the ballot. These elections are assuming even greater significance because of two large factors - the upcoming parliamentary elections are a prelude to a potentially regime changing presidential election in the Autumn as well as the echo of recent elections in other former Soviet Republics, notably Ukraine and Georgia. The use of ink has been controversial - especially among groups perceived to be pro-government.
Widely circulated articles compared the use of ink to the rural practice of marking sheep - a still common metaphor in this primarily agricultural society.
The author of one such article began a petition drive against the use of the ink. The greatest part of the opposition to ink has often been sheer ignorance. Local newspapers have carried stories that the ink is harmful, radioactive or even that the ultraviolet readers may cause health problems. Others, such as the aggressively middle of the road, Coalition of Non-governmental Organizations, have lauded the move as an important step forward. This type of ink has been used in many elections in the world, in countries as varied as Serbia, South Africa, Indonesia and Turkey. The other common type of ink in elections is indelible visible ink - but as the elections in Afghanistan showed, improper use of this type of ink can cause additional problems. The use of invisible ink is not without its own problems. In most elections, numerous rumors have spread about it.
In Serbia, for example, both Christian and Islamic leaders assured their populations that its use was not contrary to religion. Other rumours are associated with how to remove the ink - various soft drinks, solvents and cleaning products are put forward. However, in reality, the ink is very effective at getting under the cuticle of the thumb and difficult to wash off. The ink stays on the finger for at least 72 hours and for up to a week. The use of ink and readers by itself is not a panacea for election ills. The passage of the inking law is, nevertheless, a clear step forward towards free and fair elections. The country's widely watched parliamentary elections are scheduled for 27 February.
David Mikosz works for the IFES, an international, non-profit organisation that supports the building of democratic societies.";
```

So we have provided a really long text to the model now. Once the query gets executed successfully, we can get a short summary of it as shown below.

> ***MindsDB has recently organized a Hackathon in collaboration with Hashnode. You can check all the details by clicking on the banner below.***
> 
> [![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681064378865/db525fc7-637f-4600-b152-ffb278ab6e52.png align="center")](https://hashnode.com/hackathons/mindsdb)

[![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681064428416/ffe7278b-26fa-4844-90ec-6582dda88ab2.png align="center")](https://github.com/sponsors/Rutam21)