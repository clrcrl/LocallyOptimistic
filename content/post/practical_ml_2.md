---
author: "Ilan Man"
title: "Model Building in a Dynamic Environment"
cover: "/img/practical_ml_2.jpg"
tags: ["datascience"]
date: "2018-06-03"
draft: false
---

A lot of Data Science literature focuses on the mathematical and algorithmic aspects of model building. This is after all what much of academia spends time grappling with. However the Data practitioner understands that applying Data Science and Machine Learning models to solve real-world problems involves much more than coding a statistical formula. Today's post will be about keeping your models fresh and up to date, and your team informed as your data world evolves. We will discuss some implications of a changing data distribution on your model, practical technical considerations when building a model that is integrated with the product application, and how presenting to your team can be a great checkpoint on your model building progress.
<!--more-->
 
To help illustrate and motivate these considerations, we will use a spam filter as our running Data Science model. You can imagine that people use our platform for sending spam emails with subject lines such as *“Congratulations, you’ve been approved!”*. Our goal is to filter out spammy emails from non-spammy emails.
 
## Training data != Future data
 
One of the most fundamental goals of a Data Scientist is to build a model that performs well on new, unseen data. In technical parlance, this means building a model that minimizes the [generalization error](https://en.wikipedia.org/wiki/Generalization_error?oldformat=true). Many Data Scientists believe that if they build their model using best practices, then after deploying to production they can put their feet up, have a drink and call it a day. Unfortunately, the real world is not so accommodating.
 
Consider our spam filter. Say that we have manually identified a number of spammy and non-spammy messages and proceed to build a model that, based on our historic data, does an excellent job of classifying spam. **Our model is only as good at identifying what spammy emails look like, as the extent of diverse spammy emails exist in our data set**. In other words, if we get a new example of spam that looks completely different from anything we have yet seen, then our model is less likely to catch it. 
 
Spam filters are a special class of problems because spam is created by malcontents. These individuals (or groups) continuously try to poke holes in our system. They have a strong incentive to ensure that future spam looks different from past spam. What's more, these bad actors can detect what patterns our model has identified as likely to be spam, and adjust their methods accordingly. As a result, our future data will **always** look different from the past.
 
Spam is an extreme example of a changing data distribution. However, the notion that future data will look different from your training data is a reality that pervades all types of problems. Factors that can impact your data distribution include:
 
* *Seasonality*: Most businesses are sensitive to different seasons of the year, days of the month or week. This requires changing model assumptions to account for this. 
* *Marketing efforts*: Different types of campaigns will attract different types of users, with different behaviors. 
* *Changes in Regulatory landscape*: [GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation?oldformat=true) is a prime example of this. What happens when we have to delete user data that our model has been leveraging?
* *Product launches*: It is very common for tech startups to pivot unexpectedly. This wreaks havoc for our models.
 
In order to combat the reality of a changing data distribution, the Data Scientist must retrain her model at regular intervals, perform feature engineering as needed and potentially change the entire algorithm that underlies her model. While we can strive to minimize our model's generalization error, we must recognize that our job is not complete just because we hit deploy.
 
## Technical considerations
 
Unless the Data Scientist's model is deployed on production, it usually runs on a Data Warehouse and likely does not interact with the application. This may result in the Data Scientist cutting some corners; it is not uncommon to sacrifice model speed for increased accuracy. After all, if a model is not used in production then stakeholders don't need real-time results, making optimization a "nice to have". In addition, Data Scientists are often not trained in software engineering best practices. A Data Scientist’s model is usually a script having minimal interaction with a larger application. The range of a model’s inputs and outputs are much more narrow and easily controlled.
 
However, consider our spam filter. Every time a user sends an email we must classify it as spam or not. Our filter is part of a series of checkpoints between when the user hits *send* and when the email gets delivered in their recipient's inbox. That is very real-time. In this paradigm, optimization and efficiency are critical. If we trained a heavy model, however accurate, that slowed down our email deliverability, it would never see the light of day. 

Moreover, since this classifier interacts with the application, it must in some way be integrated with the greater application infrastructure. This means a much higher bar with regards to testing, exception handling, logging and other software engineering best practices. For example, it is very common and indeed good practice to leverage existing libraries in your model. However, more dependence on external libraries means more frequent maintenance and testing of functionality to ensure everything works as expected. If your model is a standalone script then debugging dependency related errors may be nothing more than a nuisance; for models that are integrated with the application or are expected to run real-time, this type of extra overhead can be a non-starter.
 
In this context, the Data Scientist quickly realizes that her statistical model is but a very small part in a large web of interconnected processes. As a result, it may be prudent to spend less time tweaking a model to achieve some theoretical guarantee and more time understanding how it will fit in the bigger picture.
 
## Show and tell

The model building process can be long, complex and oftentimes can be difficult to judge progress. You can spend days testing different features and algorithms only later to realize that your assumptions were incorrect and you need to go back and approach the problem from a completely different angle. To help ensure you are making progress, I recommend that you present your model with regularity. Presentations can be formal or informal, technical or non-technical, and the benefits include:
 
* *Avoid rabbit holes*: Periodically showing your progress to stakeholders or peers forces you out of the weeds and helps you to summarize results. Often, this self-organization and peer feedback will help you to realize that a particular rabbit hole may not be worth the effort. 
* *Reinforce understanding*: There is no better indication that you have a solid understanding of your work than being able to clearly explain it to someone else, especially those less technical.
* *Forced articulation*: Model building involves making numerous assumptions along the way. Sometimes simplifying assumptions are made in order not to lose momentum during the model building process. It is critical to return to those assumptions and correct them if necessary, lest they grow into trouble areas if your model becomes very sensitive to them. Presentations form a natural stopping point, forcing you to address your assumptions. It is foolhardy to present work with half-baked assumptions that you did not account for ahead of time.
* *New perspectives*: The Data Scientist should be the expert in building a good Machine Learning model. But even experts can [learn from others]({{< ref "practical_ml.md" >}}). Do not underestimate the value in getting a different perspective.
* *Show off*: Business leaders are not generally enamored with siloed functions, especially functions that are esoteric and whose value is difficult to quantify. Data Science functions can sometimes fall into this category partly because in general we are not great at selling ourselves, explaining our methodologies or communicating how we add value. Presentations are a great way to counter that.
 
## Conclusion
 
Despite its name Data Science is both a science *and* an art. In practical real-world settings the Data Scientist must balance the tension between business constraints and scientific correctness. In this post we highlighted some of these tensions, such as the changing data distribution on which a model is built and maintained, technical considerations that vary depending on your model application, and some of the value in presenting your model well before completion.

If you found the topics here useful, please see my earlier post which discussed a similar theme on [practical Data Science]({{< ref "practical_ml.md" >}}). There are certainly more considerations to be aware of when performing Data Science in a business context (e.g., how to get trained data?) but hopefully those covered in these two posts are helpful for Data Scientists trying to add value and build useful and meaningful data products.
