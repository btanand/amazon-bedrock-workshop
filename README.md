# Amazon Bedrock Workshop

This hands-on workshop, aimed at developers and solution builders, introduces how to leverage foundation models (FMs) through [Amazon Bedrock](https://aws.amazon.com/bedrock/).

Amazon Bedrock is a fully managed service that provides access to FMs from third-party providers and Amazon; available via an API. With Bedrock, you can choose from a variety of models to find the one that’s best suited for your use case.

Within this series of labs, you'll explore some of the most common usage patterns we are seeing with our customers for Generative AI. We will show techniques for generating text and images, creating value for organizations by improving productivity. This is achieved by leveraging foundation models to help in composing emails, summarizing text, answering questions, building chatbots, and creating images. You will gain hands-on experience implementing these patterns via Bedrock APIs and SDKs, as well as open-source software like [LangChain](https://python.langchain.com/docs/get_started/introduction) and [FAISS](https://faiss.ai/index.html).

Labs include:

- **Text Generation** \[Estimated time to complete - 30 mins\]
- **Text Summarization** \[Estimated time to complete - 30 mins\]
- **Questions Answering** \[Estimated time to complete - 45 mins\]  
- **Chatbot** \[Estimated time to complete - 45 mins\]
- **Image Generation** \[Estimated time to complete - 30 mins\]
- **Code Generation** \[Estimated time to complete - 30 mins\]

<div align="center">

![imgs/11-overview](imgs/11-overview.png "Overview of the different labs in the workshop")

</div>

You can also refer to these [Step-by-step guided instructions on the workshop website](https://catalog.us-east-1.prod.workshops.aws/workshops/a4bdb007-5600-4368-81c5-ff5b4154f518/en-US).


## Getting started

### Choose a notebook environment

This workshop is presented as a series of **Python notebooks**, which you can run from the environment of your choice:

- For a fully-managed environment with rich AI/ML features, we'd recommend using [SageMaker Studio](https://aws.amazon.com/sagemaker/studio/). To get started quickly, you can refer to the [instructions for domain quick setup](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-quick-start.html).
- For a fully-managed but more basic experience, you could instead [create a SageMaker Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html).
- If you prefer to use your existing (local or other) notebook environment, make sure it has [credentials for calling AWS](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).


### Enable AWS IAM permissions for Bedrock

The AWS identity you assume from your notebook environment (which is the [*Studio/notebook Execution Role*](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html) from SageMaker, or could be a role or IAM User for self-managed notebooks), must have sufficient [AWS IAM permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) to call the Amazon Bedrock service.

To grant Bedrock access to your identity, you can:

- Open the [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iam/home?#)
- Find your [Role](https://us-east-1.console.aws.amazon.com/iamv2/home?#/roles) (if using SageMaker or otherwise assuming an IAM Role), or else [User](https://us-east-1.console.aws.amazon.com/iamv2/home?#/users)
- Select *Add Permissions > Create Inline Policy* to attach new inline permissions, open the *JSON* editor and paste in the below example policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "BedrockFullAccess",
            "Effect": "Allow",
            "Action": ["bedrock:*"],
            "Resource": "*"
        }
    ]
}
```

> ⚠️ **Note:** With Amazon SageMaker, your notebook execution role will typically be *separate* from the user or role that you log in to the AWS Console with. If you'd like to explore the AWS Console for Amazon Bedrock, you'll need to grant permissions to your Console user/role too.

For more information on the fine-grained action and resource permissions in Bedrock, check out the Bedrock Developer Guide.


### Clone and use the notebooks

> ℹ️ **Note:** In SageMaker Studio, you can open a "System Terminal" to run these commands by clicking *File > New > Terminal*

Once your notebook environment is set up, clone this workshop repository into it.

```sh
git clone https://github.com/aws-samples/amazon-bedrock-workshop.git
cd amazon-bedrock-workshop
```

Because the service is in preview, the Amazon Bedrock SDK is not yet included in standard releases of the [AWS SDK for Python - boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html). Run the following script to download and extract custom SDK wheels for testing Bedrock:

```sh
bash ./download-dependencies.sh
```

This script will create a `dependencies` folder and download the relevant SDKs, but will not `pip install` them just yet.

You're now ready to explore the lab notebooks! Start with [00_Intro/bedrock_boto3_setup.ipynb](00_Intro/bedrock_boto3_setup.ipynb) for details on how to install the Bedrock SDKs, create a client, and start calling the APIs from Python.


## Content

This repository contains notebook examples for the Bedrock Architecture Patterns workshop. The notebooks are organised by module as follows:

### Intro

- [Simple Bedrock Usage](./00_Intro/bedrock_boto3_setup.ipynb): This notebook shows setting up the boto3 client and some basic usage of bedrock.

### Generation

- [Simple use case with boto3](./01_Generation/00_generate_w_bedrock.ipynb): In this notebook, you generate text using Amazon Bedrock. We demonstrate consuming the Amazon Titan model directly with boto3 
- [Simple use case with LangChain](./01_Generation/01_zero_shot_generation.ipynb): We then perform the same task but using the popular framework LangChain
- [Generation with additional context](./01_Generation/02_contextual_generation.ipynb): We then take this further by enhancing the prompt with additional context in order to improve the response.

### Summarization

- [Small text summarization](./02_Summarization/01.small-text-summarization-claude.ipynb): In this notebook, you use use Bedrock to perform a simple task of summarizing a small piece of text. 
- [Long text summarization](./02_Summarization/02.long-text-summarization-titan.ipynb): The above approach may not work as the content to be summarized gets larger and exceeds the max tokens of the model. In this notebook we show an approach of breaking the file up into smaller chunks, summarizing each chunk, and then summarizing the summaries.

### Question Answering

- [Simple questions with context](./03_QuestionAnswering/00_qa_w_bedrock_titan.ipynb): This notebook shows a simple example answering a question with given context by calling the model directly. 
- [Answering questions with Retrieval Augmented Generation](./03_QuestionAnswering/01_qa_w_rag_claude.ipynb): We can improve the above process by implementing an architecure called Retreival Augmented Generation (RAG). RAG retrieves data from outside the language model (non-parametric) and augments the prompts by adding the relevant retrieved data in context.

### Chatbot

- [Chatbot using Claude](./04_Chatbot/00_Chatbot_Claude.ipynb): This notebook shows a chatbot using Claude
- [Chatbot using Titan](./04_Chatbot/00_Chatbot_Titan.ipynb): This notebook shows a chatbot using Titan

### Text to Image

- [Image Generation with Stable Diffusion](./05_Image/Bedrock%20Stable%20Diffusion%20XL.ipynb): This notebook demonstrates image generation with using the Stable Diffusion model

### Code Generation, SQL Generation, Code Translation and Explanation

1. [Code Generation](./06_CodeGeneration/00_code_generatation_w_bedrock.ipynb): Demonstrates how to generate Python code using Natural language. It shows examples of prompting to generate simple functions, classes, and full programs in Python for Data Analyst to perform sales analysis on a given Sales CSV dataset.

2. [Database or SQL Query Generation](./06_CodeGeneration/01_sql_query_generate_w_bedrock.ipynb) : Focuses on generating SQL queries with Amazon Bedrock APIs. It includes examples of generating both simple and complex SQL statements for a given data set and database schema. 

3. [Code Explanation](./06_CodeGeneration/02_code_interpret_w_langchain.ipynb) : Uses Bedrock's foundation models to generate explanations for complex C++ code snippets. It shows how to carefully craft prompts to get the model to generate comments and documentation that explain the functionality and logic of complicated C++ code examples. Prompts can be easily updated for another programming languages.

4. [Code Translation ](./06_CodeGeneration/03_code_translate_w_langchain.ipynb) : Guides you through translating C++ code to Java using Amazon Bedrock and LangChain APIs. It shows techniques for prompting the model to port C++ code over to Java, handling differences in syntax, language constructs, and conventions between the languages.
