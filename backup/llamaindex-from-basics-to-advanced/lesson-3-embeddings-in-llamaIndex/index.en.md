---
categories: [llamaindex]
date: 2024-08-22T23:52:11+0700
description: In this lesson, you'll dive into how embeddings work in LlamaIndex. You'll learn how text is transformed into numerical representations through embeddings, allowing LLMs to process and understand the data more effectively. This lesson will cover the different types of embeddings, their use cases, and how to implement them in your applications to enhance performance and accuracy.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-3-embeddings-in-lamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-3-embeddings-in-lamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, embeddings]
title: Lesson 3 - Embeddings In LLamaIndex
weight: 3
---

Here is the English translation of the content you provided:

* * *

**Embeddings** play a crucial role in LlamaIndex, transforming text into complex numerical representations that allow large language models (LLMs) to understand and process semantics effectively. This is a critical step in optimizing applications like search and text classification. In this article, we will explore how to use embeddings in LlamaIndex, from basic setup to advanced customization, and how to integrate them into your system.

## Embeddings: Semantic Representation for Intelligent Search

**Concept**

Embeddings are key in LlamaIndex by converting your documents into sophisticated numerical representations. Embedding models take text as input and produce a list of numbers that represent the meaning of the text. These models have been trained to represent text this way, supporting many applications, especially search.

At a high level, if a user asks about "dogs," the embedding for that question will be highly similar to the embedding of text about "dogs."

When calculating similarity between embeddings, there are many methods like dot product, cosine similarity, etc. By default, LlamaIndex uses cosine similarity to compare embeddings.

There are many embedding models to choose from. By default, LlamaIndex uses `text-embedding-ada-002` from OpenAI. We also support any embedding model from Langchain and provide an easily extendable base class to implement your own embeddings.

## Using Embedding Models

### Common Usage

In LlamaIndex, the most common use of an embedding model is to specify it in the global `Settings` object and then use it in a vector index. The embedding model will be used to:

-   **Embed documents:** Convert documents into vector representations during index construction.
-   **Embed queries:** Convert user queries into vectors to search for similar documents in the index.

You can also specify an embedding model per index if needed.

### Setup

If you haven't installed the embeddings package:

```bash
pip install llama-index-embeddings-openai
```

Then, you can use the OpenAI embedding model as follows:

```python
from llama_index.embeddings.openai import OpenAIEmbedding
from llama_index.core import VectorStoreIndex
from llama_index.core import Settings

# Global setup
Settings.embed_model = OpenAIEmbedding()

# Per-index setup
index = VectorStoreIndex.from_documents(documents, embed_model=embed_model)
```

### Using Local Models

To save costs, you can use a local embedding model from Hugging Face:

```bash
pip install llama-index-embeddings-huggingface
```

```python
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
from llama_index.core import Settings

Settings.embed_model = HuggingFaceEmbedding(
    model_name="BAAI/bge-small-en-v1.5")
```

This model offers good performance and speed.

## Customizing Embeddings

### Batch Size

By default, embedding requests are sent to OpenAI in batches of 10. In rare cases, this may incur a rate limit. For users embedding many documents, this batch size may be too small. You can customize the batch size as follows:

```python
# Set batch size to 42
embed_model = OpenAIEmbedding(embed_batch_size=42)
```

### Local Embedding Models

The easiest way to use a local model is through Hugging Face:

```python
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
from llama_index.core import Settings

Settings.embed_model = HuggingFaceEmbedding(
    model_name="BAAI/bge-small-en-v1.5")
```

### HuggingFace Optimum ONNX Embeddings

LlamaIndex also supports creating and using ONNX embeddings with the Optimum library from HuggingFace. You simply create and save the ONNX embeddings and then use them.

Some prerequisites:

```bash
pip install transformers optimum[exporters]
pip install llama-index-embeddings-huggingface-optimum
```

Create and save the ONNX model:

```python
from llama_index.embeddings.huggingface_optimum import OptimumEmbedding

OptimumEmbedding.create_and_save_optimum_model(
    "BAAI/bge-small-en-v1.5", "./bge_onnx")
```

Using the ONNX model:

```python
Settings.embed_model = OptimumEmbedding(folder_name="./bge_onnx")
```

### LangChain Integration

LlamaIndex supports any embeddings offered by Langchain.

The example below loads a model from Hugging Face using Langchain's embedding class:

```bash
pip install llama-index-embeddings-langchain
```

```python
from langchain.embeddings.huggingface import HuggingFaceBgeEmbeddings
from llama_index.core import Settings

Settings.embed_model = HuggingFaceBgeEmbeddings(model_name="BAAI/bge-base-en")
```

### Custom Embedding Model

If you want to use embeddings not provided by LlamaIndex or Langchain, you can extend our base embeddings class and implement your own!

The example below uses Instructor Embeddings and implements a custom embeddings class. Instructor embeddings work by providing text along with "instructions" on the domain of the text to embed. This is helpful when embedding text from a very specific and specialized topic.

```python
from typing import Any, List
from InstructorEmbedding import INSTRUCTOR
from llama_index.core.embeddings import BaseEmbedding

class InstructorEmbeddings(BaseEmbedding):
    def __init__(
        self,
        instructor_model_name: str = "hkunlp/instructor-large",
        instruction: str = "Represent the Computer Science documentation or question:",
        **kwargs: Any,
    ) -> None:
        self._model = INSTRUCTOR(instructor_model_name)
        self._instruction = instruction
        super().__init__(**kwargs)

    def _get_query_embedding(self, query: str) -> List[float]:
        embeddings = self._model.encode([[self._instruction, query]])
        return embeddings[0]

    def _get_text_embedding(self, text: str) -> List[float]:
        embeddings = self._model.encode([[self._instruction, text]])
        return embeddings[0]

    def _get_text_embeddings(self, texts: List[str]) -> List[List[float]]:
        embeddings = self._model.encode(
            [[self._instruction, text] for text in texts]
        )
        return embeddings

    async def _get_query_embedding(self, query: str) -> List[float]:
        return self._get_query_embedding(query)

    async def _get_text_embedding(self, text: str) -> List[float]:
        return self._get_text_embedding(text)
```

### Standalone Usage

You can also use embeddings as a standalone module for your project, existing application, or general testing and exploration.

```python
embeddings = embed_model.get_text_embedding(
    "It is raining cats and dogs here!")
```

### Supported Embeddings

Here is a table listing the supported embedding models along with links to their documentation:

| **Embedding Model**          | **Documentation Link**                                                                |
| ---------------------------- | ------------------------------------------------------------------------------------- |
| **Azure OpenAI**             | [Azure OpenAI](https://azure.microsoft.com/en-us/services/cognitive-services/openai/) |
| **ClarifAI**                 | [ClarifAI](https://www.clarifai.com/)                                                 |
| **Cohere**                   | [Cohere](https://cohere.ai/)                                                          |
| **Custom**                   | [Custom Models](https://docs.llamaindex.ai/)                                          |
| **Dashscope**                | [Dashscope](https://dashscope.ai/)                                                    |
| **ElasticSearch**            | [ElasticSearch](https://www.elastic.co/elasticsearch/)                                |
| **FastEmbed**                | [FastEmbed](https://fastembed.io/)                                                    |
| **Google Palm**              | [Google Palm](https://palm.ai/)                                                       |
| **Gradient**                 | [Gradient](https://www.gradient.com/)                                                 |
| **Anyscale**                 | [Anyscale](https://www.anyscale.com/)                                                 |
| **Huggingface**              | [Huggingface](https://huggingface.co/)                                                |
| **JinaAI**                   | [JinaAI](https://www.jina.ai/)                                                        |
| **Langchain**                | [Langchain](https://langchain.com/)                                                   |
| **LLM Rails**                | [LLM Rails](https://llmrails.com/)                                                    |
| **MistralAI**                | [MistralAI](https://mistral.ai/)                                                      |
| **OpenAI**                   | [OpenAI](https://openai.com/)                                                         |
| **Sagemaker**                | [Sagemaker](https://aws.amazon.com/sagemaker/)                                        |
| **Text Embedding Inference** | [Text Embedding Inference](https://github.com/inference-api/text-embedding-inference) |
| **TogetherAI**               | [TogetherAI](https://together.ai/)                                                    |
| **Upstage**                  | [Upstage](https://upstage.ai/)                                                        |
| **VoyageAI**                 | [VoyageAI](https://voyage.ai/)                                                        |
| **Nomic**                    | [Nomic](https://www.nomic.ai/)                                                        |
| **Fireworks AI**             | [Fireworks AI](https://fireworks.ai/)                                                 |

LlamaIndex supports a wide range of embedding models from leading providers like OpenAI, Hugging Face, and many others. This diversity allows you to customize and optimize the use of large language models (LLMs) in your applications, from search to data analysis, helping you achieve high performance and meet specific project needs.
