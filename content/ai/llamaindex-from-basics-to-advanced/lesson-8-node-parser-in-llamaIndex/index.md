---
categories: [llamaindex]
date: 2024-08-25T18:54:11+0700
description: Trình phân tích cú pháp Nút trong LlamaIndex là các mô-đun được thiết kế để phân chia tài liệu thành các đơn vị nhỏ hơn, dễ quản lý gọi là nút. Những nút này rất quan trọng cho việc tổ chức, truy vấn và phân tích dữ liệu văn bản một cách hiệu quả. LlamaIndex cung cấp nhiều loại Trình phân tích cú pháp Nút khác nhau, bao gồm các trình phân tích cú pháp dựa trên tệp cho các định dạng cụ thể như JSON và Markdown, các trình phân tách văn bản để phân đoạn nội dung theo câu hoặc ý nghĩa ngữ nghĩa, và các trình phân tích cú pháp dựa trên quan hệ để duy trì cấu trúc phân cấp. Những công cụ này cho phép xử lý nội dung văn bản đa dạng một cách tùy chỉnh và hiệu quả.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-8-node-parser-in-llamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-8-node-parser-in-llamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, SimpleDirectoryReader]
title: Lesson 8 - Node Parser trong llamaIndex
weight: 8
---

### Mô-đun Phân tích Cú pháp Nút (Node Parser Modules) trong LlamaIndex

LlamaIndex cung cấp một loạt các mô-đun phân tích cú pháp nút giúp chuyển đổi tài liệu (Document) thành các phần nhỏ hơn, gọi là nút (Node). Các mô-đun này hỗ trợ xử lý nhiều loại nội dung khác nhau và cho phép tùy chỉnh cách thức dữ liệu được phân đoạn và phân tích. Dưới đây là chi tiết về các mô-đun phân tích cú pháp nút phổ biến và cách sử dụng chúng.

#### 1. Trình phân tích cú pháp dựa trên tệp (File-Based Node Parsers)

Các mô-đun này tạo ra các nút từ các loại nội dung cụ thể như JSON, Markdown, hoặc HTML. Chúng rất hữu ích khi bạn làm việc với các tệp có định dạng cụ thể và muốn phân tích cú pháp nội dung trực tiếp từ tệp.

-   **SimpleFileNodeParser**: Tự động chọn trình phân tích cú pháp nút tốt nhất cho loại tệp bạn đưa vào.

    ```python
    from llama_index.core.node_parser import SimpleFileNodeParser
    from llama_index.readers.file import FlatReader
    from pathlib import Path

    # Tải tài liệu Markdown
    md_docs = FlatReader().load_data(Path("./test.md"))
    # Sử dụng SimpleFileNodeParser để phân tích cú pháp
    parser = SimpleFileNodeParser()
    md_nodes = parser.get_nodes_from_documents(md_docs)
    ```

-   **HTMLNodeParser**: Sử dụng thư viện `beautifulsoup` để phân tích cú pháp các thẻ HTML được chỉ định. Bạn có thể tùy chỉnh danh sách thẻ cần phân tích.

    ```python
    from llama_index.core.node_parser import HTMLNodeParser

    # Chỉ phân tích cú pháp các thẻ p và h1
    parser = HTMLNodeParser(tags=["p", "h1"])
    nodes = parser.get_nodes_from_documents(html_docs)
    ```

-   **JSONNodeParser**: Được sử dụng để phân tích cú pháp các tệp JSON, tạo ra các nút tương ứng với cấu trúc JSON.

    ```python
    from llama_index.core.node_parser import JSONNodeParser

    # Phân tích cú pháp các tài liệu JSON
    parser = JSONNodeParser()
    nodes = parser.get_nodes_from_documents(json_docs)
    ```

-   **MarkdownNodeParser**: Đặc biệt dành cho phân tích cú pháp các tệp Markdown, giúp bạn chia nhỏ tài liệu Markdown thành các nút dễ quản lý.

    ```python
    from llama_index.core.node_parser import MarkdownNodeParser

    # Phân tích cú pháp tài liệu Markdown
    parser = MarkdownNodeParser()
    nodes = parser.get_nodes_from_documents(markdown_docs)
    ```

#### 2. Trình phân tách văn bản (Text Splitters)

Các mô-đun này chia nhỏ văn bản thành các nút dựa trên các tiêu chí khác nhau như độ dài, câu, hoặc mã nguồn.

-   **CodeSplitter**: Chia mã nguồn thành các đoạn nhỏ dựa trên số dòng hoặc ký tự, tùy theo ngôn ngữ lập trình.

    ```python
    from llama_index.core.node_parser import CodeSplitter

    # Chia mã Python thành các đoạn 40 dòng với độ chồng lấp 15 dòng
    splitter = CodeSplitter(
        language="python",
        chunk_lines=40,
        chunk_lines_overlap=15,
        max_chars=1500,
    )
    nodes = splitter.get_nodes_from_documents(documents)
    ```

-   **SentenceSplitter**: Chia văn bản thành các đoạn nhỏ dựa trên câu, giúp giữ lại tính liên tục ngữ nghĩa.

    ```python
    from llama_index.core.node_parser import SentenceSplitter

    # Chia văn bản thành các đoạn 1024 ký tự với độ chồng lấp 20 ký tự
    splitter = SentenceSplitter(
        chunk_size=1024,
        chunk_overlap=20,
    )
    nodes = splitter.get_nodes_from_documents(documents)
    ```

-   **SemanticSplitterNodeParser**: Chia văn bản dựa trên độ tương đồng ngữ nghĩa giữa các câu, giúp tạo ra các đoạn văn bản có liên quan về mặt ngữ nghĩa.

    ```python
    from llama_index.core.node_parser import SemanticSplitterNodeParser
    from llama_index.embeddings.openai import OpenAIEmbedding

    embed_model = OpenAIEmbedding()
    splitter = SemanticSplitterNodeParser(
        buffer_size=1, breakpoint_percentile_threshold=95, embed_model=embed_model
    )
    ```

#### 3. Trình phân tích cú pháp nút dựa trên quan hệ (Relation-Based Node Parsers)

-   **HierarchicalNodeParser**: Phân chia văn bản thành các nút phân cấp, tạo ra một hệ thống các nút cha-con. Điều này hữu ích khi bạn cần giữ lại cấu trúc phân cấp của tài liệu để xử lý hoặc truy vấn sau này.

    ```python
    from llama_index.core.node_parser import HierarchicalNodeParser

    # Tạo hệ thống nút phân cấp với kích thước các đoạn lần lượt là 2048, 512, và 128 ký tự
    node_parser = HierarchicalNodeParser.from_defaults(
        chunk_sizes=[2048, 512, 128]
    )
    ```
