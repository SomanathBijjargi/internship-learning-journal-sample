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

# Vision Models

- Setting Up Vision Models: Integrate vision capabilities with LLMs using APIs like OpenAI’s Chat Completion.
- Sending Image URLs for Analysis: Pass URLs or base64-encoded images to LLMs for processing.
- Reading Image Responses: Get detailed textual descriptions of images, from scenic landscapes to specific objects like cricketers or bank statements.
- Extracting Data from Images: Convert extracted image data to various formats like Markdown tables or JSON arrays.
- Handling Model Hallucinations: Address inaccuracies in extraction results, understanding how different prompts can affect output quality.
- Cost Management for Vision Models: Adjust detail settings (e.g., “detail: low”) to balance cost and output precision.

```
curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "model": "gpt-4o-mini",
    "messages": [
      {
        "role": "user",
        "content": [
          {"type": "text", "text": "What is in this image?"},
          {
            "type": "image_url",
            "image_url": {
              "url": "https://upload.wikimedia.org/wikipedia/commons/3/34/Correlation_coefficient.png",
              "detail": "low"
            }
          }
        ]
      }
    ]
  }'
```
# Text Extraction :

- LLM for Data Extraction: Use OpenAI’s API to extract structured information from unstructured data like addresses.
- JSON Schema: Define a JSON schema to ensure consistent and structured output from the LLM.
- Prompt Engineering: Craft effective prompts to guide the LLM’s response and improve accuracy.
- Data Cleaning: Use string functions and OpenAI’s API to clean and standardize data.
- Data Analysis: Analyze extracted data using Pandas to gain insights.
- LLM Limitations: Understand the limitations of LLMs, including potential errors and inconsistencies in output.
- Production Use Cases: Explore real-world applications of LLMs for data extraction, such as customer service email analysis.

# Function Calling with OpenAI
Function Calling allows Large Language Models to convert natural language into structured function calls. This is perfect for building chatbots and AI assistants that need to interact with your backend systems.

## How to define functions
The function definition is a JSON schema with a few OpenAI specific properties. See the Supported schemas.
```
MEETING_TOOL = {
    "type": "function",
    "function": {
        "name": "schedule_meeting",
        "description": "Schedule a meeting room for a specific date and time",
        "parameters": {
            "type": "object",
            "properties": {
                "date": {"type": "string", "description": "Meeting date in YYYY-MM-DD format"},
                "time": {"type": "string", "description": "Meeting time in HH:MM format"},
                "meeting_room": {"type": "string", "description": "Name of the meeting room"},
            },
            "required": ["date", "time", "meeting_room"],
            "additionalProperties": False,
        },
        "strict": True,
    },
}
```
# LLM Evaluations with PromptFoo

PromptFoo is a test-driven development framework for LLMs:

- Developer-first: Fast CLI with live reload & caching 
- Multi-provider: Works with OpenAI, Anthropic, HuggingFace, Ollama & more 
- Assertions: Built‑in (contains, equals) & model‑graded (llm-rubric)
- CI/CD: Integrate evals into pipelines for regression safety 

To run PromptFoo:

- Install Node.js & npm (nodejs.org)
- Set up your OPENAI_API_KEY environment variable
- Configure promptfooconfig.yaml. Below is an example:
```
prompts:
  - |
      Summarize this text: "{{text}}"
  - |
      Please write a concise summary of: "{{text}}"

providers:
  - openai:gpt-3.5-turbo
  - openai:gpt-4

tests:
  - name: summary_test
    vars:
      text: "PromptFoo is an open-source CLI and library for evaluating and testing LLMs with assertions, caching, and matrices."
    assertions:
      - contains-all:
          values:
            - "open-source"
            - "LLMs"
      - llm-rubric:
          instruction: |
            Score the summary from 1 to 5 for:
            - relevance: captures the main info?
            - clarity: wording is clear and concise?
          schema:
            type: object
            properties:
              relevance:
                type: number
                minimum: 1
                maximum: 5
              clarity:
                type: number
                minimum: 1
                maximum: 5
            required: [relevance, clarity]
            additionalProperties: false

commandLineOptions:
  cache: true
```
Now, you can run the evaluations and see the results.
```
# Execute all tests
npx -y promptfoo eval -c promptfooconfig.yaml

# List past evaluations
npx -y promptfoo list evals

# Launch interactive results viewer on port 8080
npx -y promptfoo view -p 8080
```
PromptFoo caches API responses by default (TTL 14 days). You can disable it with --no-cache or clear it.
```
# Disable cache for this run
echo y | promptfoo eval --no-cache -c promptfooconfig.yaml

# Clear all cache
echo y | promptfoo cache clear