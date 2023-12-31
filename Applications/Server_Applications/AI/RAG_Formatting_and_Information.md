---
title: Retrieval Augmented Generation (RAG) formatting and Optimization
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# RAG_Formatting_and_Information
How should you format docs for retrieval augmented generation (RAG)?

🔋 Easily readable by a human = ideal for the LLM model.

Here are 8 must-dos for teams using RAG ⤵️

1️⃣ Have a structured layout with clear headings for easy navigation.

2️⃣ Begin sections with a brief snapchat about the content.

3️⃣ Spotlight frequently searched terms like product names, IDs, FAQs, etc.

4️⃣ Maintain a neat and easy to read format for predictable information retrieval.

5️⃣ Use bullet points to organize complex ideas into digestible chunks.

6️⃣ Avoid Jargon, provide clear explanation for technical terms

7️⃣ Break dense text into shorter, readable paragraphs

8️⃣ Include a table of contents for quick navigation through large documents



Having a large knowledge base in Obsidian and a sizable collection of technical documents, for the last couple of months, I have been trying to build an RAG-based QnA system that would allow effective querying.

After the initial implementation using a standard architecture (structure unaware, format agnostic recursive text splitters and cosine similarity for semantic search), the results were a bit underwhelming. Throwing a more powerful LLM at the problem helped, but not by an order of magnitude (the model was able to reason better about the provided context, but if the context wasn't relevant to begin with, obviously it didn't matter).

Here are implementation details and tricks that helped me achieve significantly better quality. I hope it will be helpful to people implementing similar systems. Many of them I learned by reading suggestions from this and other communities, while others were discovered through experimentation.

Most of the methods described below are implemented ihere - [GitHub - snexus/llm-search: Querying local documents, powered by LLM](https://github.com/snexus/llm-search/tree/main).

## Pre-processing and chunking

Document format - the best quality is achieved with a format where the logical structure of the document can be parsed - titles, headers/subheaders, tables, etc. Examples of such formats include markdown, HTML, or .docx.

PDFs, in general, are hard to parse due to multiple ways to represent the internal structure - for example, it can be just a bunch of images stacked together. In most cases, expect to be able to split by sentences.

Content splitting:

Splitting by logical blocks (e.g., headers/subheaders) improved the quality significantly. It comes at the cost of format-dependent logic that needs to be implemented. Another downside is that it is hard to maintain an equal chunk size with this approach.

For documents containing source code, it is best to treat the code as a single logical block. If you need to split the code in the middle, make sure to embed metadata providing a hint that different pieces of code are related.

Metadata included in the text chunks:

Document name.

References to higher-level logical blocks (e.g., pointing to the parent header from a subheader in a markdown document).

For text chunks containing source code - indicating the start and end of the code block and optionally the name of the programming language.

External metadata - added as external metadata in the vector store. These fields will allow dynamic filtering by chunk size and/or label.

Chunk size.

Document path.

Document collection label, if applicable.

Chunk sizes - as many people mentioned, there appears to be high sensitivity to the chunk size. There is no universal chunk size that will achieve the best result, as it depends on the type of content, how generic/precise the question asked is, etc.

One of the solutions is embedding the documents using multiple chunk sizes and storing them in the same collection.

During runtime, querying against these chunk sizes and selecting dynamically the size that achieves the best score according to some metric.

Downside - increases the storage and processing time requirements.

## Embeddings

There are multiple embedding models achieving the same or better quality as OpenAI's ADA - for example, `e5-large-v2` - it provides a good balance between size and quality.

Some embedding models require certain prefixes to be added to the text chunks AND the query - that's the way they were trained and presumably achieve better results compared to not appending these prefixes.

## Retrieval

One of the main components that allowed me to improve retrieval is a **re-ranker**. A re-ranker allows scoring the text passages obtained from a similarity (or hybrid) search against the query and obtaining a numerical score indicating how relevant the text passage is to the query. Architecturally, it is different (and much slower) than a similarity search but is supposed to be more accurate. The results can then be sorted by the numerical score from the re-ranker before stuffing into LLM.

A re-ranker can be costly (time-consuming and/or require API calls) to implement using LLMs but is efficient using cross-encoders. It is still slower, though, than cosine similarity search and can't replace it.

Sparse embeddings - I took the general idea from [Getting Started with Hybrid Search | Pinecone](https://www.pinecone.io/learn/hybrid-search-intro/) and implemented sparse embeddings using SPLADE. This particular method has an advantage that it can minimize the "vocabulary mismatch problem." Despite having large dimensionality (32k for SPLADE), sparse embeddings can be stored and loaded efficiently from disk using Numpy's sparse matrices.

With sparse embeddings implemented, the next logical step is to use a **hybrid search** - a combination of sparse and dense embeddings to improve the quality of the search.

Instead of following the method suggested in the blog (which is a weighted combination of sparse and dense embeddings), I followed a slightly different approach:

Retrieve the **top k** documents using SPLADE (sparse embeddings).

Retrieve **top k** documents using similarity search (dense embeddings).

Create a union of documents from sparse or dense embeddings. Usually, there is some overlap between them, so the number of documents is almost always smaller than 2*k.

Re-rank all the documents (sparse + dense) using the re-ranker mentioned above.

Stuff the top documents sorted by the re-ranker score into the LLM as the most relevant documents.

The justification behind this approach is that it is hard to compare the scores from sparse and dense embeddings directly (as suggested in the blog - they rely on magical weighting constants) - but the re-ranker should explicitly be able to identify which document is more relevant to the query.

Let me know if the approach above makes sense or if you have suggestions for improvement. I would be curious to know what other tricks people used to improve the quality of their RAG systems.