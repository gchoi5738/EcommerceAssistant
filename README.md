# E-Commerce Assistant Chatbot

An intelligent e-commerce assistant that leverages **RAG (Retrieval-Augmented Generation)** to provide accurate product recommendations and order information.

---

## Overview

This chatbot serves as a smart e-commerce assistant capable of:

- Answering product-related queries using semantic search  
- Retrieving and presenting order information  
- Maintaining conversation context  
- Providing personalized recommendations  

---

## Setup and Installation

### Prerequisites

- **Python**: 3.8+  
- **Jupyter Notebook**  
- **FastAPI**  

Install required packages:

```bash
pip install fastapi uvicorn pandas numpy sentence-transformers scikit-learn requests openai langchain
```
Running the Application
Launch Jupyter Notebook:
```bash
jupyter notebook
```
Open chatbot.ipynb and run the cells in order.
Make sure to add your OpenAI api key to the appropriate cell.


Start the FastAPI server (in a separate terminal):
```bash
cd Mock_Api
python mock_api.py
```

# Implementation Details
## Model Selection Journey
Initially, I experimented with running Llama-3.2 3B locally for cost efficiency. However, I encountered several limitations:

Occasional hallucinations in responses
- Inconsistent output quality
- High resource requirements
I then tried using HuggingFace's Inference API with various open-source models but faced reliability issues with API availability and response times. Huggingface only allows "warm" models to be used in Inference API, but even then they could be unreliable(to be expected for a service that is free, we get 1000 free api calls per day).

Finally, I opted for GPT-4o-Mini through OpenAI's API because:

- Excellent price-performance ratio (approximately $0.15 per million tokens)
- Consistent, high-quality responses
- Reliable API service(though recently API services went down which was alarming)
## Structured outputs
The implementation uses ConversationBufferMemory for maintaining conversation context. This choice was made because:

- Use Case Alignment: E-commerce interactions are typically short-lived (e.g., checking orders, product inquiries).
- Simplicity: Direct storage of recent interactions is sufficient for our needs.
- Cost Efficiency: No need for expensive summarization operations.
- For more complex applications (e.g., long customer service conversations, complex troubleshooting), alternatives like ConversationSummaryBufferMemory or entity-relational techniques might be more appropriate.

## RAG Implementation
Product recommendations are powered by:

- Sentence-transformer embeddings (all-MiniLM-L6-v2)
- Cosine similarity for semantic search
- Maintains conversation history
- Remembers customer IDs
- Links products with order history
- Intelligent Query Processing
- Automatic query type detection
- Structured information extraction
- Context-aware responses

## Testing
A 10 query test suite is provided, covering:

- Basic product queries
- Order status checks
- Mixed context queries
- Follow-up questions
- Customer ID handling