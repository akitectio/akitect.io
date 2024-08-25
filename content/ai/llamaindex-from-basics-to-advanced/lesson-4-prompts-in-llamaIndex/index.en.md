---
categories: [llamaindex]
date: 2024-08-22T23:52:11+0700
description: Prompts in LlamaIndex explores how prompts guide Large Language Models (LLMs) in tasks like indexing, querying, and response generation. This lesson teaches you how to customize prompts to enhance the precision and flexibility of your LLM applications.
draft: false
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-4-prompts-in-llamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-4-prompts-in-llamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex, embeddings]
title: Lesson 4 - Prompts in LlamaIndex
weight: 4
---

### Prompts: Leveraging the Power of LLMs

### Concept

Prompts play a crucial role by providing the basic input needed to harness the expressive power of Large Language Models (LLMs). In LlamaIndex, prompts are widely used at different stages:

-   **Index construction:** Shaping how information is organized and stored.
-   **Data insertion:** Guiding how new data is added to the index.
-   **Querying and traversal:** Controlling how LLM searches and processes information in the index to answer queries.
-   **Answer synthesis:** Structuring the content of the final response.

LlamaIndex offers a set of **default prompt templates** that work effectively out-of-the-box. Additionally, there are prompts specifically designed for conversational models like `gpt-3.5-turbo`.

Users can provide **custom prompt templates** to fine-tune the system's behavior. The best approach to customization is to copy a default prompt template and use it as a base for any modifications.

### Usage Pattern

Using prompts in LlamaIndex is straightforward.

```python
from llama_index.core import PromptTemplate

template = (
    "We have provided context information below. \n"
    "---------------------\n"
    "{context_str}"
    "\n---------------------\n"
    "Given this information, please answer the question: {query_str}\n")

qa_template = PromptTemplate(template)

# Create text prompt (for completion API)
prompt = qa_template.format(context_str=..., query_str=...)

# Or convert to a message prompt (for chat API)
messages = qa_template.format_messages(context_str=..., query_str=...)
```

### Customizing Prompts: Definition and Implementation

#### Defining a Custom Prompt

Creating a custom prompt in LlamaIndex is simple---you just need to create a format string.

```python
from llama_index.core import PromptTemplate

template = (
    "We have provided context information below. \n"
    "---------------------\n"
    "{context_str}"
    "\n---------------------\n"
    "Given this information, please answer the question: {query_str}\n")

qa_template = PromptTemplate(template)

# Create text prompt (for completion API)
prompt = qa_template.format(context_str=..., query_str=...)

# Or convert to a message prompt (for chat API)
messages = qa_template.format_messages(context_str=..., query_str=...)
```

**Note:** Older prompt subclasses like `QuestionAnswerPrompt`, `RefinePrompt` have been deprecated (and are now type aliases for `PromptTemplate`). You can now directly specify `PromptTemplate(template)` to build custom prompts. However, you still need to ensure that the template string contains the expected parameters (e.g., `{context_str}` and `{query_str}`) when replacing a default question-answer prompt.

You can also define a template from chat messages:

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

# Create message prompt (for chat API)
messages = chat_template.format_messages(topic=...)

# Or convert to a text prompt (for completion API)
prompt = chat_template.format(topic=...)
```

### Accessing and Setting Custom Prompts

Since LlamaIndex is a multi-step process, it's important to identify the operation you want to modify and pass the custom prompt to the correct location.

For example, prompts are used in response synthesizers, retrievers, index construction, etc.; some of these modules are nested within other modules (e.g., a synthesizer is nested within a query engine).

### Commonly Used Prompts

The most commonly used prompts are `text_qa_template` and `refine_template`.

-   `text_qa_template` -- used to get the initial answer to a query using retrieved nodes.
-   `refine_template` -- used when the retrieved text does not fit into a single LLM call with `response_mode="compact"` (default), or when more than one node is retrieved with `response_mode="refine"`. The answer from the first query is inserted as `existing_answer`, and the LLM must update or iterate on the existing answer based on new context.

### Accessing Prompts

You can call `get_prompts` on many modules within LlamaIndex to get a flat list of prompts used within that module and any nested sub-modules.

For example, consider the following code.

```python
query_engine = index.as_query_engine(response_mode="compact")
prompts_dict = query_engine.get_prompts()
print(list(prompts_dict.keys()))
```

You might get back the following keys:

```python
['response_synthesizer:text_qa_template', 'response_synthesizer:refine_template']
```

Note that the prompts are prefixed by their sub-modules as "namespaces."

### Updating Prompts

You can customize prompts on any module that implements `get_prompts` using the `update_prompts` function. Simply pass in argument values with keys matching the keys you see in the prompt dictionary obtained through `get_prompts`.

For example, concerning the above example, we could do:

```python
# Shakespeare style!
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

### Modifying Prompts Used in the Query Engine

For query engines, you can also pass custom prompts directly at query time (i.e., to query the index and synthesize the final response).

There are also two equivalent ways to override prompts:

-   Via high-level API

```python
query_engine = index.as_query_engine(
    text_qa_template=custom_qa_prompt, refine_template=custom_refine_prompt)
```

-   Via low-level component API

```python
retriever = index.as_retriever()
synth = get_response_synthesizer(
    text_qa_template=custom_qa_prompt, refine_template=custom_refine_prompt)
query_engine = RetrieverQueryEngine(retriever, response_synthesizer)
```

The two approaches above are equivalent, where 1 is essentially syntactic sugar for 2 and hides the underlying complexity. You might want to use 1 to quickly modify some common parameters and use 2 for more fine-grained control.

For more details on which class uses which prompt, please visit the Query Class Reference.

Refer to the documentation for a complete set of all prompts.

### Modifying Prompts Used in Index Construction

Some indexes use different types of prompts during construction (NOTE: the most common indexes, `VectorStoreIndex` and `SummaryIndex`, do not use any prompts).

For example, `TreeIndex` uses a summary prompt to summarize nodes hierarchically, and `KeywordTableIndex` uses a keyword extraction prompt to extract keywords.

There are two equivalent ways to override prompts:

-   Via default node constructor

```python
index = TreeIndex(nodes, summary_template=custom_prompt)
```

-   Via document constructor

```python
index = TreeIndex.from_documents(docs, summary_template=custom_prompt)
```

For more details on which index uses which prompt, please visit the Index Class Reference.

### [Advanced] Advanced Prompt Capabilities

In this section, we present some advanced prompt capabilities in LlamaIndex.

**Related Guides:**

-   Advanced Prompts
-   Prompt Techniques for RAG

#### Partial Formatting

Partially format a prompt, filling in some variables while leaving others to be filled in later.

```python
from llama_index.core import PromptTemplate

prompt_tmpl_str = "{foo} {bar}"
prompt_tmpl = PromptTemplate(prompt_tmpl_str)
partial_prompt_tmpl = prompt_tmpl.partial_format(foo="abc")
fmt_str = partial_prompt_tmpl.format(bar="def")
```

#### Template Variable Mapping

LlamaIndex's prompt abstractions often expect certain keys. For example, our `text_qa_prompt` expects `context_str` for context and `query_str` for the user's query.

But if you're trying to adapt a string template for use with LlamaIndex, it may be tedious to change the template variables.

Instead, define `template_var_mappings`:

```python
template_var_mappings = {"context_str": "my_context", "query_str": "my_query"}
prompt_tmpl = PromptTemplate(
    qa_prompt_tmpl_str, template_var_mappings
```

Prompts are key to unlocking the power of LLMs in LlamaIndex. By understanding and customizing prompts, you can build powerful, flexible, and efficient search and retrieval applications.
