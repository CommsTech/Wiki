---
title: RAG Concepts and LangChain for Enhanced Multi-Document Reading
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# RAG Concepts and LangChain for Enhanced Multi-Document Reading
Creating an Advanced Private GPT: Leveraging RAG Concepts and LangChain for Enhanced Multi-Document Reading and Chatbot Interaction
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

## Assigned Tags

Similar Blog PostsRelated Questions 

/![](https://blogs.sap.com/wp-content/uploads/2023/12/Summanry.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/Whole-1.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/PDF.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/Project.jpg)![](https://blogs.sap.com/wp-content/uploads/2023/12/Loader.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/Vct.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/RAG-Rag.png)![](https://blogs.sap.com/wp-content/uploads/2023/12/Open.png)
