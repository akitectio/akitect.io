---
categories: [llamaindex]
date: 2024-08-25T18:52:11+0700
description: SimpleDirectoryReader là một công cụ tiện lợi trong LlamaIndex, cho phép bạn dễ dàng tải dữ liệu từ các tệp cục bộ vào hệ thống. Nó hỗ trợ nhiều định dạng tệp phổ biến và cung cấp các tùy chọn linh hoạt để tùy chỉnh quá trình tải dữ liệu, bao gồm cả việc đọc từ các thư mục con, lọc tệp theo phần mở rộng và trích xuất siêu dữ liệu.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-6-loading-simple-directory-reader-in-llamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-6-loading-simple-directory-reader-in-llamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, SimpleDirectoryReader]
title: Lesson 6 - SimpleDirectoryReader trong llamaIndex
weight: 6
---

**SimpleDirectoryReader** là cách đơn giản nhất để tải dữ liệu từ các tệp cục bộ vào LlamaIndex. Đối với các trường hợp sử dụng trong sản xuất, có nhiều khả năng bạn sẽ muốn sử dụng một trong nhiều Trình đọc (Readers) có sẵn trên LlamaHub, nhưng SimpleDirectoryReader là một cách tuyệt vời để bắt đầu.

### Các loại tệp được hỗ trợ

Theo mặc định, SimpleDirectoryReader sẽ cố gắng đọc bất kỳ tệp nào nó tìm thấy, coi tất cả chúng là văn bản. Ngoài văn bản thuần túy, nó hỗ trợ rõ ràng các loại tệp sau, được tự động phát hiện dựa trên phần mở rộng tệp:

-   `.csv` - các giá trị được phân tách bằng dấu phẩy
-   `.docx` - Microsoft Word
-   `.epub` - Định dạng sách điện tử EPUB
-   `.hwp` - Hangul Word Processor
-   `.ipynb` - Jupyter Notebook
-   `.jpeg`, `.jpg` - Hình ảnh JPEG
-   `.mbox` - Lưu trữ email MBOX
-   `.md` - Markdown
-   `.mp3`, `.mp4` - âm thanh và video
-   `.pdf` - Định dạng tài liệu di động (Portable Document Format)
-   `.png` - Đồ họa mạng di động (Portable Network Graphics)
-   `.ppt`, `.pptm`, `.pptx` - Microsoft PowerPoint

Một loại tệp mà bạn có thể mong đợi tìm thấy ở đây là JSON; đối với loại đó, chúng tôi khuyên bạn nên sử dụng Trình tải JSON của chúng tôi.

### Cách sử dụng

Cách sử dụng cơ bản nhất là truyền một `input_dir` và nó sẽ tải tất cả các tệp được hỗ trợ trong thư mục đó:

```python
from llama_index.core import SimpleDirectoryReader

reader = SimpleDirectoryReader(input_dir="path/to/directory")
documents = reader.load_data()
```

Tài liệu cũng có thể được tải với xử lý song song nếu tải nhiều tệp từ một thư mục. Lưu ý rằng có sự khác biệt khi sử dụng `multiprocessing` với các máy Windows và Linux/MacOS, điều này được giải thích trong suốt tài liệu multiprocessing (ví dụ: xem tại đây). Cuối cùng, người dùng Windows có thể thấy ít hoặc không có lợi ích về hiệu suất trong khi người dùng Linux/MacOS sẽ thấy những lợi ích này khi tải cùng một tập hợp các tệp.

```python
...
documents = reader.load_data(num_workers=4)
```

### Đọc từ các thư mục con

Theo mặc định, `SimpleDirectoryReader` sẽ chỉ đọc các tệp ở cấp cao nhất của thư mục. Để đọc từ các thư mục con, hãy đặt `recursive=True`:

```python
SimpleDirectoryReader(input_dir="path/to/directory", recursive=True)
```

### Lặp qua các tệp khi chúng được tải

Bạn cũng có thể sử dụng phương thức `iter_data()` để lặp qua và xử lý các tệp khi chúng được tải

```python
reader = SimpleDirectoryReader(input_dir="path/to/directory", recursive=True)
all_docs = []
for docs in reader.iter_data():
    # <do something with the documents per file>
    all_docs.extend(docs)
```

### Hạn chế các tệp được tải

Thay vì tất cả các tệp, bạn có thể truyền một danh sách các đường dẫn tệp:

```python
SimpleDirectoryReader(input_files=["path/to/file1", "path/to/file2"])
```

hoặc bạn có thể truyền một danh sách các đường dẫn tệp để **loại trừ** bằng cách sử dụng `exclude`:

```python
SimpleDirectoryReader(
    input_dir="path/to/directory", exclude=["path/to/file1", "path/to/file2"]
)
```

Bạn cũng có thể đặt `required_exts` thành một danh sách các phần mở rộng tệp để chỉ tải các tệp có phần mở rộng đó:

```python
SimpleDirectoryReader(
    input_dir="path/to/directory", required_exts=[".pdf", ".docx"]
)
```

Và bạn có thể đặt số lượng tệp tối đa được tải với `num_files_limit`:

```python
SimpleDirectoryReader(input_dir="path/to/directory", num_files_limit=100)
```

### Chỉ định mã hóa tệp

`SimpleDirectoryReader` mong đợi các tệp được mã hóa `utf-8` nhưng bạn có thể ghi đè điều này bằng cách sử dụng tham số `encoding`:

```python
SimpleDirectoryReader(input_dir="path/to/directory", encoding="latin-1")
```

### Trích xuất siêu dữ liệu (metadata)

Bạn có thể chỉ định một hàm sẽ đọc từng tệp và trích xuất siêu dữ liệu được đính kèm vào đối tượng `Document` kết quả cho mỗi tệp bằng cách truyền hàm làm `file_metadata`:

```python
def get_meta(file_path):
    return {"foo": "bar", "file_path": file_path}

SimpleDirectoryReader(input_dir="path/to/directory", file_metadata=get_meta)
```

Hàm này nên nhận một đối số duy nhất, đường dẫn tệp và trả về một từ điển siêu dữ liệu.

### Mở rộng sang các loại tệp khác

Bạn có thể mở rộng `SimpleDirectoryReader` để đọc các loại tệp khác bằng cách truyền một từ điển các phần mở rộng tệp đến các phiên bản của `BaseReader` làm `file_extractor`. Một `BaseReader` nên đọc tệp và trả về một danh sách các `Document`. Ví dụ: để thêm hỗ trợ tùy chỉnh cho các tệp `.myfile`:

```python
from llama_index.core import SimpleDirectoryReader
from llama_index.core.readers.base import BaseReader
from llama_index.core import Document

class MyFileReader(BaseReader):
    def load_data(self, file, extra_info=None):
        with open(file, "r") as f:
            text = f.read()
        # load_data trả về một danh sách các đối tượng Document
        return [Document(text=text + "Foobar", extra_info=extra_info or {})]

reader = SimpleDirectoryReader(
    input_dir="./data", file_extractor={".myfile": MyFileReader()}
)
documents = reader.load_data()
print(documents)
```

Lưu ý rằng ánh xạ này sẽ ghi đè các trình trích xuất tệp mặc định cho các loại tệp bạn chỉ định, vì vậy bạn sẽ cần thêm chúng trở lại nếu bạn muốn hỗ trợ chúng.

### Hỗ trợ cho Hệ thống tệp bên ngoài

Giống như các mô-đun khác, `SimpleDirectoryReader` nhận một tham số `fs` tùy chọn có thể được sử dụng để duyệt các hệ thống tệp từ xa.

Đây có thể là bất kỳ đối tượng hệ thống tệp nào được triển khai bởi giao thức `fsspec`. Giao thức `fsspec` có các triển khai mã nguồn mở cho nhiều hệ thống tệp từ xa bao gồm AWS S3, Azure Blob & DataLake, Google Drive, SFTP và nhiều hệ thống khác.

Đây là một ví dụ kết nối với S3:

```python
from s3fs import S3FileSystem

s3_fs = S3FileSystem(key="...", secret="...")
bucket_name = "my-document-bucket"
reader = SimpleDirectoryReader(
    input_dir=bucket_name,
    fs=s3_fs,
    recursive=True,  # tìm kiếm đệ quy tất cả các thư mục con
)
documents = reader.load_data()
print(documents)
```
