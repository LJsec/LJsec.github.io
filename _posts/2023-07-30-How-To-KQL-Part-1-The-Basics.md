---
title: How to KQL - Part 1 - The Basics
date: 2023-07-30 12:00:00 +0000
categories: [Keep Calm and KQL, How to KQL]
tags: [kql, azure data explorer]
pin: true
comments: true
math: true
mermaid: true
image:
  path: /assets/img/CoverImages/HowToKQLP1.png
  alt: How To KQL - Part 1 - Cover
---

## Introduction

Kusto Query Language (KQL) is a powerful data query language developed by Microsoft, primarily used for exploring and visualising data across various Microsoft products. It plays an important role in Microsoft's security tooling, with Microsoft Sentinel, Defender for Endpoint, and Intune utilising KQL for data searching and reporting. If you work with Microsoft security products, mastering KQL will be an essential skill to learn.

Named after French explorer Jacques Cousteau, KQL embodies the spirit of exploration as it enables users to delve deep into data. Though it can be complex, grasping the fundamentals of KQL is straightforward and empowers users with its versatility.

While KQL resources may be somewhat limited compared to other coding languages, the Microsoft documentation proves invaluable as an essential resource. Everything you need to know about KQL can be found there.

In this article, we'll cover the basics of KQL, operators, and provide additional resources to aid your learning journey.

## The Basics

Like many other query languages we search data stored in tables. The aim is to refine, transform and augment this data until we achieve a desired outcome. KQL makes data malleable through the use of queries. Queries are read-only operations, which can be performed against the tables of data ingested into the storage cluster.

Lets look at an example. Say we have a table called `Calendar`, which has three columns `Day`, `Month` and `Year`. We can look at all of the data in the table by just using a ‘flat search’ against `Calendar`:

```sql
Calendar
```

When we want to start refining or transforming the data we use keywords called (query) operators. Query operators are usually delimited by a `|` (pipe). A list of common operators and their descriptions can be found in the next section, but first lets look at a simple example with a popular operator `where`. To view all of the data in our Calendar table, when the `day` column is equal to 25, we would use the `where` operator as follows:

```sql
Calendar
| where Day == 25
```

KQL’s syntax is logical, not only should the code be understandable by the machine but easily-readable as well. 

## Operators

Operators are command words, they are what provide most of the functionality we have in KQL. If KQL is a toolbox, then operators are the tools contained within. There are a tonne of different operators which you can find [here](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator), in the Microsoft documentation. In this blog we are going to look at just a few of the most common ones.

| Operator Name | Description |
| --- | --- |
| [where](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/whereoperator) | where is used to filter data to a defined condition. It returns results for which the condition is true. Using where clauses as early as possible in your query, will usually improve overall query performance (something we will talk about later on in this series).  |
| [extend](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator) | extend is used to augment data in the existing table. Extend allows you to create new columns based on an expression.  |
| [summarize](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator) | summarize allows you to aggregate the input data.  Summarising uses special functions called [aggregation functions](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/aggregation-functions), like `count()`. You then group the aggregation by a GroupExpression, typically column(s) from the input data. The operator returns as many records as there are distinct values in all of the GroupExpressions.  |
| [project](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator) | project is a simple operator that defines which columns will be displayed in the output. It’s useful for large tables where you only need to return relevant information. |
| [join](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/joinoperator?pivots=azuredataexplorer) | join is an operator common across all query languages. It is used to connect multiple data tables together through the use of a shared data column. Data from both tables are merged into a single row in the output, by the matching values in the shared column. |
| [union](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/unionoperator?pivots=azuredataexplorer) | union is similar to the join operator in that data from two sources are aggregated, but there is no requirement of a shared column. With union, all rows in all tables are returned. |
| [render](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/renderoperator?pivots=azuredataexplorer) | render is the operator used to create graphs in KQL. You can create a whole host of graphs, maps and charts. But we will talk about these later in the series. |

If you’re like me then reading these won’t make much sense, you’ll need to see them in action. In all honesty, to learn any sort of language you need to practice. You can read all of the documentation and tutorials in the world, but nothing helps you learn coding as good as actually practicing for yourself. Take a look at the next section to help you get started. 

## Resources

### Azure Data Explorer (ADX)

The first thing you are going to need is a development environment to start learning. Microsoft provide a free public cluster to start learning KQL [here](https://dataexplorer.azure.com/publicfreecluster). You’ll have to set up a Microsoft account, and then create an instance of the cluster. Once configured, you can go to Home and start practicing with some sample data:

![Azure Data Explorer](/assets/img/HowToKQL/adx.png)
Azure Data Explorer isn’t security focussed in terms of data, but it will give you a fundamental understanding of how the language works and it’s where I recommend you start playing around with KQL. You have to start somewhere!

### Chat GPT

Now, I am not suggesting that you load Chat GPT and start hammering it to write all of your KQL, but I am a big believer in using the tools we have available to learn and assist us. AI is here and it’s not going anywhere fast, so lets use it to our advantage. A great use of Chat GPT is to ask questions to speed up the learning process. There are tonnes of KQL keywords and functions, and asking Chat GPT to tell you which one you want is a great way to learn faster.

![Chat-GPT](/assets/img/HowToKQL/chatgpt.png)

### Microsoft Learn

If you haven’t got a [Microsoft Learn](https://learn.microsoft.com/en-us/training/) account and you want to learn Microsoft technologies, create one now! There are hundreds of modules and training paths to go through, [including some on KQL](https://learn.microsoft.com/en-us/training/browse/?terms=kql): 

[Write your first query with Kusto Query Language - Training](https://learn.microsoft.com/en-us/training/modules/write-first-query-kusto-query-language/)

[Explore the fundamentals of data analysis using Kusto Query Language (KQL) - Training](https://learn.microsoft.com/en-us/training/modules/explore-fundamentals-kql/)

[Construct KQL statements for Microsoft Sentinel - Training](https://learn.microsoft.com/en-us/training/modules/construct-kusto-query-language-statements/)

## Conclusion

In this blog I gave you an overview of KQL, we looked at some of the basics of the Kusto Query Language. Remember to always start with your table and work your way down, if you can’t find what your looking for, broaden your search!

We then went on to talk about some of the most common operators, which will help you write powerful queries. Please read the Microsoft documentation for more information on these operators, or stay tuned for the next post in this series, where we will deep dive into them with some practical examples.

This was a really information heavy blog, I suggest that you go away and understand all of these points before going any further. Once you feel like you understand them, practice, practice, practice by using the free resources in the last section. 

If you have any questions, I am always happy to help, if you drop me a message on LinkedIn! And remember… Keep Calm and KQL.