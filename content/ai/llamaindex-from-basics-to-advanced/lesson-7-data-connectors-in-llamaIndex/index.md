---
categories: [llamaindex]
date: 2024-08-25T18:53:11+0700
description: Bài viết này cung cấp một cái nhìn tổng quan về các "Reader" trong LlamaIndex, một công cụ mạnh mẽ để kết nối và xử lý dữ liệu từ nhiều nguồn khác nhau. Mỗi Reader được thiết kế để truy xuất dữ liệu từ các định dạng và nguồn cụ thể như tệp cục bộ, cơ sở dữ liệu, hệ thống tệp từ xa, và các nền tảng trực tuyến như Discord, Slack, GitHub, và Twitter. Mô tả chi tiết về chức năng của từng Reader giúp người dùng hiểu rõ cách sử dụng chúng để tối ưu hóa việc quản lý và truy vấn dữ liệu.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-7-data-connectors-in-llamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-7-data-connectors-in-llamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, SimpleDirectoryReader]
title: Lesson 7 - Trình kết nối dữ liệu trong llamaIndex
weight: 7
---

## Bộ kết nối dữ liệu (LlamaHub)

Bộ kết nối dữ liệu (còn được gọi là Reader) giúp lấy dữ liệu từ nhiều nguồn và định dạng dữ liệu khác nhau, sau đó chuyển chúng thành một biểu diễn `Document` đơn giản (gồm văn bản và siêu dữ liệu cơ bản).

**Mẹo:** Sau khi đã lấy dữ liệu, bạn có thể xây dựng một `Index` trên đó, đặt câu hỏi bằng `Query Engine` và trò chuyện bằng `Chat Engine`.

### LlamaHub

Các bộ kết nối dữ liệu của chúng tôi được cung cấp thông qua **LlamaHub** 🦙. LlamaHub là một kho lưu trữ mã nguồn mở chứa các trình tải dữ liệu mà bạn có thể dễ dàng cắm và chạy trong bất kỳ ứng dụng LlamaIndex nào.

## Mô hình Sử dụng

Mỗi trình tải dữ liệu (data loader) đều chứa một phần "Cách sử dụng" hiển thị cách sử dụng trình tải đó. Cốt lõi của việc sử dụng mỗi trình tải là hàm `download_loader`, tải xuống tệp trình tải vào một mô-đun mà bạn có thể sử dụng trong ứng dụng của mình.

**Ví dụ sử dụng:**

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

1.  `download_loader`: Hàm này được sử dụng để tải xuống và cài đặt trình tải dữ liệu cụ thể mà bạn muốn sử dụng. Trong ví dụ này, chúng ta tải xuống `GoogleDocsReader` để làm việc với tài liệu Google Docs.
2.  `load_data`: Sau khi khởi tạo trình tải, bạn sử dụng phương thức `load_data` để lấy dữ liệu từ nguồn. Trong trường hợp này, bạn cung cấp danh sách `document_ids` (ID của các tài liệu Google Docs) để chỉ định tài liệu nào cần tải.
3.  `VectorStoreIndex.from_documents`: Dữ liệu được tải về dưới dạng danh sách các đối tượng `Document`. Bạn sử dụng hàm này để xây dựng một chỉ mục vectơ từ các tài liệu này, cho phép tìm kiếm dựa trên ngữ nghĩa.
4.  `as_query_engine`: Chuyển đổi chỉ mục thành một công cụ truy vấn, cho phép bạn đặt câu hỏi về nội dung của các tài liệu.
5.  `query`: Sử dụng công cụ truy vấn để tìm kiếm thông tin trong chỉ mục. Trong ví dụ này, chúng ta hỏi "Where did the author go to school?" (Tác giả đã học ở đâu?). 

## Các trình đọc dữ liệu được hổ trợ (Reader)

Bảng này cung cấp cái nhìn tổng quan về chức năng chính của từng **Reader** trong LlamaIndex.

| **Reader**                  | **Chức năng**                                                                                                                            |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **Simple Directory Reader** | Đọc dữ liệu từ các tệp cục bộ trong một thư mục được chỉ định, hỗ trợ nhiều định dạng tệp như `.csv`, `.pdf`, `.docx`, và nhiều hơn nữa. |
| **Psychic Reader**          | Tích hợp với Psychic API để truy xuất dữ liệu từ các nguồn bên ngoài.                                                                    |
| **Deeplake Reader**         | Kết nối với Deeplake để truy cập và xử lý các tập dữ liệu lớn được lưu trữ trên đám mây.                                                 |
| **Qdrant Reader**           | Kết nối với Qdrant, một công cụ tìm kiếm vector, để đọc và truy vấn dữ liệu vector.                                                      |
| **Discord Reader**          | Truy xuất tin nhắn và dữ liệu từ các kênh và máy chủ Discord.                                                                            |
| **MongoDB Reader**          | Lấy các tài liệu và bộ sưu tập từ cơ sở dữ liệu MongoDB.                                                                                 |
| **Chroma Reader**           | Kết nối với Chroma để tải vector embeddings và thực hiện tìm kiếm theo độ tương đồng.                                                    |
| **MyScale Reader**          | Tích hợp với MyScale để truy vấn và lấy dữ liệu từ cơ sở dữ liệu của nó.                                                                 |
| **FAISS Reader**            | Sử dụng FAISS để tìm kiếm và truy xuất dữ liệu vector một cách hiệu quả.                                                                 |
| **Obsidian Reader**         | Đọc các ghi chú và tài liệu từ Obsidian, tích hợp với hệ thống ghi chú của bạn.                                                          |
| **Slack Reader**            | Lấy tin nhắn, tệp và dữ liệu từ các kênh và không gian làm việc Slack.                                                                   |
| **Webpage Reader**          | Trích xuất nội dung từ các trang web, cho phép bạn lập chỉ mục và tìm kiếm thông tin trên web.                                           |
| **Pinecone Reader**         | Kết nối với Pinecone để đọc và truy vấn dữ liệu vector được lưu trữ trong cơ sở dữ liệu vector của nó.                                   |
| **Pathway Reader**          | Tích hợp với Pathway để truy cập và xử lý các luồng dữ liệu hoặc tập dữ liệu phức tạp.                                                   |
| **MBox Reader**             | Đọc và xử lý các kho lưu trữ email được lưu trữ ở định dạng MBox.                                                                        |
| **Milvus Reader**           | Tích hợp với Milvus để tìm kiếm và truy xuất dữ liệu theo độ tương đồng vector.                                                          |
| **Notion Reader**           | Truy xuất và đọc nội dung từ các trang, cơ sở dữ liệu, và tài liệu của Notion.                                                           |
| **Github Reader**           | Trích xuất mã nguồn, các vấn đề và dữ liệu khác từ các kho lưu trữ GitHub.                                                               |
| **Google Docs Reader**      | Truy xuất và xử lý tài liệu được lưu trữ trên Google Docs.                                                                               |
| **Database Reader**         | Reader chung để truy vấn các cơ sở dữ liệu SQL và lấy các bản ghi.                                                                       |
| **Twitter Reader**          | Kết nối với Twitter API để lấy các tweet, dòng thời gian và dữ liệu người dùng.                                                          |
| **Weaviate Reader**         | Kết nối với Weaviate để truy cập và tìm kiếm dữ liệu đã được vector hóa.                                                                 |
| **Make Reader**             | Tích hợp với Make (trước đây là Integromat) để tự động hóa quy trình làm việc và truy xuất dữ liệu.                                      |
| **Deplot Reader**           | Lấy và đọc dữ liệu từ Deplot, một nền tảng quản lý và hiển thị dữ liệu.                                                                  |
