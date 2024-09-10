---
categories: [llamaindex]
date: 2024-08-25T18:52:11+0700
description: SimpleDirectoryReader is a convenience tool in LlamaIndex that allows you to easily load data from local files into the system. It supports many popular file formats and provides flexible options for customizing the data loading process, including reading from subfolders, filtering files by extension, and extracting metadata.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-6-loading-simple-directory-reader-in-llamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-6-loading-simple-directory-reader-in-llamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, SimpleDirectoryReader]
title: Lesson 6 - SimpleDirectoryReader in llamaIndex
weight: 6
---

**SimpleDirectoryReader** is the simplest way to load data from local files into LlamaIndex. For production use cases, you might want to utilize one of the many Readers available on LlamaHub, but SimpleDirectoryReader is an excellent way to start.

### Supported File Types

By default, SimpleDirectoryReader tries to read any file it finds, treating them as text. Besides plain text, it explicitly supports the following file types, automatically detected based on the file extension:

-   `.csv` - Comma-Separated Values
-   `.docx` - Microsoft Word
-   `.epub` - EPUB eBook Format
-   `.hwp` - Hangul Word Processor
-   `.ipynb` - Jupyter Notebook
-   `.jpeg`, `.jpg` - JPEG Images
-   `.mbox` - MBOX Email Archives
-   `.md` - Markdown
-   `.mp3`, `.mp4` - Audio and Video
-   `.pdf` - Portable Document Format
-   `.png` - Portable Network Graphics
-   `.ppt`, `.pptm`, `.pptx` - Microsoft PowerPoint

One file type you might expect to find here is JSON; for that, we recommend using our JSON Loader.

### How to Use

The most basic usage is to pass an `input_dir`, and it will load all supported files in that directory:

```python
from llama_index.core import SimpleDirectoryReader

reader = SimpleDirectoryReader(input_dir="path/to/directory")
documents = reader.load_data()
```

Documents can also be loaded with parallel processing if loading many files from a directory. Note that there are differences when using `multiprocessing` with Windows and Linux/MacOS machines, which are explained throughout the multiprocessing documentation. Finally, Windows users may see little or no performance benefit, while Linux/MacOS users will see these benefits when loading the same set of files.

```python
...
documents = reader.load_data(num_workers=4)
```

### Reading from Subdirectories

By default, `SimpleDirectoryReader` will only read files at the top level of the directory. To read from subdirectories, set `recursive=True`:

```python
SimpleDirectoryReader(input_dir="path/to/directory", recursive=True)
```

### Iterate Over Files as They Are Loaded

You can also use the `iter_data()` method to iterate over and process files as they are loaded.

```python
reader = SimpleDirectoryReader(input_dir="path/to/directory", recursive=True)
all_docs = []
for docs in reader.iter_data():
    # <do something with the documents per file>
    all_docs.extend(docs)
```

### Limiting Loaded Files

Instead of all files, you can pass a list of file paths:

```python
SimpleDirectoryReader(input_files=["path/to/file1", "path/to/file2"])
```

or you can pass a list of file paths to **exclude** using `exclude`:

```python
SimpleDirectoryReader(
    input_dir="path/to/directory", exclude=["path/to/file1", "path/to/file2"]
)
```

You can also set `required_exts` to a list of file extensions to load only files with those extensions:

```python
SimpleDirectoryReader(
    input_dir="path/to/directory", required_exts=[".pdf", ".docx"]
)
```

And you can set the maximum number of files to be loaded with `num_files_limit`:

```python
SimpleDirectoryReader(input_dir="path/to/directory", num_files_limit=100)
```

### Specify File Encoding

`SimpleDirectoryReader` expects files to be encoded in `utf-8`, but you can override this by using the `encoding` parameter:

```python
SimpleDirectoryReader(input_dir="path/to/directory", encoding="latin-1")
```

### Extracting Metadata

You can specify a function that will read each file and extract metadata attached to the resulting `Document` object for each file by passing the function as `file_metadata`:

```python
def get_meta(file_path):
    return {"foo": "bar", "file_path": file_path}

SimpleDirectoryReader(input_dir="path/to/directory", file_metadata=get_meta)
```

This function should take a single argument, the file path, and return a dictionary of metadata.

### Extending to Other File Types

You can extend `SimpleDirectoryReader` to read other file types by passing a dictionary of file extensions to instances of `BaseReader` as `file_extractor`. A `BaseReader` should read a file and return a list of `Document`. For example, to add custom support for `.myfile` files:

```python
from llama_index.core import SimpleDirectoryReader
from llama_index.core.readers.base import BaseReader
from llama_index.core import Document

class MyFileReader(BaseReader):
    def load_data(self, file, extra_info=None):
        with open(file, "r") as f:
            text = f.read()
        # load_data returns a list of Document objects
        return [Document(text=text + "Foobar", extra_info=extra_info or {})]

reader = SimpleDirectoryReader(
    input_dir="./data", file_extractor={".myfile": MyFileReader()}
)
documents = reader.load_data()
print(documents)
```

Note that this mapping will override the default file extractors for the file types you specify, so you will need to add them back if you want to support them.

### Support for External File Systems

Like other modules, `SimpleDirectoryReader` takes an optional `fs` parameter that can be used to browse remote file systems.

This can be any file system object implemented by the `fsspec` protocol. The `fsspec` protocol has open-source implementations for many remote file systems, including AWS S3, Azure Blob & DataLake, Google Drive, SFTP, and many others.

Here is an example of connecting to S3:

```python
from s3fs import S3FileSystem

s3_fs = S3FileSystem(key="...", secret="...")
bucket_name = "my-document-bucket"
reader = SimpleDirectoryReader(
    input_dir=bucket_name,
    fs=s3_fs,
    recursive=True,  # recursively search all subdirectories
)
documents = reader.load_data()
print(documents)
```
