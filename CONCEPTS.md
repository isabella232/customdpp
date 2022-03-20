# Concepts

## Display Post Processing (DPP) Overview

Microsoft Speech service can be viewed as two components, speech recognition (SR) and display post processing (DPP). SR service transcripts audio to lexical text, and DPP service transforms the lexical text to display text.

![The relationship of SR and DPP](SRDPP.png)

The DPP service is essential a display post processing pipeline. The display post processing pipeline is composed by a sequence of display format builders. Each builder corresponds to a display format feature, such as *ITN*, *capitalization*, and *profanity masking/removal*, etc..

Inverse Text Normalization (ITN)
: A feature to convert the text of spoken form numbers to display form. For example, "I spend twenty dollars" -> "I spend $20".

Capitalization
: To upper case entity names, acronyms, or the first letter of a sentence. For example, "she is from microsoft" -> "She is from Microsoft".

Profanity Masking/Removal
: Masking or removal profanity words from a sentence. For example, assuming "abcd" is a profanity word, then by profanity masking, "I never say abcd" -> "I never say ****".

Beside the base features maintained by Microsoft for the general purpose display processing tasks, the DPP pipeline also provides three customizable features to meet customers' domain specific requirements.

Custom ITN
: Extend the functionalities of base ITN, by applying a rule based ITN model from customer

Rewrite
: At the end of pipeline, rewrite one phrase to another based on a rule based model from customer, each rule in the model is a pair of phrases (old -> new).

Custom Profanity
: Perform profanity handling based on the profanity word list from customer.

The order of features in the DPP pipeline is illustrated in below diagram.

![The features of DPP pipeline](PIPELINE.png)


## What's in Custom DPP?

Custom DPP is a service and its corresponding tool set to allow customers to customize certain features of Microsoft DPP service.

The Custom DPP service provides a set of REST API to create, evaluate, and deploy a Custom DPP model. A Custom DPP model is a collection of the custom models for *Custom ITN*, *Rewrite* and *Custom Profanity*.

For ease of use, Custom DPP also includes a command line utility named Custom DPP CLI. The Custom DPP CLI interacts with Custom DPP service to upload the rules of a custom model, start an evaluation or deployment, etc..

In a Custom DPP model, there are three kinds of rules, *ITN*, *Rewrite*, and *Profanity*, which define the corresponding custom models of the three customizable features, *Custom ITN*, *Rewrite* and *Custom Profanity*.

## *INT* Rules

The rules to build a *Custom ITN* model.

## *Rewrite* Rules

The rules to define the behaviors of the *Rewrite* feature.

## *Profanity* Rules

The list of profanity words for the *Custom Profanity* feature.
