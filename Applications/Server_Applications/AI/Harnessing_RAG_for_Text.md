---
title: "Harnessing RAG for Text, Tables, and Images: A Comprehensive Guide"
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Harnessing_RAG_for_Text
Harnessing RAG for Text, Tables, and Images: A Comprehensive Guide
==================================================================

![]()In the realm of information retrieval, Retrieval Augmented Generation (RAG) has emerged as a powerful tool for extracting knowledge from vast amounts of text data. This versatile technique leverages a combination of retrieval and generation strategies to effectively summarize and synthesize information from relevant documents. However, while RAG has gained considerable traction, its application to a broader range of content types, encompassing text, tables, and images, remains relatively unexplored.

**The Challenge of Multimodal Content**

Most real-world documents contain a rich tapestry of information, often combining text, tables, and images to convey complex ideas and insights. While traditional RAG models excel at processing text, they struggle to effectively integrate and comprehend multimodal content. This limitation hinders the ability of RAG to fully capture the essence of these documents, potentially leading to incomplete or inaccurate representations.

In this post we will explore how to create a Multi-modal RAG that can handle these types of documents.  
Here is a graph that we will use as a guide to process such documents.

![]()Multi-Modal RAG**Step 1 : Split the file to raw elements.**

First, let’s import all necessary libraries to our environment


```
import os  
import openai  
import io  
import uuid  
import base64  
import time   
from base64 import b64decode  
import numpy as np  
from PIL import Image  
  
from unstructured.partition.pdf import partition\_pdf  
  
from langchain.chat\_models import ChatOpenAI  
from langchain.schema.messages import HumanMessage, SystemMessage  
from langchain.vectorstores import Chroma  
from langchain.storage import InMemoryStore  
from langchain.schema.document import Document  
from langchain.embeddings import OpenAIEmbeddings  
from langchain.retrievers.multi\_vector import MultiVectorRetriever  
from langchain.chat\_models import ChatOpenAI  
from langchain.prompts import ChatPromptTemplate  
from langchain.schema.output\_parser import StrOutputParser  
from langchain.schema.runnable import RunnablePassthrough, RunnableLambda  
  
from operator import itemgetter
```
We will use Unstructured to parse images, texts, and tables from documents (PDFs), you can run this code directly in this google colab, or download the pdf file from here ,upload it to session storage. and follow steps below.


> ( Refer to the installation instructions in google colab to setup your venv before executing the code )
> 
> 


```
# load the pdf file to drive  
# split the file to text, table and images  
def doc\_partition(path,file\_name):  
 raw\_pdf\_elements = partition\_pdf(  
 filename=path + file\_name,  
 extract\_images\_in\_pdf=True,  
 infer\_table\_structure=True,  
 chunking\_strategy="by\_title",  
 max\_characters=4000,  
 new\_after\_n\_chars=3800,  
 combine\_text\_under\_n\_chars=2000,  
 image\_output\_dir\_path=path)  
  
 return raw\_pdf\_elements  
path = "/content/"  
file\_name = "wildfire\_stats.pdf"  
raw\_pdf\_elements = doc\_partition(path,file\_name)
```
Once you run the code above, all images included in the file will be extracted and saved in your path. in our case (path = “/content/”)

Next we will append each raw element to its category, (text to texts ,table to tables, for images, *unstructed* has taken care of that already..).


```
# appending texts and tables from the pdf file  
def data\_category(raw\_pdf\_elements): # we may use decorator here  
 tables = []  
 texts = []  
 for element in raw\_pdf\_elements:  
 if "unstructured.documents.elements.Table" in str(type(element)):  
 tables.append(str(element))  
 elif "unstructured.documents.elements.CompositeElement" in str(type(element)):  
 texts.append(str(element))  
 data\_category = [texts,tables]  
 return data\_category  
texts = data\_category(raw\_pdf\_elements)[0]  
tables = data\_category(raw\_pdf\_elements)[1]
```
**Step 2 : Image captioning and table summarizing**

For summarizing tables, we will use Langchain and GPT-4. For generating image captions, we will use GPT-4-Vision-Preview. This is because it is the only model that can currently handle multiple images together, which is important for our documents that contain multiple images. For text elements, we will leave them as they are before making them into embeddings.

Get your OpenAI API key ready


```
os.environ["OPENAI\_API\_KEY"] = 'sk-xxxxxxxxxxxxxxx'  
openai.api\_key = os.environ["OPENAI\_API\_KEY"]
```

```
# function to take tables as input and then summarize them  
def tables\_summarize(data\_category):  
 prompt\_text = """You are an assistant tasked with summarizing tables. \  
 Give a concise summary of the table. Table chunk: {element} """  
  
 prompt = ChatPromptTemplate.from\_template(prompt\_text)  
 model = ChatOpenAI(temperature=0, model="gpt-4")  
 summarize\_chain = {"element": lambda x: x} | prompt | model | StrOutputParser()  
 table\_summaries = summarize\_chain.batch(tables, {"max\_concurrency": 5})  
   
  
 return table\_summaries  
table\_summaries = tables\_summarize(data\_category)  
text\_summaries = texts
```
For images we need to encode them to base64 format before feeding them to our model for captioning


```
def encode\_image(image\_path):  
 ''' Getting the base64 string '''  
 with open(image\_path, "rb") as image\_file:  
 return base64.b64encode(image\_file.read()).decode('utf-8')  
  
def image\_captioning(img\_base64,prompt):  
 ''' Image summary '''  
 chat = ChatOpenAI(model="gpt-4-vision-preview",  
 max\_tokens=1024)  
  
 msg = chat.invoke(  
 [  
 HumanMessage(  
 content=[  
 {"type": "text", "text":prompt},  
 {  
 "type": "image\_url",  
 "image\_url": {  
 "url": f"data:image/jpeg;base64,{img\_base64}"  
 },  
 },  
 ]  
 )  
 ]  
 )  
 return msg.content
```
Now we can append our list of images\_base64 and summarize them, then we split base64-encoded images and their associated texts.

while running the code below you may get this `RateLimitError` with error code `429`. This error occurs when you’ve exceeded the rate limit for requests per minute (RPM) for the `gpt-4-vision-preview` in your organization, in my case, I have a usage limit of 3 RPM, so after each image captioning, I set 60s as a safety measure and wait for the rate limit to reset.


```
# Store base64 encoded images  
img\_base64\_list = []  
  
# Store image summaries  
image\_summaries = []  
  
# Prompt : Our prompt here is customized to the type of images we have which is chart in our case  
prompt = "Describe the image in detail. Be specific about graphs, such as bar plots."  
  
# Read images, encode to base64 strings  
for img\_file in sorted(os.listdir(path)):  
 if img\_file.endswith('.jpg'):  
 img\_path = os.path.join(path, img\_file)  
 base64\_image = encode\_image(img\_path)  
 img\_base64\_list.append(base64\_image)  
 img\_capt = image\_captioning(base64\_image,prompt)  
 time.sleep(60)  
 image\_summaries.append(image\_captioning(img\_capt,prompt))
```

```
def split\_image\_text\_types(docs):  
 ''' Split base64-encoded images and texts '''  
 b64 = []  
 text = []  
 for doc in docs:  
 try:  
 b64decode(doc)  
 b64.append(doc)  
 except Exception as e:  
 text.append(doc)  
 return {  
 "images": b64,  
 "texts": text  
 }
```
**Step 3** : **Create a Multi-vector retriever and store texts, tables, images and their indexes in a Vector Base**

We have completed the first part, which include document partitioning to raw elements, summarizing tables and images, now we are ready for the second part, where we will create a multi-vector retriever and store our outputs from part one in chromadb along with their ids.


> 1. we create a vectorestore to index the child chunks (summary\_texts ,summary\_tables, summary\_img), and use OpenAIEmbeddings() for embeddings,
> 
> 2 .A docstore for the parent documents to store (doc\_ids, texts), (table\_ids, tables) and (img\_ids, img\_base64\_list)
> 
> 


```
# The vectorstore to use to index the child chunks  
vectorstore = Chroma(collection\_name="multi\_modal\_rag",  
 embedding\_function=OpenAIEmbeddings())  
  
# The storage layer for the parent documents  
store = InMemoryStore()  
id\_key = "doc\_id"  
  
# The retriever (empty to start)  
retriever = MultiVectorRetriever(  
 vectorstore=vectorstore,  
 docstore=store,  
 id\_key=id\_key,  
)  
  
# Add texts  
doc\_ids = [str(uuid.uuid4()) for \_ in texts]  
summary\_texts = [  
 Document(page\_content=s, metadata={id\_key: doc\_ids[i]})  
 for i, s in enumerate(text\_summaries)  
]  
retriever.vectorstore.add\_documents(summary\_texts)  
retriever.docstore.mset(list(zip(doc\_ids, texts)))  
  
# Add tables  
table\_ids = [str(uuid.uuid4()) for \_ in tables]  
summary\_tables = [  
 Document(page\_content=s, metadata={id\_key: table\_ids[i]})  
 for i, s in enumerate(table\_summaries)  
]  
retriever.vectorstore.add\_documents(summary\_tables)  
retriever.docstore.mset(list(zip(table\_ids, tables)))  
  
# Add image summaries  
img\_ids = [str(uuid.uuid4()) for \_ in img\_base64\_list]  
summary\_img = [  
 Document(page\_content=s, metadata={id\_key: img\_ids[i]})  
 for i, s in enumerate(image\_summaries)  
]  
retriever.vectorstore.add\_documents(summary\_img)  
retriever.docstore.mset(list(zip(img\_ids, img\_base64\_list)))
```
**Step 4 : Wrap all the above using langchain RunnableLambda**

* We first compute the context (both “texts” and “images” in this case) and the question (just a RunnablePassthrough here)
* Then we pass this into our prompt template, which is a custom function that formats the message for the gpt-4-vision-preview model.
* And finally we parse the output as a string.


```
from operator import itemgetter  
from langchain.schema.runnable import RunnablePassthrough, RunnableLambda  
  
def prompt\_func(dict):  
 format\_texts = "\n".join(dict["context"]["texts"])  
 return [  
 HumanMessage(  
 content=[  
 {"type": "text", "text": f"""Answer the question based only on the following context, which can include text, tables, and the below image:  
Question: {dict["question"]}  
  
Text and tables:  
{format\_texts}  
"""},  
 {"type": "image\_url", "image\_url": {"url": f"data:image/jpeg;base64,{dict['context']['images'][0]}"}},  
 ]  
 )  
 ]  
  
model = ChatOpenAI(temperature=0, model="gpt-4-vision-preview", max\_tokens=1024)  
  
# RAG pipeline  
chain = (  
 {"context": retriever | RunnableLambda(split\_image\_text\_types), "question": RunnablePassthrough()}  
 | RunnableLambda(prompt\_func)  
 | model  
 | StrOutputParser()  
 )
```
Now, we are ready to test our Multi-retrieval Rag


```
chain.invoke(  
 "What is the change in wild fires from 1993 to 2022?"  
)
```
here is the answer :


> Based on the provided chart, the number of wildfires has increased from 1993 to 2022. The chart shows a line graph with the number of fires in thousands, which appears to start at a lower point in 1993 and ends at a higher point in 2022. The exact numbers for 1993 are not provided in the text or visible on the chart, but the visual trend indicates an increase.
> 
> Similarly, the acres burned, represented by the shaded area in the chart, also show an increase from 1993 to 2022. The starting point of the shaded area in 1993 is lower than the ending point in 2022, suggesting that more acres have been burned in 2022 compared to 1993. Again, the specific figures for 1993 are not provided, but the visual trend on the chart indicates an increase in the acres burned over this time period.to do
> 
> 

**References** :

https://python.langchain.com/docs/modules/data\_connection/retrievers/multi\_vector

https://blog.langchain.dev/semi-structured-multi-modal-rag/
