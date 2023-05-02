---
title: "Spam Detection using MindsDB with OpenAI GPT-4 or HuggingFace"
datePublished: Tue May 02 2023 18:28:16 GMT+0000 (Coordinated Universal Time)
cuid: clh6lrhd7000309mfa1zugk2r
slug: spam-detection-using-mindsdb-with-openai-gpt-4-or-huggingface
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1683021539043/bfd1b33b-d704-4bbf-a7c3-1815726b9899.png
tags: nlp, openai, huggingface, mindsdb, mindsdbhackathon

---

# Introduction

In today's digital world, everything is very accessible, be it a piece of information about a place or people's emails or phone numbers at random. While this makes connectivity more accessible, some people or organizations use them to dispatch several unwanted texts or emails with malicious intent that we generally call SPAM.

With the growing problem of SPAM every day for everyone, there has been a need to restrict these messages from unnecessarily cluttering our inboxes. But all thanks to modern AI solutions, you can now easily create AI models that can detect the SPAM texts for you.

In this article, you will learn how you can create a functional SPAM detection model using MindsDB along with OpenAI GPT-4 and HuggingFace to detect a piece of text as either SPAM or HAM.

# Data Setup

You will need a dataset that contains a mixture of SPAM and HAM texts to complete this tutorial. You can easily find some publicly available SPAM datasets either from Kaggle to any other dataset distribution centres.

For this tutorial, the dataset available [here](https://github.com/mohitgupta-omg/Kaggle-SMS-Spam-Collection-Dataset-/blob/master/spam.csv) will be used.

### Connecting the Data

Once you have downloaded the CSV file, you can use it to create a table in MindsDB Cloud. Follow the steps below to create the table successfully.

* Login to the [MindsDB Cloud Editor](https://cloud.mindsdb.com/) and click on the `Add` button. Then click on the `Upload File` button from the dropdown menu.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683051724132/51d8f07c-f602-4b29-be56-0979e49f3461.png align="center")
    
* Click on the `Import a File` section and browse the CSV file from your local system. Provide a name for the Table in the `Datasource Name` section and then click on `Save and Continue` to create the table.
    
    ![Upload File](https://cdn.hashnode.com/res/hashnode/image/upload/v1683051979025/56736d8f-368a-4250-827c-c13a37bd3422.png align="center")
    

If the table is created successfully, then the control takes you back to the Editor where you can find two queries now. Simply, execute the first one to fetch the list of all tables.

```sql
SHOW TABLES FROM files;
```

This should return you the list and your table should be present there. This confirms that the table is created.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683052764177/8210aaf8-1737-4add-bf2e-2f046f6307c8.png align="center")

### Understanding the Data

Once you have created the table, the next thing you can do is check the contents of that table. To do this, simply run the next query available in the Editor.

```sql
SELECT * FROM files.Spam LIMIT 10;
```

This should return the first 10 records from the `Spam` table.

![Table Records](https://cdn.hashnode.com/res/hashnode/image/upload/v1683054202579/5fccebf4-0732-4c38-9b3e-5b6954166669.png align="center")

As you can see above, the current table has 2 rows only. The `Label` column contains the pre-determined labels and the `Text` column contains a variety of texts.

You are now ready to create a model that can work on this data to find out the SPAM texts.

# Creating an OpenAI GPT-4 Model

In this section, you will create a model that uses the OpenAI engine to detect SPAM texts. With MindsDB, you can do this by executing a simple SQL statement as shown below.

```sql
CREATE MODEL model_name
PREDICT column_name
USING
    engine = 'engine_name',
    model_name = 'name_of_the_model',
    prompt_template = 'your_prompt_for_gpt-4';
```

Well, it's time now to look at all the parameters that you'll be passing in this statement while creating the model.

* **model\_name**: Provide a name as you wish for the model.
    
* **column\_name**: Name the target column, which can be anything as you want.
    
* **engine\_name**: Provide the name of the engine. (`openai` here)
    
* **name\_of\_the\_model**: Provide the name of the model you want to use. (`gpt-4` here)
    
* **your\_prompt\_for\_gpt-4**: Provide a specific prompt that GPT-4 will understand and process your data accordingly to fetch you the desired output.
    

Now if you replace the placeholders with the real values, then the actual query will look something like this.

```sql
CREATE MODEL openai_spam_detect
PREDICT Type
USING
    engine = 'openai',
    model_name = 'gpt-4',
    prompt_template = 'Determine and Label the given text as "SPAM" or "HAM". 
                       "You have won 12 crore lottery. Click here to claim" : SPAM
                       "The team scrum call has been shifted to 8PM today. Kindly join on time." : HAM
                       "{{Text}}" : ';
```

Once you execute it, it returns the row record for the `openai_spam_detect` model from the `models` table.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683056001827/12e889c5-d4f8-4f1a-b535-3828bf4549ce.png align="center")

One thing to note here is that the `{{Text}}` parameter in the `prompt_template` represents

# Creating a HuggingFace Model

You can also choose to create a HuggingFace engine model if you don't want to go with the OpenAI engine. The SQL statement to create such a model will look something like this.

```sql
CREATE MODEL model_name
PREDICT column_name
USING
engine = 'engine_name',
task = 'task_name',
model_name = 'model_name',
input_column = 'input_column',
labels = ['label1_name', 'label2_name'];
```

Before you proceed with the actual query, it's time to understand all the parameters that will use.

* **model name**: Name of the model as you wish
    
* **column\_name**: Name of the target column
    
* **engine**: Provide the name of the engine. (`huggingface` here)
    
* **task**: Provide the name of the task category as per HuggingFace Engine. (`text-classification` here)
    
* **model\_name**: Provide the name of the model from HuggingFace Engine. (`mariagrandury/roberta-base-finetuned-sms-spam-detection` here)
    
* **input\_column**: Name of the column/parameter that will serve the input text. (`Text` here)
    
* **label1\_name/label2\_name**: This is a model-specific parameter which requires the label names for the classification. (`HAM` and `SPAM` here)
    

You can now replace the placeholders with the original values to form the actual query.

```sql
CREATE MODEL mindsdb.hf_spam_detect
PREDICT Type
USING
engine = 'huggingface',
task = 'text-classification',
model_name = 'mariagrandury/roberta-base-finetuned-sms-spam-detection',
input_column = 'Text',
labels = ['HAM', 'SPAM'];
```

Once you execute it successfully, it returns the row record for the specific model from the `models` table.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683057878309/04518ec2-debc-4b2e-a919-9cafda5b8afa.png align="center")

# Status of the Model

The models that you created above will take some time to be fully trained. In the meanwhile, you can check the status of each of those models by using the queries below respectively.

### OpenAI GPT-4 Status

The following SQL statement will fetch the status of the model.

```sql
SELECT STATUS FROM models
WHERE name='openai_spam_detect';
```

This should return the current status of the model `openai_spam_detect` .

![OpenAI Model Status](https://cdn.hashnode.com/res/hashnode/image/upload/v1683058273311/bcb0ae0a-bdc1-4489-a19a-fe72f1433d0f.png align="center")

### HuggingFace Model Status

You can use the statement below to find the status of the model.

```sql
SELECT STATUS FROM models
WHERE name='hf_spam_detect';
```

You can see the status of the `hf_spam_detect` as shown below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683058425105/765809a3-0b7d-4354-8d51-d360dd0ee49d.png align="center")