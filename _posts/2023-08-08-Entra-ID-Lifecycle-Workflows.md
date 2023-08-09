---
title: Entra ID Lifecycle Workflows
date: 2023-08-05 22:24:00 +0000
categories: [The Engine Room, Access and Identity]
tags: [entra id, lifecycle workflows]
pin: true
comments: true
math: true
mermaid: true
image:
  path: /assets/img/CoverImages/EngineLifecycleWorkflows.png
  alt: Lifecycle Workflows Cover
---

## Introduction

A new feature in Entra ID, Lifecycle Workflows allows organisations to automate changes to user objects throughout three key stages in a user’s lifecycle. These three key stages are:

- Joiners - When a user is first created within the organisation.
- Movers - When a user moves internally within the organisation.
- Leavers - When a user leaves the organisation.

Managing users can be a significant undertaking for larger companies. Lifecycle Workflows allow IAM teams to standardise processes, save time, and operate more efficiently through the use of automation. I’ve honestly lost track of the amount of times I have seen businesses who haven’t removed user licenses after someone has left, or seen new joiners twiddling their thumbs because they haven’t got the access they need on their first day of work. Lifecycle Workflows aims to make these admin oversights a thing of the past.

Lifecycle workflows consist of two main features, **tasks** and **execution conditions**. Let’s say we have a new joiner starting at our company called Bob. 

An example of some **tasks** could be to assign Bob all the groups he needs to get him started on his first day, email the manager with a temporary access pass and enable the account. 

**Execution conditions** define for who and when the task is run. We can set the workflow to be the day Bob joins so everything is ready to go. This allows IAM teams to create the user account straight away and leave it in a disabled state, as long as the workflows are configured correctly then there should be no more actions to be taken. 

Automatic workflows run off of triggers, which are based on user attributes such as the employee start date, but triggers can also be run on demand where needed.

## Licensing Requirements

Lifecycle Workflows requires an add on to Entra ID P1 or P2 licensing called Microsoft Entra ID Governance. The license is required for all users that you wish to govern using Entra Identity Governance so costs could quickly mount up for large organisations. At the time of writing, licenses are £5.80 per user per month in addition to the P1 or P2 licensing, but there is a promotional offer if you buy the licenses before the end of the year.

Essentially, in terms of cost I don’t think you would see much return on investment if you only utilised this feature in Identity Governance but there are [other features](https://www.microsoft.com/en-gb/security/business/microsoft-entra-pricing?rtc=1) included in this licensing add-on too, albeit not very many.

![Entra ID governance add-on features](/assets/img/EngineRoom/licenseFeatures.png)

## Custom Extensions

Let’s not mince words, custom extensions are logic applications. As well as the list of built in tasks, you can also create logic applications to run inside of the workflow. This really does open up a huge range of possibilities to be included within the workflow. You can create the logic apps from scratch if you wish, but creating them through the workflow lifecycle means the application is set up ready to go with all of the correct triggers.

![](/assets/img/EngineRoom/customExtensions.png)

![](/assets/img/EngineRoom/logicAppTrigger.png)

We won’t go through how to create logic applications here, but let’s touch on a few ideas.

### Joiners

1. Assign Azure RBAC roles through the Microsoft Graph API.
2. Post a staff update to Viva Engage or LinkedIn.
3. Arrange meetings for the first day.

### Leavers

1. Delegate access to a mailbox or set up a mail forwarding rule.
2. Perform recent activity auditing through compliance and security by using the Microsoft Graph API. 

## Conclusion

Throughout this article we have explored what lifecycle workflows are in Entra ID. We have spoken about how utilising lifecycle workflows helps automate tasks at different stages of user lifetimes, and how this helps to improve efficiencies. We also explored what kind of licensing is required to employ this feature and finally how we can extend the functionality by using custom extensions. 

I want to close of this article by answering a question, are Lifecycle Workflows really worth the extra cost? In my opinion, no they aren’t (at least not yet). Considering you can do everything that lifecycle workflows offer with logic apps, I really don’t see the benefit for large organisations. Yes, Lifecycle Workflows allow you to centralise the management of these actions and audit the runs but I don’t think this warrants a £5.80 per user price tag. As for the other features included in the licensing, there isn’t much value add there in my opinion. I can see Microsoft extending the default workflows in due course but really, at the moment the features out of the box aren’t rich enough.

So in summary, the feature is a great idea for small companies, but managing even a company of one hundred users will end up costing £580 per month. A hefty price to pay for capabilities that could be achieved through a single logic application.