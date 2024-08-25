---
categories: [llamaindex]
date: 2024-08-22T23:52:11+0700
description: Prompts trong LlamaIndex tìm hiểu cách prompts hướng dẫn các Mô hình Ngôn ngữ Lớn (LLM) trong các tác vụ như lập chỉ mục, truy vấn và tạo phản hồi. Bài học này dạy bạn cách tùy chỉnh prompts để nâng cao độ chính xác và tính linh hoạt của ứng dụng LLM.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-4-prompts-in-llamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-4-prompts-in-llamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, embeddings]
title: Lesson 4 - Prompts trong LlamaIndex
weight: 4
---

## Prompts: Đòn bẩy sức mạnh của LLM

### Khái niệm

Prompts (lời nhắc) đóng vai trò then chốt, cung cấp đầu vào cơ bản để khai thác sức mạnh biểu đạt của các Mô hình Ngôn ngữ Lớn (LLM). Trong LlamaIndex, prompts được sử dụng rộng rãi trong các giai đoạn:

-   **Xây dựng chỉ mục:** Định hình cách thông tin được tổ chức và lưu trữ.
-   **Chèn dữ liệu:** Hướng dẫn cách dữ liệu mới được thêm vào chỉ mục.
-   **Truy vấn và duyệt:** Điều khiển cách LLM tìm kiếm và xử lý thông tin trong chỉ mục để trả lời truy vấn.
-   **Tổng hợp câu trả lời:** Xây dựng cấu trúc và nội dung của câu trả lời cuối cùng.

LlamaIndex cung cấp một bộ **mẫu prompts mặc định** hoạt động hiệu quả ngay lập tức. Ngoài ra, còn có các prompts được viết riêng cho các mô hình trò chuyện như `gpt-3.5-turbo`.

Người dùng có thể cung cấp các **mẫu prompts tùy chỉnh** để tinh chỉnh hành vi của hệ thống. Cách tốt nhất để tùy chỉnh là sao chép prompt mặc định từ liên kết ở trên và sử dụng nó làm cơ sở cho bất kỳ sửa đổi nào.

### Mô hình sử dụng

Sử dụng prompts rất đơn giản.

```python
from llama_index.core import PromptTemplate

template = (
    "We have provided context information below. \n"
    "---------------------\n"
    "{context_str}"
    "\n---------------------\n"
    "Given this information, please answer the question: {query_str}\n")

qa_template = PromptTemplate(template)

# Tạo prompt văn bản (cho API hoàn thành)
prompt = qa_template.format(context_str=..., query_str=...)

# Hoặc chuyển đổi thành prompt tin nhắn (cho API trò chuyện)
messages = qa_template.format_messages(context_str=..., query_str=...)
```

## Mô hình sử dụng: Định nghĩa và tùy chỉnh Prompts

### Định nghĩa một prompt tùy chỉnh

Tạo một prompt tùy chỉnh trong LlamaIndex rất đơn giản, bạn chỉ cần tạo một chuỗi định dạng (format string).

```python
from llama_index.core import PromptTemplate

template = (
    "We have provided context information below. \n"
    "---------------------\n"
    "{context_str}"
    "\n---------------------\n"
    "Given this information, please answer the question: {query_str}\n")

qa_template = PromptTemplate(template)

# Tạo prompt văn bản (cho API hoàn thành)
prompt = qa_template.format(context_str=..., query_str=...)

# Hoặc chuyển đổi thành prompt tin nhắn (cho API trò chuyện)
messages = qa_template.format_messages(context_str=..., query_str=...)
```

**Lưu ý:** Các lớp con prompt cũ như `QuestionAnswerPrompt`, `RefinePrompt` đã bị khai tử (và hiện là bí danh kiểu của `PromptTemplate`). Giờ đây, bạn có thể chỉ định trực tiếp `PromptTemplate(template)` để xây dựng các prompt tùy chỉnh. Tuy nhiên, bạn vẫn phải đảm bảo rằng chuỗi mẫu có chứa các tham số mong đợi (ví dụ: `{context_str}` và `{query_str}`) khi thay thế một prompt câu hỏi trả lời mặc định.

Bạn cũng có thể định nghĩa một mẫu từ các tin nhắn trò chuyện:

```python
from llama_index.core import ChatPromptTemplate
from llama_index.core.llms import ChatMessage, MessageRole

message_templates = [
    ChatMessage(content="You are an expert system.", role=MessageRole.SYSTEM),
    ChatMessage(
        content="Generate a short story about {topic}",
        role=MessageRole.USER,
    ),
]

chat_template = ChatPromptTemplate(message_templates=message_templates)

# Tạo prompt tin nhắn (cho API trò chuyện)
messages = chat_template.format_messages(topic=...)

# Hoặc chuyển đổi thành prompt văn bản (cho API hoàn thành)
prompt = chat_template.format(topic=...)
```

### Lấy và Đặt Prompts Tùy chỉnh

Vì LlamaIndex là một quy trình nhiều bước, điều quan trọng là xác định hoạt động bạn muốn sửa đổi và chuyển prompt tùy chỉnh vào đúng vị trí.

Ví dụ: prompts được sử dụng trong bộ tổng hợp phản hồi, bộ truy xuất, xây dựng chỉ mục, v.v.; một số mô-đun này được lồng trong các mô-đun khác (bộ tổng hợp được lồng trong công cụ truy vấn).

### Prompts Thường được Sử dụng

Các prompts thường được sử dụng nhất sẽ là `text_qa_template` và `refine_template`.

-   `text_qa_template` - được sử dụng để nhận câu trả lời ban đầu cho một truy vấn bằng cách sử dụng các nút đã truy xuất.
-   `refine_template` - được sử dụng khi văn bản đã truy xuất không phù hợp với một lệnh gọi LLM duy nhất với `response_mode="compact"` (mặc định), hoặc khi nhiều hơn một nút được truy xuất bằng `response_mode="refine"`. Câu trả lời từ truy vấn đầu tiên được chèn dưới dạng `existing_answer`, và LLM phải cập nhật hoặc lặp lại câu trả lời hiện có dựa trên ngữ cảnh mới.

### Truy cập Prompts

Bạn có thể gọi `get_prompts` trên nhiều mô-đun trong LlamaIndex để nhận được một danh sách phẳng các prompts được sử dụng trong mô-đun và các mô-đun con lồng nhau.

Ví dụ: hãy xem đoạn mã sau đây.

```python
query_engine = index.as_query_engine(response_mode="compact")
prompts_dict = query_engine.get_prompts()
print(list(prompts_dict.keys()))
```

Bạn có thể nhận lại các khóa sau:

```python
['response_synthesizer:text_qa_template', 'response_synthesizer:refine_template']
```

Lưu ý rằng các prompts được thêm tiền tố bởi các mô-đun con của chúng dưới dạng "không gian tên".

### Cập nhật Prompts

Bạn có thể tùy chỉnh prompts trên bất kỳ mô-đun nào triển khai `get_prompts` với hàm `update_prompts`. Chỉ cần chuyển vào các giá trị đối số với các khóa bằng với các khóa bạn thấy trong từ điển prompt thu được thông qua `get_prompts`.

Ví dụ: liên quan đến ví dụ trên, chúng ta có thể làm như sau

```python
# shakespeare!
qa_prompt_tmpl_str = (
    "Context information is below.\n"
    "---------------------\n"
    "{context_str}\n"
    "---------------------\n"
    "Given the context information and not prior knowledge, "
    "answer the query in the style of a Shakespeare play.\n"
    "Query: {query_str}\n"
    "Answer: ")
qa_prompt_tmpl = PromptTemplate(qa_prompt_tmpl_str)
query_engine.update_prompts(
    {"response_synthesizer:text_qa_template": qa_prompt_tmpl})
```

### Sửa đổi prompts được sử dụng trong công cụ truy vấn

Đối với các công cụ truy vấn, bạn cũng có thể chuyển các prompt tùy chỉnh trực tiếp trong thời gian truy vấn (tức là để thực hiện truy vấn đối với chỉ mục và tổng hợp phản hồi cuối cùng).

Ngoài ra còn có hai cách tương đương để ghi đè các prompts:

-   Thông qua API cấp cao

```python
query_engine = index.as_query_engine(
    text_qa_template=custom_qa_prompt, refine_template=custom_refine_prompt)
```

-   Thông qua API thành phần cấp thấp

```python
retriever = index.as_retriever()
synth = get_response_synthesizer(
    text_qa_template=custom_qa_prompt, refine_template=custom_refine_prompt)
query_engine = RetrieverQueryEngine(retriever, response_synthesizer)
```

Hai cách tiếp cận trên là tương đương, trong đó 1 về cơ bản là đường cú pháp cho 2 và ẩn đi sự phức tạp cơ bản. Bạn có thể muốn sử dụng 1 để nhanh chóng sửa đổi một số tham số phổ biến và sử dụng 2 để có nhiều quyền kiểm soát chi tiết hơn.

Để biết thêm chi tiết về lớp nào sử dụng prompt nào, vui lòng truy cập Tài liệu tham khảo lớp truy vấn.

Xem tài liệu tham khảo để biết một bộ đầy đủ tất cả các prompts.

### Sửa đổi prompts được sử dụng trong xây dựng chỉ mục

Một số chỉ mục sử dụng các loại prompts khác nhau trong quá trình xây dựng (LƯU Ý: các chỉ mục phổ biến nhất, `VectorStoreIndex` và `SummaryIndex`, không sử dụng bất kỳ chỉ mục nào).

Ví dụ: `TreeIndex` sử dụng một prompt tóm tắt để tóm tắt phân cấp các nút và `KeywordTableIndex` sử dụng một prompt trích xuất từ khóa để trích xuất từ khóa.

Có hai cách tương đương để ghi đè các prompts:

-   Thông qua hàm tạo nút mặc định

```python
index = TreeIndex(nodes, summary_template=custom_prompt)
```

-   Thông qua hàm tạo tài liệu.

```python
index = TreeIndex.from_documents(docs, summary_template=custom_prompt)
```

Để biết thêm chi tiết về chỉ mục nào sử dụng prompt nào, vui lòng truy cập Tài liệu tham khảo lớp chỉ mục. 

### [Nâng cao] Khả năng Prompt Nâng cao

Trong phần này, chúng tôi trình bày một số khả năng prompt nâng cao trong LlamaIndex.

**Hướng dẫn liên quan:**

-   Prompts nâng cao
-   Kỹ thuật Prompt cho RAG

#### Định dạng một phần

Định dạng một phần prompt, điền vào một số biến trong khi để lại những biến khác để điền vào sau.

```python
from llama_index.core import PromptTemplate

prompt_tmpl_str = "{foo} {bar}"
prompt_tmpl = PromptTemplate(prompt_tmpl_str)
partial_prompt_tmpl = prompt_tmpl.partial_format(foo="abc")
fmt_str = partial_prompt_tmpl.format(bar="def")
```

#### Ánh xạ biến mẫu

Các cấu trúc trừu tượng prompt của LlamaIndex thường mong đợi một số khóa nhất định. Ví dụ: `text_qa_prompt` của chúng tôi mong đợi `context_str` cho ngữ cảnh và `query_str` cho truy vấn của người dùng.

Nhưng nếu bạn đang cố gắng điều chỉnh một mẫu chuỗi để sử dụng với LlamaIndex, có thể khó chịu khi thay đổi các biến mẫu.

Thay vào đó, hãy định nghĩa `template_var_mappings`:

```python
template_var_mappings = {"context_str": "my_context", "query_str": "my_query"}
prompt_tmpl = PromptTemplate(
    qa_prompt_tmpl_str, template_var_mappings

```

Prompts là chìa khóa để khai thác sức mạnh của LLM trong LlamaIndex. Bằng cách hiểu và tùy chỉnh prompts, bạn có thể xây dựng các ứng dụng tìm kiếm và truy xuất thông tin mạnh mẽ, linh hoạt và hiệu quả. 
