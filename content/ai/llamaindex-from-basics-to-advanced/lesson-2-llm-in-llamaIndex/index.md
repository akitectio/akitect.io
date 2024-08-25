---
categories: [llamaindex]
date: 2024-08-21T23:52:11+0700
description: Trong bài học này, bạn sẽ khám phá cách tích hợp các Mô hình Ngôn ngữ Lớn (LLM) trong LlamaIndex. Bạn sẽ học cách tận dụng LLM cho các tác vụ như hoàn thành văn bản và AI trò chuyện, cùng với cách sử dụng LLM như các mô-đun độc lập hoặc tích hợp vào các thành phần khác của LlamaIndex như lập chỉ mục và truy vấn. Bài học cũng bao gồm các khái niệm quan trọng như mã hóa token và cung cấp các ví dụ chi tiết, giúp bạn khai thác tối đa tiềm năng của LLM trong các ứng dụng của mình.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-2-models-in-lamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-2-models-in-lamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex]
title: Lesson 2 - LLM Trong LLamaIndex
weight: 2
---

**Mô hình ngôn ngữ lớn (LLM)** đóng vai trò cốt lõi trong LlamaIndex, mang lại khả năng xử lý ngôn ngữ tự nhiên với độ chính xác cao. Tích hợp LLM trong LlamaIndex giúp bạn tận dụng tối đa sức mạnh của các mô hình ngôn ngữ tiên tiến như OpenAI, Hugging Face, PaLM và nhiều hơn nữa. Từ việc hoàn thành văn bản đến hỗ trợ trò chuyện, LLM không chỉ hoạt động độc lập mà còn kết hợp chặt chẽ với các mô-đun khác trong LlamaIndex để tối ưu hóa hiệu suất và đáp ứng nhu cầu phức tạp của người dùng.

## Khái niệm

**Lựa chọn Mô hình Ngôn ngữ Lớn (LLM) phù hợp là một trong những bước đầu tiên bạn cần xem xét khi xây dựng bất kỳ ứng dụng LLM nào trên dữ liệu của bạn.**

LLM là một thành phần cốt lõi của LlamaIndex. Chúng có thể được sử dụng như các mô-đun độc lập hoặc được cắm vào các mô-đun LlamaIndex cốt lõi khác (chỉ mục, bộ truy xuất, công cụ truy vấn). Chúng luôn được sử dụng trong bước tổng hợp phản hồi (ví dụ: sau khi truy xuất). Tùy thuộc vào loại chỉ mục đang được sử dụng, LLM cũng có thể được sử dụng trong quá trình xây dựng chỉ mục, chèn và duyệt truy vấn.

LlamaIndex cung cấp một giao diện thống nhất để xác định các mô-đun LLM, cho dù đó là từ OpenAI, Hugging Face hay LangChain, để bạn không phải tự viết mã mẫu để xác định giao diện LLM. Giao diện này bao gồm các yếu tố sau (chi tiết hơn bên dưới):

-   Hỗ trợ cho các điểm cuối **hoàn thành văn bản** và **trò chuyện** (chi tiết bên dưới)

-   Hỗ trợ cho các điểm cuối **truyền trực tuyến** và **không truyền trực tuyến**

-   Hỗ trợ cho các điểm cuối **đồng bộ** và **không đồng bộ** 

-   ### Mô hình Sử dụng

Đoạn mã sau đây cho thấy cách bạn có thể bắt đầu sử dụng LLM.

```bash
pip install llama-index-llms-openai
```

Sau đó tạo tiệp `llama_demo.py`:

```python
from llama_index.llms.openai import OpenAI

resp = OpenAI().complete("Akitect.io là")
print(resp)
```

-   #### Sử dụng LLM như các mô-đun độc lập

    Bạn có thể sử dụng các mô-đun LLM một cách độc lập trong LlamaIndex, dưới đây là cách thực hiện.

#### **Ví dụ về Hoàn thành Văn bản**

```python
from llama_index.llms.openai import OpenAI

# Hoàn thành không truyền trực tuyến
completion = OpenAI().complete("akitect.io là")
print(completion)

# Hoàn thành truyền trực tuyến
llm = OpenAI()
completions = llm.stream_complete("akitect.io là")
for completion in completions:
print(completion.delta, end="")
```

-   **Hoàn thành không truyền trực tuyến**: Bạn sử dụng `OpenAI()` để tạo đối tượng LLM và gọi phương thức `complete`. Phương thức này nhận vào một chuỗi văn bản ("akitect.io là") và trả về phần hoàn thành câu. Ví dụ này không yêu cầu kết nối liên tục với mô hình, và kết quả được trả về ngay lập tức sau khi hoàn tất xử lý.

-   **Hoàn thành truyền trực tuyến**: Với `stream_complete`, bạn nhận kết quả từng phần theo thời gian thực khi mô hình tạo ra nội dung. Điều này hữu ích trong các trường hợp cần phản hồi nhanh, như trong các ứng dụng tương tác trực tiếp với người dùng. Dữ liệu được in ra liên tục khi từng phần hoàn thành được gửi từ mô hình.

#### **Ví dụ về Trò chuyện**

```python
from llama_index.core.llms import ChatMessage
from llama_index.llms.openai import OpenAI

messages = [
ChatMessage(role="system", content="Bạn là một tên cướp với tính cách đầy màu sắc"),
ChatMessage(role="user", content="Tên của bạn là gì"),
]
resp = OpenAI().chat(messages)
print(resp)
```

-   **Trò chuyện**: Trong ví dụ này, bạn sử dụng mô-đun LLM để tạo ra một cuộc hội thoại. `ChatMessage` được sử dụng để thiết lập bối cảnh cho mô hình, ví dụ như nói rằng mô hình là một "tên cướp với tính cách đầy màu sắc". Sau đó, bạn có thể gửi câu hỏi từ người dùng ("Tên của bạn là gì") để nhận phản hồi từ mô hình.

-   Phương thức `chat` nhận vào danh sách các tin nhắn và trả về phản hồi của mô hình. Điều này mô phỏng cách mà LLM có thể giữ ngữ cảnh qua nhiều lượt tương tác, rất hữu ích trong các ứng dụng chatbot hoặc trợ lý ảo.

-   ### Lưu ý về Tokenization (Mã hóa)

Theo mặc định, LlamaIndex sử dụng một tokenizer toàn cục cho tất cả việc đếm token. Mặc định này là `cl100k` từ tiktoken, là tokenizer để phù hợp với LLM mặc định `gpt-3.5-turbo`.

Nếu bạn thay đổi LLM, bạn có thể cần cập nhật tokenizer này để đảm bảo số lượng token, phân đoạn và nhắc nhở chính xác.

Yêu cầu duy nhất cho một tokenizer là nó phải là một hàm có thể gọi được, nhận một chuỗi và trả về một danh sách.

Bạn có thể đặt một tokenizer toàn cục như sau:

```python
from llama_index.core import Settings

# tiktoken
import tiktoken
Settings.tokenizer = tiktoken.encoding_for_model("gpt-3.5-turbo").encode

# huggingface
from transformers import AutoTokenizer
Settings.tokenizer = AutoTokenizer.from_pretrained(

  "HuggingFaceH4/zephyr-7b-beta")

```

-   **Tokenizer:** Một tokenizer là một công cụ chia văn bản thành các đơn vị nhỏ hơn gọi là token. Các token này có thể là từ, ký tự hoặc các phần nhỏ hơn của từ.
-   **cl100k:** Đây là một tokenizer cụ thể được sử dụng bởi OpenAI cho các mô hình như `gpt-3.5-turbo`.
-   **Thay đổi tokenizer:** Nếu bạn sử dụng một LLM khác, bạn có thể cần thay đổi tokenizer để đảm bảo tính chính xác trong việc xử lý văn bản.
-   **Yêu cầu của tokenizer:** Một tokenizer phải là một hàm có thể gọi được, nhận một chuỗi làm đầu vào và trả về một danh sách các token.
-   **Đặt tokenizer toàn cục:** Bạn có thể sử dụng `Settings.tokenizer` để đặt tokenizer toàn cục cho LlamaIndex. Ví dụ cung cấp cách sử dụng tokenizer từ tiktoken hoặc HuggingFace. 


-   ### Các tích hợp LLM có sẵn

Dưới đây là bảng liệt kê các tích hợp LLM có sẵn cùng với liên kết đến các tài liệu tương ứng:

| **LLM**                           | **Liên Kết Tài Liệu**                                                                         |
| --------------------------------- | --------------------------------------------------------------------------------------------- |
| **AI21**                          | [AI21](https://www.ai21.com/)                                                                 |
| **Anthropic**                     | [Anthropic](https://www.anthropic.com/)                                                       |
| **AnyScale**                      | [AnyScale](https://www.anyscale.com/)                                                         |
| **Azure OpenAI**                  | [Azure OpenAI](https://azure.microsoft.com/en-us/services/cognitive-services/openai-service/) |
| **Bedrock**                       | [Bedrock](https://aws.amazon.com/bedrock/)                                                    |
| **Clarifai**                      | [Clarifai](https://www.clarifai.com/)                                                         |
| **Cohere**                        | [Cohere](https://cohere.ai/)                                                                  |
| **Dashscope**                     | [Dashscope](https://dashscope.ai/)                                                            |
| **Dashscope Multi-Modal**         | [Dashscope Multi-Modal](https://dashscope.ai/)                                                |
| **EverlyAI**                      | [EverlyAI](https://www.everlyai.com/)                                                         |
| **Fireworks**                     | [Fireworks](https://fireworks.ai/)                                                            |
| **Friendli**                      | [Friendli](https://friendli.ai/)                                                              |
| **Gradient**                      | [Gradient](https://gradient.com/)                                                             |
| **Gradient Model Adapter**        | [Gradient Model Adapter](https://gradient.com/)                                               |
| **Groq**                          | [Groq](https://groq.com/)                                                                     |
| **HuggingFace Camel-7B**          | [HuggingFace Camel-7B](https://huggingface.co/CAMEL)                                          |
| **HuggingFace StableLM**          | [HuggingFace StableLM](https://huggingface.co/stableLM)                                       |
| **HuggingFace Llama2**            | [HuggingFace Llama2](https://huggingface.co/Llama-2)                                          |
| **Konko**                         | [Konko](https://konko.ai/)                                                                    |
| **LangChain**                     | [LangChain](https://www.langchain.com/)                                                       |
| **LiteLLM**                       | [LiteLLM](https://litellm.com/)                                                               |
| **Llama API**                     | [Llama API](https://llamaapi.com/)                                                            |
| **Llama CPP**                     | [Llama CPP](https://llamacpp.com/)                                                            |
| **LocalAI**                       | [LocalAI](https://localai.com/)                                                               |
| **MariTalk**                      | [MariTalk](https://maritalk.com/)                                                             |
| **MistralAI**                     | [MistralAI](https://mistral.ai/)                                                              |
| **Modelscope**                    | [Modelscope](https://modelscope.cn/)                                                          |
| **MonsterAPI**                    | [MonsterAPI](https://monsterapi.com/)                                                         |
| **MyMagic**                       | [MyMagic](https://mymagic.ai/)                                                                |
| **NeutrinoAI**                    | [NeutrinoAI](https://neutrinoapi.net/)                                                        |
| **Nvidia TensorRT-LLM**           | [Nvidia TensorRT-LLM](https://developer.nvidia.com/tensorrt)                                  |
| **Nvidia Triton**                 | [Nvidia Triton](https://developer.nvidia.com/nvidia-triton-inference-server)                  |
| **Ollama**                        | [Ollama](https://ollama.com/)                                                                 |
| **OpenAI**                        | [OpenAI](https://openai.com/)                                                                 |
| **OpenLLM**                       | [OpenLLM](https://openllm.ai/)                                                                |
| **OpenRouter**                    | [OpenRouter](https://openrouter.com/)                                                         |
| **PaLM**                          | [PaLM](https://palm.dev/)                                                                     |
| **Perplexity**                    | [Perplexity](https://www.perplexity.ai/)                                                      |
| **PremAI**                        | [PremAI](https://premai.com/)                                                                 |
| **Portkey**                       | [Portkey](https://portkey.ai/)                                                                |
| **Predibase**                     | [Predibase](https://predibase.com/)                                                           |
| **Replicate Llama2**              | [Replicate Llama2](https://replicate.com/llama2)                                              |
| **Replicate Vicuna**              | [Replicate Vicuna](https://replicate.com/vicuna)                                              |
| **Replicate Vector Index Llama2** | [Replicate Vector Index Llama2](https://replicate.com/llama2-vector-index)                    |
| **RunGPT**                        | [RunGPT](https://rungpt.com/)                                                                 |
| **SageMaker**                     | [SageMaker](https://aws.amazon.com/sagemaker/)                                                |
| **Solar**                         | [Solar](https://solar.ai/)                                                                    |
| **Together.ai**                   | [Together.ai](https://together.ai/)                                                           |
| **Unify AI**                      | [Unify AI](https://unify.ai/)                                                                 |
| **Upstage**                       | [Upstage](https://upstage.ai/)                                                                |
| **Vertex**                        | [Vertex](https://cloud.google.com/vertex-ai)                                                  |
| **vLLM**                          | [vLLM](https://vllm.ai/)                                                                      |
| **Xorbits Inference**             | [Xorbits Inference](https://xorbits.io/)                                                      |
| **Yi**                            | [Yi](https://yi.ai/)                                                                          |
