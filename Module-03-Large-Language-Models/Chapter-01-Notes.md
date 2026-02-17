# Week 3 Session 1
# Large Language Model (LLM):

## Embeddings: OpenAI and Local Models#

Embedding models convert text into a list of numbers. These are like a map of text in numerical form. Each number represents a feature, and similar texts will have numbers close to each other. So, if the numbers are similar, the text they represent mean something similar.

## Multi-Model Embedding

Multimodal embeddings map text and images into the same vector space, enabling direct comparison between, say, a caption — “A cute cat” — and an image of that cat. This unified representation powers real-world applications 

# RAG(Retrieval-Augmented Generation)

## Vector databases:

Vector databases are specialized databases that store and search vector embeddings efficiently.

Use vector databases when your embeddings exceed available memory or when you want it run fast at scale. (This is important. If your code runs fast and fits in memory, you DON’T need a vector database. You can can use numpy for these tasks.)

## Retrieval Augmented Generation (RAG) with the CLI

RAG Combines retrieval (searching a knowledge base) with generation (using an LLM) to produce answers grounded in your own documents. Instead of relying solely on a general-purpose LLM, RAG lets you feed it the most relevant chunks from your corpus at query time, improving accuracy, reducing hallucinations, and allowing you to answer domain‑specific questions without fine‑tuning.

## Hybrid Retrieval Augmented Generation (Hybrid RAG) with TypeSense

Hybrid RAG combines semantic (vector) search with traditional keyword search to improve retrieval accuracy and relevance. By mixing exact text matches with embedding-based similarity, you get the best of both worlds: precision when keywords are present, and semantic recall when phrasing varies. TypeSense makes this easy with built-in hybrid search and automatic embedding generation.

# Base 64 Encoding

Base64 is a method to convert binary data into ASCII text. It’s essential when you need to transmit binary data through text-only channels or embed binary content in text formats.

```
import base64
text = "Hello, World!"
encoded = base64.b64encode(text.encode()).decode()
decoded = base64.b64decode(encoded).decode()
```