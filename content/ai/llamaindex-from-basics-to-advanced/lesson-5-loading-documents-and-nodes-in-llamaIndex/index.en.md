---
categories: [llamaindex]
date: 2024-08-24T23:52:11+0700
description: Instructions on the important steps to enter and organize data in LlamaIndex. This lesson likes to load documents and divide them into nodes, which are database units in the system. You will learn how to manage and configure these nodes effectively, making querying and retrieving data in your LLM application faster and more accurate.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-5-loading-documents-and-nodes-in-llamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-5-loading-documents-and-nodes-in-llamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, embeddings]
title: Lesson 5 - Loading Documents & Nodes in LlamaIndex
weight: 5
---

In an increasingly complex world of information, effective search and retrieval are no longer solely reliant on exact keyword matches. LlamaIndex, with the support of Embeddings, has transcended this limitation, offering intelligent search capabilities based on meaning and context. Embeddings act as a bridge between natural language and mathematical representation, allowing LlamaIndex to deeply understand the content of documents and user queries, thus providing more accurate and relevant search results than ever before.

## Concept

The `Document` and `Node` objects are fundamental abstract concepts in LlamaIndex.

-   **Document**: A `Document` is a general container for any data source---e.g., a PDF file, API output, or data retrieved from a database. They can be manually constructed or automatically generated through our data loaders. By default, a `Document` stores text along with several other attributes. Some of these include:

    -   `metadata`: a dictionary of annotations that can be appended to the text.
    -   `relationships`: a dictionary containing relationships with other `Document`/`Node` objects.

    **Note**: We have beta support for allowing `Document` to store images and are actively working to improve its multimodal capabilities.

-   **Node**: A `Node` represents a "piece" of the source `Document`, whether it's a text segment, an image, or something else. Similar to `Document`, they contain metadata and relationship information with other nodes.
    `Node` is a first-class citizen in LlamaIndex. You can choose to define a `Node` and all its attributes directly. You can also choose to "parse" the source `Document` into `Nodes` through our `NodeParser` classes. By default, any `Node` derived from a `Document` inherits the same metadata from that `Document` (e.g., a "file_name" field in a `Document` is passed to every `Node`).

## Defining and Customizing Documents

### 1. Defining Documents

Documents can be automatically generated through data loaders or manually constructed.

-   **Automatic Creation**: By default, all of our data loaders (including those provided on LlamaHub) return `Document` objects via the `load_data` function.

```python
from llama_index.core import SimpleDirectoryReader
documents = SimpleDirectoryReader("./data").load_data()
```

-   **Manual Construction**: You can also choose to manually construct documents. LlamaIndex provides the `Document` structure.

```python
from llama_index.core import Document

text_list = [text1, text2, ...]
documents = [Document(text=t) for t in text_list]
```

-   **Quick Creation**: To speed up prototyping and development, you can quickly create a document with some default text:

```python
document = Document.example()
```

### 2. Customizing Documents

This section covers various ways to customize `Document` objects. Since the `Document` object is a subclass of our `TextNode` object, all of these settings and details also apply to the `TextNode` class.

#### Metadata

Documents also provide an opportunity to include useful metadata. Using the `metadata` dictionary on each document, additional information can be included to help inform responses and track sources for query responses. This information can be anything, such as a file name or category. If you are integrating with a vector database, note that some vector databases require the keys to be strings and the values to be flat (either `str`, `float`, or `int`).

Any information placed in the `metadata` dictionary of each document will appear in the `metadata` of each source node created from the document. Additionally, this information is included in the nodes, allowing the index to use it in queries and responses. By default, metadata is included in the text for both embeddings and LLM calls.

There are a few ways to set this dictionary:

-   In the document constructor:

```python
document = Document(
    text="text",
    metadata={"filename": "<doc_file_name>", "category": "<category>"},
)
```

-   After the document is created:

```python
document.metadata = {"filename": "<doc_file_name>"}
```

-   Automatically set the file name using `SimpleDirectoryReader` and the `file_metadata` hook. This will automatically run the hook on each document to set the `metadata` field:

```python
from llama_index.core import SimpleDirectoryReader

filename_fn = lambda filename: {"file_name": filename}

# Automatically set the metadata of each document according to filename_fn
documents = SimpleDirectoryReader(
    "./data", file_metadata=filename_fn
).load_data()
```

#### Customizing IDs

As detailed in the Document Management section, the `doc_id` is used to allow for efficient refreshing of documents in the index. When using `SimpleDirectoryReader`, you can automatically set the document's `doc_id` to the full path of each document:

```python
from llama_index.core import SimpleDirectoryReader

documents = SimpleDirectoryReader("./data", filename_as_id=True).load_data()
print([x.doc_id for x in documents])
```

You can also directly set the `doc_id` of any `Document`!

```python
document.doc_id = "My new document id!"
```

**Note**: IDs can also be set via the `node_id` or `id_` attribute on a Document object, similar to a `TextNode` object.

#### Advanced - Customizing Metadata

One important detail mentioned above is that by default, any metadata you set is included during the embedding and LLM generation process.

##### Customizing LLM Metadata Text

Typically, a document may have multiple metadata keys, but you may not want all of them to be visible to the LLM during response synthesis. In the examples above, we may not want the LLM to read the `file_name` of our document. However, `file_name` might contain information that will help create better embeddings. A key benefit of doing this is to steer the embeddings for retrieval without altering what the LLM ultimately reads.

We can exclude it as follows:

```python
document.excluded_llm_metadata_keys = ["file_name"]
```

We can then check what the LLM will actually read using the `get_content()` function and specifying `MetadataMode.LLM`:

```python
from llama_index.core.schema import MetadataMode

print(document.get_content(metadata_mode=MetadataMode.LLM))
```

##### Customizing Embedding Metadata Text

Similar to customizing the metadata displayed to the LLM, we can also customize the metadata displayed to embeddings. In this case, you can specifically exclude metadata displayed to the embedding model, in case you DO NOT want specific text to skew the embeddings.

```python
document.excluded_embed_metadata_keys = ["file_name"]
```

We can then check what the embedding model will actually read using the `get_content()` function and specifying `MetadataMode.EMBED`:

```python
from llama_index.core.schema import MetadataMode

print(document.get_content(metadata_mode=MetadataMode.EMBED))
```

##### Customizing Metadata Formatting

As you know, metadata is included in the actual text of each document/node when it is sent to the LLM or embedding model. By default, the format of this metadata is controlled by three attributes:

-   `Document.metadata_separator` -> default = `"\n"`
    This controls the separator between each key/value pair when concatenating all of your metadata's key/value fields.

-   `Document.metadata_template` -> default = `"{key}: {value}"`
    This attribute controls how each key/value pair in your metadata is formatted. The `key` and `value` string variables are required.

-   `Document.text_template` -> default = `{metadata_str}\n\n{content}`
    When your metadata is converted into a string using `metadata_separator` and `metadata_template`, this template controls what that metadata looks like when it is concatenated with the text content of your document/node. The `metadata` and `content` string keys are required.

#### Summary

Knowing all this, let's create a short example that uses all this power:

```python
from llama_index.core import Document
from llama_index.core.schema import MetadataMode

document = Document(
    text="This is a super-customized document",
    metadata={
        "file_name": "super_secret_document.txt",
        "category": "finance",
        "author": "LlamaIndex",
    },
    excluded_llm_metadata_keys=["file_name"],
    metadata_separator="::",
    metadata_template="{key}=>{value}",
    text_template="Metadata: {metadata_str}\n-----\nContent: {content}",
)

print(
    "The LLM sees this: \n",
    document.get_content(metadata_mode=MetadataMode.LLM),
)
print(
    "The Embedding model sees this: \n",
    document.get_content(metadata_mode=MetadataMode.EMBED),
)
```

### 3. Advanced - Automatic Metadata Extraction

You can use LlamaIndex's **Metadata Extractor** modules to automatically extract metadata from text with the support of LLM.

The metadata extraction modules include:

-   `SummaryExtractor`: Automatically extracts summaries from a set of nodes.
-   `QuestionsAnsweredExtractor`: Extracts questions that each node can answer.
-   `TitleExtractor`: Extracts titles from the context of each node.
-   `EntityExtractor`: Extracts entities such as place names, people, or objects mentioned in the content.

### Example:

```python
from llama_index.core.extractors import TitleExtractor, QuestionsAnsweredExtractor
from llama_index.core.node_parser import TokenTextSplitter
from llama_index.core.ingestion import IngestionPipeline

# Set up text splitter and extractors
text_splitter = TokenTextSplitter(separator=" ", chunk_size=512, chunk_overlap=128)
title_extractor = TitleExtractor(nodes=5)
qa_extractor = QuestionsAnsweredExtractor(questions=3)

# Create a pipeline to process documents
pipeline = IngestionPipeline(transformations=[text_splitter, title_extractor, qa_extractor])
nodes = pipeline.run(documents=documents, in_place=True, show_progress=True)

# Or insert into an index
from llama_index.core import VectorStoreIndex
index = VectorStoreIndex.from_documents(documents, transformations=[text_splitter, title_extractor, qa_extractor])
```

-   **Text Splitter (`TokenTextSplitter`)**: Splits the document into smaller segments (nodes) for processing.
-   **Metadata Extractors**: `TitleExtractor` extracts titles and `QuestionsAnsweredExtractor` extracts questions that can be answered from the nodes.
-   **Document Processing Pipeline**: Uses `IngestionPipeline` to apply the steps of text splitting and metadata extraction.
-   **Index Insertion**: You can directly insert the documents into an index with the extracted metadata.

Using LLM for automatic metadata extraction helps generate useful information from documents, optimizing search and data analysis.

## Defining and Customizing Nodes

### 1. Defining Nodes

Nodes are small parts of the original document, which could be text segments, images, or other types of content. They not only contain content but also metadata and information about relationships with other nodes within the index structure.

Nodes are a crucial component in LlamaIndex. You can define nodes directly or "parse" the original document into nodes through our `NodeParser` classes.

**Example**:

```python
from llama_index.core.node_parser import SentenceSplitter

parser = SentenceSplitter()
nodes = parser.get_nodes_from_documents(documents)
```

In this example, the `SentenceSplitter` splits the document into nodes based on sentences. Each node can then be stored and retrieved individually.

### 2. Manually Creating Nodes

You can also create nodes manually if you want complete control over the content and structure of each node.

**Detailed Example**:

```python
from llama_index.core.schema import TextNode, NodeRelationship, RelatedNodeInfo

# Create two text nodes
node1 = TextNode(text="This is the first text chunk.", id_="node_1")
node2 = TextNode(text="This is the second text chunk.", id_="node_2")

# Establish relationships between the nodes
node1.relationships[NodeRelationship.NEXT] = RelatedNodeInfo(
    node_id=node2.node_id
)
node2.relationships[NodeRelationship.PREVIOUS] = RelatedNodeInfo(
    node_id=node1.node_id
)
nodes = [node1, node2]
```

In this example:

-   **TextNode**: Used to create a node with text content.
-   **NodeRelationship**: Specifies the type of relationship (e.g., NEXT, PREVIOUS).
-   **RelatedNodeInfo**: Stores information about the relationship, including metadata if needed.

You can also add metadata to the relationships between nodes:

```python
node2.relationships[NodeRelationship.PARENT] = RelatedNodeInfo(
    node_id=node1.node_id, metadata={"summary": "This is the parent node"}
)
```

### 3. Customizing Node IDs

Each node has a `node_id` attribute that is automatically generated if not specified. This ID can be used for:

-   Updating nodes in memory.
-   Identifying relationships between nodes.

You can also manually set the `node_id`:

```python
print(node1.node_id)  # "node_1"
node1.node_id = "custom_node_id_1"
```

## Summary

Understanding and customizing nodes in LlamaIndex allows you to control how data is organized and linked. This is especially useful in applications requiring complex data structures and efficient information retrieval.
