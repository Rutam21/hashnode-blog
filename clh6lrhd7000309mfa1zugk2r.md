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