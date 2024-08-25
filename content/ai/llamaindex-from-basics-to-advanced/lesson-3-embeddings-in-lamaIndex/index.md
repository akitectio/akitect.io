---
categories: [llamaindex]
date: 2024-08-22T23:52:11+0700
description: Trong bài học này, bạn sẽ tìm hiểu cách các embedding hoạt động trong LlamaIndex. Bạn sẽ học cách văn bản được chuyển đổi thành các biểu diễn số thông qua embedding, giúp các mô hình ngôn ngữ lớn (LLM) xử lý và hiểu dữ liệu hiệu quả hơn. Bài học sẽ bao gồm các loại embedding khác nhau, các trường hợp sử dụng, và cách triển khai chúng trong ứng dụng của bạn để cải thiện hiệu suất và độ chính xác.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-3-embeddings-in-lamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-3-embeddings-in-lamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, embeddings]
title: lesson 3 - Embeddings trong LLamaIndex
weight: 3
---

**Embeddings** đóng vai trò quan trọng trong LlamaIndex, giúp chuyển đổi văn bản thành các biểu diễn số phức tạp để mô hình ngôn ngữ lớn (LLM) có thể hiểu và xử lý ngữ nghĩa một cách hiệu quả. Đây là bước quan trọng giúp tối ưu hóa các ứng dụng như tìm kiếm và phân loại văn bản. Trong bài viết này, chúng ta sẽ khám phá cách sử dụng embeddings trong LlamaIndex, từ cài đặt cơ bản đến tùy chỉnh nâng cao, và làm thế nào để tích hợp chúng vào hệ thống của bạn.

## Embeddings: Biểu diễn ngữ nghĩa cho tìm kiếm thông minh

**Khái niệm**

Embeddings đóng vai trò then chốt trong LlamaIndex bằng cách chuyển đổi tài liệu của bạn thành một dạng biểu diễn số tinh vi. Các mô hình nhúng nhận văn bản đầu vào và tạo ra một danh sách các số đại diện cho ý nghĩa của văn bản đó. Các mô hình này đã được huấn luyện để biểu diễn văn bản theo cách này, hỗ trợ nhiều ứng dụng, đặc biệt là tìm kiếm.

Về cơ bản, nếu người dùng hỏi về "chó", thì embedding của câu hỏi đó sẽ rất giống với embedding của văn bản nói về "chó".

Khi tính toán sự tương đồng giữa các embeddings, có nhiều phương pháp như tích vô hướng, cosine similarity,... Theo mặc định, LlamaIndex sử dụng cosine similarity để so sánh embeddings.

Có nhiều mô hình nhúng để lựa chọn. Mặc định, LlamaIndex sử dụng `text-embedding-ada-002` từ OpenAI. Chúng tôi cũng hỗ trợ bất kỳ mô hình nhúng nào từ Langchain, đồng thời cung cấp một lớp cơ sở dễ mở rộng để bạn tự triển khai embeddings của riêng mình. 

## Mô hình sử dụng Embeddings

### Cách sử dụng phổ biến

Trong LlamaIndex, cách sử dụng phổ biến nhất của mô hình nhúng (embedding) là chỉ định nó trong đối tượng `Settings` toàn cục, sau đó sử dụng trong một chỉ mục vectơ. Mô hình nhúng sẽ được sử dụng để:

-   **Nhúng tài liệu:** Chuyển đổi các tài liệu thành biểu diễn vectơ trong quá trình xây dựng chỉ mục.
-   **Nhúng truy vấn:** Chuyển đổi truy vấn của người dùng thành vectơ để tìm kiếm các tài liệu tương tự trong chỉ mục.

Bạn cũng có thể chỉ định mô hình nhúng riêng cho từng chỉ mục nếu cần.

### Cài đặt

Nếu bạn chưa cài đặt gói embeddings:

```bash
pip install llama-index-embeddings-openai
```

Sau đó, bạn có thể sử dụng mô hình nhúng OpenAI như sau:

```python
from llama_index.embeddings.openai import OpenAIEmbedding
from llama_index.core import VectorStoreIndex
from llama_index.core import Settings

# Cài đặt toàn cục
Settings.embed_model = OpenAIEmbedding()

# Cài đặt cho từng chỉ mục
index = VectorStoreIndex.from_documents(documents, embed_model=embed_model)
```

### Sử dụng mô hình cục bộ

Để tiết kiệm chi phí, bạn có thể sử dụng mô hình nhúng cục bộ từ Hugging Face:

```bash
pip install llama-index-embeddings-huggingface
```

```python
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
from llama_index.core import Settings

Settings.embed_model = HuggingFaceEmbedding(
    model_name="BAAI/bge-small-en-v1.5")
```

Mô hình này có hiệu suất tốt và tốc độ nhanh.

## Tùy chỉnh Embeddings

### Kích thước lô (Batch Size)

Theo mặc định, các yêu cầu embeddings được gửi đến OpenAI theo lô 10. Trong một số trường hợp hiếm hoi, điều này có thể gây ra giới hạn tỷ lệ. Đối với những người dùng nhúng nhiều tài liệu, kích thước lô này có thể quá nhỏ. Bạn có thể tùy chỉnh kích thước lô như sau:

```python
# Đặt kích thước lô là 42
embed_model = OpenAIEmbedding(embed_batch_size=42)
```

### Mô hình nhúng cục bộ

Cách đơn giản nhất để sử dụng mô hình cục bộ là thông qua Hugging Face:

```python
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
from llama_index.core import Settings

Settings.embed_model = HuggingFaceEmbedding(
    model_name="BAAI/bge-small-en-v1.5")
```

### HuggingFace Optimum ONNX Embeddings

LlamaIndex cũng hỗ trợ tạo và sử dụng ONNX embeddings bằng thư viện Optimum từ HuggingFace. Bạn chỉ cần tạo và lưu các ONNX embeddings, sau đó sử dụng chúng.

Một số điều kiện tiên quyết:

```bash
pip install transformers optimum[exporters]
pip install llama-index-embeddings-huggingface-optimum
```

Tạo và lưu mô hình ONNX:

```python
from llama_index.embeddings.huggingface_optimum import OptimumEmbedding

OptimumEmbedding.create_and_save_optimum_model(
    "BAAI/bge-small-en-v1.5", "./bge_onnx")
```

Sử dụng mô hình ONNX:

```python
Settings.embed_model = OptimumEmbedding(folder_name="./bge_onnx")
```

### Tích hợp LangChain

LlamaIndex hỗ trợ bất kỳ embeddings nào được cung cấp bởi Langchain.

Ví dụ dưới đây tải một mô hình từ Hugging Face, sử dụng lớp nhúng của Langchain:

```bash
pip install llama-index-embeddings-langchain
```

```python
from langchain.embeddings.huggingface import HuggingFaceBgeEmbeddings
from llama_index.core import Settings

Settings.embed_model = HuggingFaceBgeEmbeddings(model_name="BAAI/bge-base-en")
```

### Mô hình nhúng tùy chỉnh

Nếu bạn muốn sử dụng embeddings không được cung cấp bởi LlamaIndex hoặc Langchain, bạn có thể mở rộng lớp embeddings cơ sở và tự triển khai!

Ví dụ dưới đây sử dụng Instructor Embeddings và triển khai một lớp embeddings tùy chỉnh. Instructor embeddings hoạt động bằng cách cung cấp văn bản, cùng với "hướng dẫn" về lĩnh vực của văn bản để nhúng. Điều này hữu ích khi nhúng văn bản từ một chủ đề rất cụ thể và chuyên ngành.

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

### Sử dụng Độc lập

Bạn cũng có thể sử dụng embeddings như một mô-đun độc lập cho dự án, ứng dụng hiện có hoặc kiểm tra và khám phá chung.

```python
embeddings = embed_model.get_text_embedding(
    "It is raining cats and dogs here!")
```

### Embedding được hỗ trợ

Dưới đây là bảng liệt kê các mô hình embedding được hỗ trợ cùng với liên kết đến tài liệu của chúng:

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

Như vậy, LlamaIndex hỗ trợ một loạt các mô hình embedding từ các nhà cung cấp hàng đầu như OpenAI, Hugging Face, và nhiều dịch vụ khác. Sự đa dạng này cho phép bạn tùy chỉnh và tối ưu hóa việc sử dụng mô hình ngôn ngữ lớn (LLM) trong các ứng dụng của mình, từ việc tìm kiếm đến phân tích dữ liệu, giúp bạn đạt được hiệu suất cao và đáp ứng nhu cầu cụ thể của dự án.
