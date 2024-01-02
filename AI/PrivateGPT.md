---
title: PrivateGPT
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# 🔒 PrivateGPT 📑

[![Tests](https://github.com/imartinez/privateGPT/actions/workflows/tests.yml/badge.svg)](https://github.com/imartinez/privateGPT/actions/workflows/tests.yml?query=branch%3Amain)

[![Website](https://img.shields.io/website?up_message=check%20it&down_message=down&url=https%3A%2F%2Fdocs.privategpt.dev%2F&label=Documentation)](https://docs.privategpt.dev/)

[![Discord](https://img.shields.io/discord/1164200432894234644?logo=discord&label=PrivateGPT)](https://discord.gg/bK6mRVpErU)

[![X (formerly Twitter) Follow](https://img.shields.io/twitter/follow/PrivateGPT_AI)](https://twitter.com/PrivateGPT_AI)

> Install & usage docs: https://docs.privategpt.dev/
> 
> Join the community: [Twitter](https://twitter.com/PrivateGPT_AI) & [Discord](https://discord.gg/bK6mRVpErU)

![Gradio UI](/fern/docs/assets/ui.png?raw=true)

PrivateGPT is a production-ready AI project that allows you to ask questions about your documents using the power

of Large Language Models (LLMs), even in scenarios without an Internet connection. 100% private, no data leaves your

execution environment at any point.

The project provides an API offering all the primitives required to build private, context-aware AI applications.

It follows and extends the [OpenAI API standard](https://openai.com/blog/openai-api),

and supports both normal and streaming responses.

The API is divided into two logical blocks:

**High-level API**, which abstracts all the complexity of a RAG (Retrieval Augmented Generation)

pipeline implementation:

- Ingestion of documents: internally managing document parsing,
splitting, metadata extraction, embedding generation and storage.
- Chat & Completions using context from ingested documents:
abstracting the retrieval of context, the prompt engineering and the response generation.

**Low-level API**, which allows advanced users to implement their own complex pipelines:

- Embeddings generation: based on a piece of text.
- Contextual chunks retrieval: given a query, returns the most relevant chunks of text from the ingested documents.

In addition to this, a working [Gradio UI](https://www.gradio.app/)

client is provided to test the API, together with a set of useful tools such as bulk model

download script, ingestion script, documents folder watch, etc.

> 👂 **Need help applying PrivateGPT to your specific use case?**
> [Let us know more about it](https://forms.gle/4cSDmH13RZBHV9at7)
> and we'll try to help! We are refining PrivateGPT through your feedback.

## 🎞️ Overview

DISCLAIMER: This README is not updated as frequently as the [documentation](https://docs.privategpt.dev/).

Please check it out for the latest updates!

### Motivation behind PrivateGPT

Generative AI is a game changer for our society, but adoption in companies of all sizes and data-sensitive

domains like healthcare or legal is limited by a clear concern: **privacy**.

Not being able to ensure that your data is fully under your control when using third-party AI tools

is a risk those industries cannot take.

### Primordial version

The first version of PrivateGPT was launched in May 2023 as a novel approach to address the privacy

concerns by using LLMs in a complete offline way.

That version, which rapidly became a go-to project for privacy-sensitive setups and served as the seed

for thousands of local-focused generative AI projects, was the foundation of what PrivateGPT is becoming nowadays;

thus a simpler and more educational implementation to understand the basic concepts required

to build a fully local -and therefore, private- chatGPT-like tool.

If you want to keep experimenting with it, we have saved it in the

[primordial branch](https://github.com/imartinez/privateGPT/tree/primordial) of the project.

> It is strongly recommended to do a clean clone and install of this new version of
PrivateGPT if you come from the previous, primordial version.

### Present and Future of PrivateGPT

PrivateGPT is now evolving towards becoming a gateway to generative AI models and primitives, including

completions, document ingestion, RAG pipelines and other low-level building blocks.

We want to make it easier for any developer to build AI applications and experiences, as well as provide

a suitable extensive architecture for the community to keep contributing.

Stay tuned to our [releases](https://github.com/imartinez/privateGPT/releases) to check out all the new features and changes included.

## 📄 Documentation

Full documentation on installation, dependencies, configuration, running the server, deployment options,

ingesting local documents, API details and UI features can be found here: https://docs.privategpt.dev/

## 🧩 Architecture

Conceptually, PrivateGPT is an API that wraps a RAG pipeline and exposes its

primitives.

* The API is built using [FastAPI](https://fastapi.tiangolo.com/) and follows

  [OpenAI's API scheme](https://platform.openai.com/docs/api-reference).

* The RAG pipeline is based on [LlamaIndex](https://www.llamaindex.ai/).

The design of PrivateGPT allows to easily extend and adapt both the API and the

RAG implementation. Some key architectural decisions are:

* Dependency Injection, decoupling the different components and layers.

* Usage of LlamaIndex abstractions such as `LLM`, `BaseEmbedding` or `VectorStore`,

  making it immediate to change the actual implementations of those abstractions.

* Simplicity, adding as few layers and new abstractions as possible.

* Ready to use, providing a full implementation of the API and RAG

  pipeline.

Main building blocks:

* APIs are defined in `private_gpt:server:<api>`. Each package contains an

  `<api>_router.py` (FastAPI layer) and an `<api>_service.py` (the

  service implementation). Each *Service* uses LlamaIndex base abstractions instead

  of specific implementations,

  decoupling the actual implementation from its usage.

* Components are placed in

  `private_gpt:components:<component>`. Each *Component* is in charge of providing

  actual implementations to the base abstractions used in the Services - for example

  `LLMComponent` is in charge of providing an actual implementation of an `LLM`

  (for example `LlamaCPP` or `OpenAI`).

## 💡 Contributing

Contributions are welcomed! To ensure code quality we have enabled several format and

typing checks, just run `make check` before committing to make sure your code is ok.

Remember to test your code! You'll find a tests folder with helpers, and you can run

tests using `make test` command.

Don't know what to contribute? Here is the public 

[Project Board](https://github.com/users/imartinez/projects/3) with several ideas. 

Head over to Discord 

#contributors channel and ask for write permissions on that GitHub project.

## 💬 Community

Join the conversation around PrivateGPT on our:

- [Twitter (aka X)](https://twitter.com/PrivateGPT_AI)
- [Discord](https://discord.gg/bK6mRVpErU)

## 📖 Citation

If you use PrivateGPT in a paper, check out the [Citation file](CITATION.cff) for the correct citation.  
You can also use the "Cite this repository" button in this repo to get the citation in different formats.

Here are a couple of examples:

### BibTeX
```bibtex
@software{Martinez_Toro_PrivateGPT_2023,
author = {Martínez Toro, Iván and Gallego Vico, Daniel and Orgaz, Pablo},
license = {Apache-2.0},
month = may,
title = {{PrivateGPT}},
url = {https://github.com/imartinez/privateGPT},
year = {2023}
}
```

### APA
```
Martínez Toro, I., Gallego Vico, D., & Orgaz, P. (2023). PrivateGPT [Computer software]. https://github.com/imartinez/privateGPT
```

## 🤗 Partners & Supporters

PrivateGPT is actively supported by the teams behind:

* [Qdrant](https://qdrant.tech/), providing the default vector database

* [Fern](https://buildwithfern.com/), providing Documentation and SDKs

* [LlamaIndex](https://www.llamaindex.ai/), providing the base RAG framework and abstractions

This project has been strongly influenced and supported by other amazing projects like 

[LangChain](https://github.com/hwchase17/langchain),

[GPT4All](https://github.com/nomic-ai/gpt4all),

[LlamaCpp](https://github.com/ggerganov/llama.cpp),

[Chroma](https://www.trychroma.com/)

and [SentenceTransformers](https://www.sbert.net/).

### Creating an Advanced Private GPT: Leveraging RAG Concepts and LangChain for Enhanced Multi-Document Reading and Chatbot Interaction

===================================================================================================================================

How Does It Work?
=================

At a basic level, how does a document chatbot work? At its core, it’s just the same as ChatGPT. On ChatGPT, you can copy a bunch of text into the prompt, and then ask ChatGPT to summaries the text for you or generate some answers based on the text.

![](https://blogs.sap.com/wp-content/uploads/2023/12/Summanry.png)

Interacting with a single document, such as a PDF, Microsoft Word, or text file, works similarly. We extract all of the text from the document, pass it into an LLM prompt(here Chatgpt), such as ChatGPT, and then ask questions about the text. This is the same way the ChatGPT example above works.

Before delving into the following proof of concept, which employs the RAG concept, those interested in learning about training methods for GPT can refer to the blog mentioned below https://blogs.sap.com/2023/12/01/transforming-enterprise-ai-mastering-chatgpt-or-any-llm-training-for-business-brilliance/

RAG –

https://blogs.sap.com/2023/12/01/exploring-the-capabilities-of-retrieval-augmented-generation-in-enterprise-ai/

Interacting with multiple documents
-----------------------------------

Where it gets a little more interesting is when the document is very large, or there are many documents we want to interact with. Passing in all of the information from these documents into a request to an LLM (Large Language Model) is impossible since these requests usually have size (token) limits, and so would only succeed if we tried to pass in too much information.

We can only send the relevant information to the LLM(Chatgpt) prompt to overcome. But how do we get only the relevant information from our documents? This is where embeddings and vector stores come in.

Embeddings and Vector Stores
============================

We want a way to send only relevant bits of information from our documents to the LLM prompt. Embeddings and vector stores can help us with this.

Embeddings are probably a little confusing if you have not heard of them before, so don’t worry if they seem a little foreign at first. A bit of explanation, and using them as part of our setup, should help make their use a little more clear.

Below is the article on Vector DB –

https://blogs.sap.com/2023/12/01/vector-databases-and-embeddings-revolutionizing-ai-in-rag-in-llm-or-gpt/

An embedding allows us to organize and categories a text based on its semantic meaning. So we split our documents into lots of little text chunks and use embeddings to characterise each bit of text by its semantic meaning. An embedding transformer is used to convert a bit of text into an embedding.

![](https://blogs.sap.com/wp-content/uploads/2023/12/Whole-1.png)

An embedding categorises a piece of text by giving it a vector (coordinate) representation. That means that vectors (coordinates) that are close to each other represent pieces of information that have a similar meaning to each other. The embedding vectors are stored inside a vector store, along with the chunks of text corresponding to each embedding.

Once we have a prompt, we can use the embeddings transformer to match it with the bits of text that are most semantically relevance to it, so we know how a way to match our prompt with other related bits of text from the vector store. In our case, we use the OpenAI embeddings transformer, which employs the cosine similarity method to calculate the similarity between documents and a question.

Now that we have a smaller subset of the information which is relevant to our prompt, we can query the LLM with our initial prompt, while passing in only the relevant information as the context to our prompt.

This is what allows us to overcome the size limitation on LLM prompts. We use embeddings and a vector store to pass in only the relevant information related to our query and let it get back to us based on that.

So, how do we do this in LangChain? Fortunately, LangChain provides this functionality out of the box, and with a few short method calls, we are good to go. Let’s get started!

Interacting with a single pdf
-----------------------------

Let’s start with processing a single pdf, and we will move on to processing multiple documents later on.

The first step is to create a `Document` from the pdf. A `Document` is the base class in LangChain, which chains use to interact with information. If we look at the class definition of a `Document`, it is a very simple class, just with a `page_content` method, that allows us to access the text content of the `Document`.

![](https://blogs.sap.com/wp-content/uploads/2023/12/PDF.png)

```
We use the DocumentLoaders that LangChain provides to convert a content source into a list of

 Documents, with one Document per page.
```

For example, there are DocumentLoaders that can be used to convert pdfs, word docs, text files, CSVs, Reddit, Twitter, Discord sources, and much more, into a list of `Document's` which the LangChain chains are then able to work. Those are some cool sources, so lots to play around with once you have these basics set up.

First, let’s create a directory for our project. You can create all this as we go along or clone the GitHub repository with this link https://github.com/AbhijeetKankani/CHATGPT/

Even instead of Laingchain one can use LLMA Index as well, difference is mentioned in below blog –

https://blogs.sap.com/2023/12/01/langchain-vs-llamaindex-enhancing-llm-applications-on-sap-btp/

For the setup, we’ll need to install various packages using the command provided below. While both Business Application Studio (BAS) and Visual Studio Code (VS Code) can be used, I recommend opting for VS Code. My experience with this proof of concept (POC) in BAS indicated that several dependencies were missing, which VS Code can better handle.

pip install flask==2.0.1 openai==0.27.0  langchain==0.0.220  unstructured==0.7.11 chromadb==0.3.26 tiktoken==0.4.0

All these libraries are there in requirements.txt file.

Solution Architecture
---------------------

There are three main steps in the workflow of RAG(:

1) Users input requests through the Chatbot web interface built in FlaskUI or any other of your choice like Streamlit, React.

2) LangChain is used as an agent framework to orchestrate the different components; Once a request comes in, LangChain sends a search query to OpenAI(Chatgpt) or we can even use other LLM like LLMA2 as well to retrieve the context that is relevant to the user request.

3) LangChain then sends a prompt that includes the user request and the relevant context to a LLM. The results from the LLM are sent back to users via the Chatbot web interface.

So by looking at Project structure one can identify that data folder is used to keep all the pdf, words or text documents, kind of Knowledge bank from where we want answers.

![](https://blogs.sap.com/wp-content/uploads/2023/12/Project.jpg)

Now that our project folders are set up, let’s convert our PDF into a document. We will use the `PyPDFLoader` class. Also, let’s set up our OpenAI API key now. We will need it later.

```
![](https://blogs.sap.com/wp-content/uploads/2023/12/Loader.png)
```

Embeddings to the rescue!
-------------------------

As explained earlier, we can use embeddings and vector stores to send only relevant information to our prompt. The steps we will need to follow are:

* Split all the documents into small chunks of text

* Pass each chunk of text into an embedding transformer to turn it into an embedding

* Store the embeddings and related pieces of text in a vector store

Here I am not storing embedding in any vector database as its a small POC, but we can store in any vector database whenever new PDF or Word documents come. Its a one-time activity.

![](https://blogs.sap.com/wp-content/uploads/2023/12/Vct.png)

For Langchain please refer –

https://blogs.sap.com/2023/12/01/harnessing-langchain-for-rag-enhanced-private-gpt-development-on-sap-btp/

When a user submits a query, Langchain first examines the embeddings to determine relevance. The pertinent section is then forwarded to ChatGPT for in-depth processing, from which GPT produces accurate responses.

 

![](https://blogs.sap.com/wp-content/uploads/2023/12/RAG-Rag.png)  

How to use API key cab be referred by below blog series –  

https://blogs.sap.com/2023/08/31/hello-world-openai-crafting-accurate-chatgpt-like-custom-search-for-sapui5-applications/

![](https://blogs.sap.com/wp-content/uploads/2023/12/Open.png)

In the project showcased on GitHub, I’ve demonstrated a proof of concept (POC) where embeddings are generated at runtime. However, for a full-scale, real-time project, it’s advisable to manage embeddings separately and store them in a vector database. This ensures efficiency and scalability. Additionally, all documents should be stored in a reliable storage solution. For instance, if using SAP’s ecosystem, SAP Document Management (DM) is suitable. Alternatively, in an Amazon hyper-scaler environment, Amazon S3 can be utilized for storage, or other comparable storage solutions can be employed based on the project’s needs.

If you’re aiming to evolve this proof of concept into a fully-fledged product, it’s advisable to consult SAP’s guidelines on utilizing the SAP Generative AI Hub, SAP AI Launchpad, and SAP AI Core.

https://blogs.sap.com/2023/11/30/revolutionizing-business-solutions-with-sap-btp-a-new-era-of-llm-agnosticism/

**Conclusion**

This project serves as a foundational POC, illustrating the integration of embeddings and vector storage in document management and query resolution using AI. While suitable for a POC, a full-scale deployment would require more robust storage and management solutions to handle the complexities and demands of real-world applications effectively.I invite the everyone to share their comments and feedback on this approach, as your insights and perspectives are invaluable for refining and enhancing future developments

### Assigned Tags

/![](https://blogs.sap.com/wp-content/uploads/2023/12/Summanry.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/Whole-1.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/PDF.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/Project.jpg)![](https://blogs.sap.com/wp-content/uploads/2023/12/Loader.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/Vct.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/RAG-Rag.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/Open.png)
