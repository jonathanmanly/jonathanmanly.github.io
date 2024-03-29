---
layout: default
title: projects
permalink: /projects/custServClass/
theme: jekyll-theme-hacker
---
# Customer Service Classifier

Try it out [live on this demo page!](https://huggingface.co/spaces/jonmanly/custServiceClassifier)

----

At a prior employer we once did a project to automatically triage and respond to written customer service inquiries.  It was a great project, using some internally crowdsourced data coding, and a bag of words classifier model.  Since that time, we've seen the advent of large language models (LLM).  I wanted to see how a project like this could be produced leveraging the new technologies!

# Goal

The goal of this project will be to automatically code customer service inquiries into useful categories, and to measure the customer's sentiment.  These two pieces of information could be used in a business process to route the inquiry to an auto-responder, or to a human representative (ex. for angry customers with specific problems).

**For example :**
_My order has not been delivered yet.  Can you check the status of the shipment?_ coded as **TRACK ORDER, with NEGATIVE sentiment.**


# Training Data

This example runs using the a data set of customer service inquiries across a number of categories.  This is available in a [github repo](https://github.com/bitext/customer-support-llm-chatbot-training-dataset), and each of the inquiries is coded into one of 27 categories (below).  The training data also includes a reponse, and this will be used in [a different project.](/projects/custServClass)

Categories in dataset : 

    ['edit_account',
    'switch_account',
    'check_invoice',
    'complaint',
    'contact_customer_service',
    'delivery_period',
    'registration_problems',
    'check_payment_methods',
    'contact_human_agent',
    'payment_issue',
    'newsletter_subscription',
    'get_invoice',
    'place_order',
    'cancel_order',
    'track_refund',
    'change_order',
    'get_refund',
    'create_account',
    'check_refund_policy',
    'review',
    'set_up_shipping_address',
    'delivery_options',
   'delete_account',
    'recover_password',
    'track_order',
    'change_shipping_address',
    'check_cancellation_fee']      
 

# Approach

In this project, I used the Huggingface library running on a python GPU environment.  The code is available in [this repo](https://github.com/jonathanmanly/custServiceLLM/blob/main/custservice_classifier.ipynb).

This would be a fairly easy project for a fully-developed LLM such as ChatGPT to execute, potentially without any fine tuning (using few-shot or zero-shot learning).  However, I wanted to use a relatively small model, and simulate building a system that could be run on-site, without incurring token costs or sending company data to a third party.  I chose to use a fine-tuning approach using the [DistilBERT](https://huggingface.co/docs/transformers/en/model_doc/distilbert) pre-trained open source model.  Huggingface makes this pretty easy to instantiate a local copy of the model, adding a softmax classifier head with random weights on top.  Our job is to use the training data to train the weights in the new classification layer to make accurate predictions as to which category each customer service inquiry belongs to, effectively creating a custom standalone model.

# Modeling

I ran two versions of the fine tuning process, with different amounts of training data.  In the actual business project I was involved in, we did not have pre-coded data for our classifications, and we needed to ask a number of folks to manually code data.  I think this is a common situations for businesses taking on NLP projects, and so I ran a version of the project with a random sample of only 870 training records.  Alternatively, a company could have sufficient coding as part of their existing contact center process, in which case, you could have a lot of coded data available, so I also ran a version with over 18,000 training records.  In both cases, I held out a random sample to evaluate the model's progress during the training process.

Both phases of the modeling converged very quickly and did a great job classifying the customer inquiries.  The small sample acheived neary 75.2% accuracy after only one pass!  After 6 epochs it converged at over 98.5% accuracy, which while not perfect, is clearly good enough to work with in a business context and provide value.

<img src="eval_acc_smallSample.png" width="50%">

<img src="confusion_small.png" width="50%">

We can see in this confusion matrix that there are a couple of classes where the model had difficulty correctly classifying (as shown by the few squares off the central line). It is confusion check_cancellation fee with cancel_order, and delete_account with cancel_order.  Let's see what the effect of more data could do...


<img src="eval_acc_largeSample.png" width="50%">

Right out of the gate, the large dataset already achieved superior accuracy on the evaluation set with only one epoch (which of course was more expensive computationally due to the larger size).  Within 3 epochs the evaluation set accuracy was nearly perfect at 99.8% on a hold out evaulation set, and the confusion matrix does not show the issue from the smaller dataset!

<img src="confusion_large.png" width="50%">

# Application

[Try out the demo app](https://huggingface.co/spaces/jonmanly/custServiceClassifier) on Huggingface spaces which uses my custom model to produce the top 3 categories, along with their softmax scores (essentially a confidence score).  I also use [a pretrained version of distilbert](https://huggingface.co/distilbert/distilbert-base-uncased-finetuned-sst-2-english) to calculate sentiment without any further customizations.

[The model](https://huggingface.co/jonmanly/custServiceClassifier) is hosted on [Huggingface](https://huggingface.co/) if you'd like to download it to experiment with it in your own learnings.

One advantage over using a fine tuned LLM over a bag of words approach is it is more robust against unexpected language usage.  In the demo app, I added a couple of overly wordy inquiries, using language styles of Shakespeare and Capt. Jack Sparrow, neither of which appeared in the training data at all.  This is sort of an extreme test, but the model is able to make sense of both.  Try it out for yourself!

<img src="screenshotDemo.png" width="75%">


