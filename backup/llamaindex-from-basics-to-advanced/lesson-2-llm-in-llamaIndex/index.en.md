---
categories: [llamaindex]
date: 2024-08-21T23:52:11+0700
draft: false
description: In this lesson, you'll explore the integration of Large Language Models (LLMs) within LlamaIndex. You'll learn how to leverage LLMs for tasks such as text completion and conversational AI, and how they can be used as standalone modules or integrated into other LlamaIndex components like indexing and querying. The lesson also covers important concepts such as tokenization and provides detailed examples and practical implementations, helping you harness the full potential of LLMs in your applications.
featuredImage: /ai/llamaindex-from-basics-to-advanced/lesson-2-llm-in-lamaIndex.webp
images: [/ai/llamaindex-from-basics-to-advanced/lesson-2-llm-in-lamaIndex.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [LlamaIndex]
title: Lesson 2 - LLM in LLamaIndex
weight: 2
---

**Large Language Models (LLMs)** play a core role in LlamaIndex, bringing high accuracy in natural language processing. Integrating LLMs into LlamaIndex helps you fully leverage the power of advanced language models such as OpenAI, Hugging Face, PaLM, and more. From text completion to conversational support, LLMs not only function independently but also integrate seamlessly with other modules in LlamaIndex to optimize performance and meet complex user demands.

## Concept

**Choosing the right Large Language Model (LLM) is one of the first steps you need to consider when building any LLM application on your data.**

LLMs are a core component of LlamaIndex. They can be used as standalone modules or plugged into other core LlamaIndex modules (indexes, retrievers, query engines). They are always used in the response synthesis step (e.g., after retrieval). Depending on the type of index being used, LLMs may also be utilized during index construction, insertion, and query traversal.

LlamaIndex provides a unified interface for defining LLM modules, whether from OpenAI, Hugging Face, or LangChain, so you don't have to write sample code to define the LLM interface. This interface includes the following features (detailed below):

-   Support for **text completion** and **conversation** endpoints (detailed below)

-   Support for **streaming** and **non-streaming** endpoints

-   Support for **synchronous** and **asynchronous** endpoints

### Model Usage

The following code shows how you can start using an LLM.

```bash
pip install llama-index-llms-openai
```

Then create the file `llama_demo.py`:

```python
from llama_index.llms.openai import OpenAI

resp = OpenAI().complete("Akitect.io is")
print(resp)
```

### Using LLMs as Standalone Modules

You can use LLM modules independently in LlamaIndex, as shown below.

#### **Example of Text Completion**

```python
from llama_index.llms.openai import OpenAI

# Non-streaming completion
completion = OpenAI().complete("akitect.io is")
print(completion)

# Streaming completion
llm = OpenAI()
completions = llm.stream_complete("akitect.io is")
for completion in completions:
    print(completion.delta, end="")
```

-   **Non-streaming completion**: You use `OpenAI()` to create an LLM object and call the `complete` method. This method takes a text string ("akitect.io is") and returns the completed text. This example does not require continuous connection to the model, and the result is returned immediately after processing.

-   **Streaming completion**: With `stream_complete`, you receive results in real-time as the model generates content. This is useful in scenarios where quick feedback is needed, such as in interactive applications. The data is printed continuously as each part of the completion is sent from the model.

#### **Example of Conversation**

```python
from llama_index.core.llms import ChatMessage
from llama_index.llms.openai import OpenAI

messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]
resp = OpenAI().chat(messages)
print(resp)
```

-   **Conversation**: In this example, you use the LLM module to create a conversation. `ChatMessage` is used to set the context for the model, such as saying that the model is a "pirate with a colorful personality." You can then send a user question ("What is your name") to receive a response from the model.

-   The `chat` method takes a list of messages and returns the model's response. This simulates how LLMs can maintain context across multiple interactions, making it very useful in chatbot or virtual assistant applications.

### Note on Tokenization

By default, LlamaIndex uses a global tokenizer for all token counting. The default is `cl100k` from tiktoken, which is the tokenizer to match the default LLM `gpt-3.5-turbo`.

If you change the LLM, you may need to update this tokenizer to ensure accurate token counting, segmentation, and prompting.

The only requirement for a tokenizer is that it must be a callable function that takes a string and returns a list.

You can set a global tokenizer as follows:

```python
from llama_index.core import Settings

# tiktoken
import tiktoken
Settings.tokenizer = tiktoken.encoding_for_model("gpt-3.5-turbo").encode

# huggingface
from transformers import AutoTokenizer
Settings.tokenizer = AutoTokenizer.from_pretrained("HuggingFaceH4/zephyr-7b-beta")
```

-   **Tokenizer**: A tokenizer is a tool that breaks down text into smaller units called tokens. These tokens can be words, characters, or subword units.
-   **cl100k**: This is a specific tokenizer used by OpenAI for models like `gpt-3.5-turbo`.
-   **Changing the tokenizer**: If you use a different LLM, you may need to change the tokenizer to ensure accurate text processing.
-   **Tokenizer requirements**: A tokenizer must be a callable function that takes a string as input and returns a list of tokens.
-   **Setting a global tokenizer**: You can use `Settings.tokenizer` to set a global tokenizer for LlamaIndex. The examples provided show how to use tokenizers from tiktoken or HuggingFace.

### Available LLM Integrations

Below is a table listing available LLM integrations along with links to their corresponding documentation:

| **LLM**                           | **Documentation Link**                                                                        |
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
