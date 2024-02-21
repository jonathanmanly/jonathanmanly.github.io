---
layout: default
title: projects
permalink: /projects/convBI/
theme: jekyll-theme-hacker
---
# Customer Service Classifier

Try it out live on this demo!  If the webpage tells you the app is sleeping, feel free to wake it up and try again in a couple of minutes.  [Live Customer Service Classifier](https://huggingface.co/spaces/jonmanly/custServiceClassifier)

----

At a prior employer we once did a project to automatically triage and respond to written customer service inquiries.  It was a great project, using some crowd sourced data coding across the company, and a bag of words coding model.  Since that time, we've seen the advent of large language models (LLM), both closed and open source kinds.  I wanted to see how a project like this could be produced leveraging the new technologies!

# Goal

The goal of this project will be to automatically code customer service inquiries into useful categories, and to measure the customer's sentiment.  These two piece of information could be used in a business to route the inquiry to an auto-responder, or to a human representative (ex. for angry customers with specific problems).

# The Data

This example runs using the a data set of customer service inquiries across a number of categories.  This is available in a [github repo](https://github.com/bitext/customer-support-llm-chatbot-training-dataset), and each of the inquiries is coded into one of X categories.  It also includes a reponse, but this is not used in this project (yet!).

# The Process

In this project, I used the Huggingface library running on a python GPU environment.  The code is available in [this repo](https://github.com/jonathanmanly/custServiceLLM/blob/main/custservice_classifier.ipynb).



Images here

<img src="convBIScreenShot1.jpg" width="100%">
