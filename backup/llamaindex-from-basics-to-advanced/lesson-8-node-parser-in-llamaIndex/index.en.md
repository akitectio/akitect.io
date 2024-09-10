---
categories: [llamaindex]
date: 2024-08-25T18:54:11+0700
description: Node Parsers in LlamaIndex are modules designed to break down documents into smaller, manageable units called nodes. These nodes are essential for organizing, querying, and analyzing text data effectively. LlamaIndex offers various types of Node Parsers, including file-based parsers for handling specific formats like JSON and Markdown, text splitters for segmenting content by sentences or semantic meaning, and relation-based parsers for maintaining hierarchical structures. These tools enable customized and efficient processing of diverse textual content.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-8-node-parser-in-llamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-8-node-parser-in-llamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, SimpleDirectoryReader]
title: Lesson 8 - Node Parser in llamaIndex
weight: 8
---

### Node Parser Modules in LlamaIndex

LlamaIndex offers a range of Node Parser modules that help convert a document into smaller sections, called nodes. These modules support handling various content types and allow customization in how data is segmented and parsed. Below is a detailed guide on common Node Parser modules and how to use them.

#### 1. File-Based Node Parsers

These modules generate nodes based on specific content types like JSON, Markdown, or HTML. They are useful when working with files of specific formats and want to parse the content directly from the file.

-   **SimpleFileNodeParser**: Automatically selects the best node parser for the file type provided.

    ```python
    from llama_index.core.node_parser import SimpleFileNodeParser
    from llama_index.readers.file import FlatReader
    from pathlib import Path

    # Load a Markdown document
    md_docs = FlatReader().load_data(Path("./test.md"))
    # Use SimpleFileNodeParser to parse the document
    parser = SimpleFileNodeParser()
    md_nodes = parser.get_nodes_from_documents(md_docs)
    ```

-   **HTMLNodeParser**: Uses the `beautifulsoup` library to parse specified HTML tags. You can customize the list of tags to parse.

    ```python
    from llama_index.core.node_parser import HTMLNodeParser

    # Parse only p and h1 tags
    parser = HTMLNodeParser(tags=["p", "h1"])
    nodes = parser.get_nodes_from_documents(html_docs)
    ```

-   **JSONNodeParser**: Used to parse JSON files, creating nodes corresponding to the JSON structure.

    ```python
    from llama_index.core.node_parser import JSONNodeParser

    # Parse JSON documents
    parser = JSONNodeParser()
    nodes = parser.get_nodes_from_documents(json_docs)
    ```

-   **MarkdownNodeParser**: Specifically designed for parsing Markdown files, helping you break down Markdown documents into manageable nodes.

    ```python
    from llama_index.core.node_parser import MarkdownNodeParser

    # Parse a Markdown document
    parser = MarkdownNodeParser()
    nodes = parser.get_nodes_from_documents(markdown_docs)
    ```

#### 2. Text Splitters

These modules break down text into nodes based on different criteria such as length, sentences, or source code.

-   **CodeSplitter**: Splits source code into smaller chunks based on the number of lines or characters, depending on the programming language.

    ```python
    from llama_index.core.node_parser import CodeSplitter

    # Split Python code into 40-line chunks with 15 lines of overlap
    splitter = CodeSplitter(
        language="python",
        chunk_lines=40,
        chunk_lines_overlap=15,
        max_chars=1500,
    )
    nodes = splitter.get_nodes_from_documents(documents)
    ```

-   **SentenceSplitter**: Splits text into smaller chunks based on sentences, helping maintain semantic continuity.

    ```python
    from llama_index.core.node_parser import SentenceSplitter

    # Split text into 1024-character chunks with 20-character overlap
    splitter = SentenceSplitter(
        chunk_size=1024,
        chunk_overlap=20,
    )
    nodes = splitter.get_nodes_from_documents(documents)
    ```

-   **SemanticSplitterNodeParser**: Splits text based on semantic similarity between sentences, creating text chunks that are semantically related.

    ```python
    from llama_index.core.node_parser import SemanticSplitterNodeParser
    from llama_index.embeddings.openai import OpenAIEmbedding

    embed_model = OpenAIEmbedding()
    splitter = SemanticSplitterNodeParser(
        buffer_size=1, breakpoint_percentile_threshold=95, embed_model=embed_model
    )
    ```

#### 3. Relation-Based Node Parsers

-   **HierarchicalNodeParser**: Divides text into hierarchical nodes, creating a parent-child system of nodes. This is useful when you need to maintain the hierarchical structure of a document for processing or querying later.

    ```python
    from llama_index.core.node_parser import HierarchicalNodeParser

    # Create a hierarchical node system with chunk sizes of 2048, 512, and 128 characters
    node_parser = HierarchicalNodeParser.from_defaults(
        chunk_sizes=[2048, 512, 128]
    )
    ```

These Node Parser modules in LlamaIndex provide powerful tools to handle and manage text data according to your specific needs, enabling efficient text processing and querying.
