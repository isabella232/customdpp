# Concepts

## Display Post Processing (DPP) Overview

Microsoft Speech service can be viewed as two components, speech recognition (SR) and display post processing (DPP). SR service transcripts audio to lexical text, and DPP service transforms the lexical text to display text.

![The relationship of SR and DPP](SRDPP.png)

The DPP service is essential a display post processing pipeline. The display post processing pipeline is composed by a sequence of display format builders. Each builder corresponds to a display format feature, such as *ITN*, *capitalization*, and *profanity masking/removal*, etc..

*Inverse Text Normalization (ITN)*  

A feature to convert the text of spoken form numbers to display form. For example, 
> `"I spend twenty dollars" -> "I spend $20"`

*Capitalization*  

To upper case entity names, acronyms, or the first letter of a sentence. For example, 
> `"she is from microsoft" -> "She is from Microsoft"`

*Profanity Masking/Removal*  

Masking or removal profanity words from a sentence. For example, assuming "abcd" is a profanity word, then by profanity masking, 

> `"I never say abcd" -> "I never say ****"`

Beside the base features maintained by Microsoft for the general purpose display processing tasks, the DPP pipeline also provides three customizable features to meet customers' domain specific requirements.

*Custom ITN*   
Extend the functionalities of base ITN, by applying a rule based custom ITN model from customer.

*Rewrite*  
At the end of pipeline, rewrite one phrase to another based on a rule based model from customer, each rule in the model is a pair of phrases (old -> new).

*Custom Profanity*  
Perform profanity handling based on the profanity word list from customer.

The order of features in the DPP pipeline is illustrated in below diagram.

![The features of DPP pipeline](PIPELINE.png)

## What's in Custom DPP?

Custom DPP is a service and a tool set to allow customers to customize certain features of Microsoft DPP service.

The Custom DPP service provides a set of REST API to create, evaluate, and deploy a Custom DPP model. A Custom DPP model is a collection of the custom models for *Custom ITN*, *Rewrite* and *Custom Profanity*.

For ease of use, Custom DPP also includes a command line utility named Custom DPP CLI. The Custom DPP CLI interacts with Custom DPP service to upload the rules of a custom model, start an evaluation or deployment, etc..

In a Custom DPP model, there are three kinds of rules, *ITN*, *Rewrite*, and *Profanity*, which define the corresponding custom models of the three customizable features, *Custom ITN*, *Rewrite* and *Custom Profanity*.


## Custom ITN

### How *Custom ITN* model works?

A *Custom ITN* model is composed by two parts, a mini base ITN model provided by Microsoft, and a custom model built from the *ITN* rules provided by customers.

For a *input string*, 
1. First, the mini base ITN model will transduce the number related phrases in the *input string* to display forms
2. Then, for the output of the mini base ITN model, the custom model will match and transduce it to the desired format defined by the *ITN* rules of the model. The matching algorithm inside the Custom ITN model is case-insensitive.

### Rule Syntax

A *Custom ITN* model is built from a set of *ITN* rules. An *ITN* rule is a regular expression like pattern string which describes 

- A matching pattern of the input string
- The desired format of the output string

![ITN rule, match + transduce](ITNRULE.png)

> Note <br/>
In private preview, Custom ITN only supports on en-us.

#### Input string

The string that a *ITN* rule is trying to match and transduce.

#### Output string

The display format string which is transduced by a *ITN* rule from the input string.

#### Atoms in an *ITN* rule

An *ITN* rule is composed of a sequence of atoms. An atom is a single point within the rule which it tries to match and then transduce an input string. There are three kinds of atoms. 

- Matching atom
  
  A literal character or a [character classes](CONCEPTS.md#character-classes) which matches the input string and output it directly without any transduction. It is also called *identity transducer atom*.

- Transducer atom
  
  A transducer that matching a literal character (case-insensitive) or a [character classes](CONCEPTS.md#character-classes) and transduce the input character to the desired one defined by the atom.

- Insert atom
  
  A symbol or punctuation character which insert to the output string directly without any matching.

#### Character classes

Like regular expression, there are several pre-defined character classes for a *ITN* rule:

* `\d` - match a digit from '0' to '9', and output it directly
* `\l` - match a letter (case-insensitive) and transduce it to lower case
* `\u` - match a letter (case-insensitive) and transduce it to upper case
* `\a` - match a letter (case-insensitive) and output it directly
* `\\` - match and output the char '\'

### Examples

#### Group digits

To group 6 digits into 2 groups and add a '-' char between them:

> *ITN* rule: `\d\d\d-\d\d\d` <br/>
Sample: `"cadence one oh five one fifteen" -> "cadence 105-115"`

#### Format an film name

*Space: 1999* is a famous film, to support it:

> *ITN* rule: `Space: 1999` <br/>
Sample: `"watching space nineteen ninety nine" -> "watching Space: 1999"`

#### Format a weapon name

To support the weapon names of AK family:

> *ITN* rule: `AK-\d\d` <br/>
Sample: `"a k forty seven" -> "AK-47"`


## Rewrite

### How *Rewrite* model works?

The *Rewrite* builder is at the end of the DPP pipeline. The model inside the builder is a collection of rewrite rules. For each input text,

1. The *Rewrite* model tries to match 

### Rule Syntax

The rules to define the behaviors of the *Rewrite* feature.
A rewrite rule is a pair of two phrases (old -> new). 

### Examples

## Custom Profanity

The list of profanity words for the *Custom Profanity* feature.

### How *Custom Profanity* works?

### Rule Syntax

The rules to define the behaviors of the *Rewrite* feature.

### Examples