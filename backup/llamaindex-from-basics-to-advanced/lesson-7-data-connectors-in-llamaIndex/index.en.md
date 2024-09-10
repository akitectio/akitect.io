---
categories: [llamaindex]
date: 2024-08-25T18:53:11+0700
description: This article provides an overview of "Readers" in LlamaIndex, a powerful tool for connecting and processing data from many different sources. Each Reader is designed to retrieve data from specific formats and sources such as local files, databases, remote file systems, and online platforms such as Discord, Slack, GitHub, and Twitter. Detailed descriptions of each Reader's functions help users clearly understand how to use them to optimize data management and querying.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-7-data-connectors-in-llamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-7-data-connectors-in-llamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, SimpleDirectoryReader]
title: Lesson 7 - Trình kết nối dữ liệu trong llamaIndex
weight: 7
---

### Data Connectors (LlamaHub)

Data connectors (also known as Readers) are essential tools for retrieving data from various sources and formats, transforming them into simple `Document` representations with text and basic metadata.

**Tip:** Once you've retrieved the data, you can build an `Index` on it, ask questions using the `Query Engine`, and interact through the `Chat Engine`.

### LlamaHub

Our data connectors are provided through **LlamaHub** 🦙, an open-source repository containing data loaders that you can easily plug into any LlamaIndex application.

## Usage Pattern

Each data loader has a "Usage" section that demonstrates how to use it. At the core of every loader's usage is the `download_loader` function, which downloads the loader file into a module that you can use in your application.

**Example Usage:**

```python
from llama_index.core import VectorStoreIndex, download_loader
from llama_index.readers.google import GoogleDocsReader

gdoc_ids = ["1wf-y2pd9C878Oh-FmLH7Q_BQkljdm6TQal-c1pUfrec"]
loader = GoogleDocsReader()
documents = loader.load_data(document_ids=gdoc_ids)
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine()
query_engine.query("Where did the author go to school?")
```

1.  `download_loader`: This function is used to download and install the specific data loader you want to use. In this example, we download `GoogleDocsReader` to work with Google Docs documents.
2.  `load_data`: After initializing the loader, you use the `load_data` method to retrieve data from the source. In this case, you provide a list of `document_ids` (IDs of Google Docs) to specify which documents to load.
3.  `VectorStoreIndex.from_documents`: The retrieved data is returned as a list of `Document` objects. You use this function to build a vector index from these documents, enabling semantic search.
4.  `as_query_engine`: Converts the index into a query engine, allowing you to ask questions about the content of the documents.
5.  `query`: Use the query engine to search for information in the index. In this example, we ask, "Where did the author go to school?"

## Supported Data Readers

The table below provides an overview of the main functionalities of each **Reader** in LlamaIndex.

| **Reader**                  | **Functionality**                                                                                                                 |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Simple Directory Reader** | Reads data from local files in a specified directory, supporting multiple file formats such as `.csv`, `.pdf`, `.docx`, and more. |
| **Psychic Reader**          | Integrates with the Psychic API to retrieve data from external sources.                                                           |
| **Deeplake Reader**         | Connects to Deeplake to access and process large datasets stored in the cloud.                                                    |
| **Qdrant Reader**           | Connects to Qdrant, a vector search engine, to read and query vector data.                                                        |
| **Discord Reader**          | Retrieves messages and data from Discord channels and servers.                                                                    |
| **MongoDB Reader**          | Retrieves documents and collections from MongoDB databases.                                                                       |
| **Chroma Reader**           | Connects to Chroma to load vector embeddings and perform similarity searches.                                                     |
| **MyScale Reader**          | Integrates with MyScale to query and retrieve data from its database.                                                             |
| **FAISS Reader**            | Utilizes FAISS to efficiently search and retrieve vector data.                                                                    |
| **Obsidian Reader**         | Reads notes and documents from Obsidian, integrating with your note-taking system.                                                |
| **Slack Reader**            | Retrieves messages, files, and data from Slack channels and workspaces.                                                           |
| **Webpage Reader**          | Extracts content from web pages, allowing you to index and search information on the web.                                         |
| **Pinecone Reader**         | Connects to Pinecone to read and query vector data stored in its vector database.                                                 |
| **Pathway Reader**          | Integrates with Pathway to access and process complex data streams or datasets.                                                   |
| **MBox Reader**             | Reads and processes email archives stored in MBox format.                                                                         |
| **Milvus Reader**           | Integrates with Milvus to search and retrieve data based on vector similarity.                                                    |
| **Notion Reader**           | Retrieves and reads content from Notion pages, databases, and documents.                                                          |
| **Github Reader**           | Extracts source code, issues, and other data from GitHub repositories.                                                            |
| **Google Docs Reader**      | Retrieves and processes documents stored on Google Docs.                                                                          |
| **Database Reader**         | A general-purpose reader for querying SQL databases and retrieving records.                                                       |
| **Twitter Reader**          | Connects to the Twitter API to retrieve tweets, timelines, and user data.                                                         |
| **Weaviate Reader**         | Connects to Weaviate to access and search vectorized data.                                                                        |
| **Make Reader**             | Integrates with Make (formerly Integromat) to automate workflows and retrieve data.                                               |
| **Deplot Reader**           | Retrieves and reads data from Deplot, a data management and visualization platform.                                               |
