# RAG-based Question Routing System

This project implements a Retrieval-Augmented Generation (RAG) system with a routing mechanism that directs queries to either a vector store or Wikipedia search based on the relevance of the query. It uses LangChain, Hugging Face embeddings, Astra DB, and Groq API for query processing.

## Features
- **Vector Store Indexing**: Documents are indexed and stored in a Cassandra-based vector store.
- **Query Routing**: A router determines whether to use Wikipedia search or the vector store.
- **Hugging Face Embeddings**: Used for document embeddings and retrieval.
- **Graph-based Execution**: Utilizes LangGraph to model the workflow.
- **Arxiv & Wikipedia API**: Supports external information retrieval.

## Installation

```bash
pip install langchain langgraph cassio
pip install langchain_community tiktoken langchain-groq langchainhub chromadb langchain langgraph langchain_huggingface
pip install arxiv wikipedia
```

## Setup

1. **Configure Astra DB**  
   Add your Astra DB credentials in the script:

   ```python
   ASTRA_DB_APPLICATION_TOKEN="your_token_here"
   ASTRA_DB_ID="your_db_id_here"
   cassio.init(token=ASTRA_DB_APPLICATION_TOKEN, database_id=ASTRA_DB_ID)
   ```

2. **Set Up Groq API Key**  
   ```python
   import os
   from google.colab import userdata
   groq_api_key = userdata.get('Groq_API')
   os.environ["GROQ_API_KEY"] = groq_api_key
   ```

3. **Run the Pipeline**  
   The workflow consists of loading documents, indexing, and running queries.

   ```python
   inputs = {"question": "What is agent?"}
   for output in app.stream(inputs):
       print(output)
   ```

## Usage

- **Indexing Documents**  
  The script fetches and indexes documents from the web:

  ```python
  urls = [
      "https://lilianweng.github.io/posts/2023-06-23-agent/",
      "https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/",
      "https://lilianweng.github.io/posts/2023-10-25-adv-attack-llm/",
  ]
  ```

- **Query Execution**  
  Queries are processed based on the routing logic:

  ```python
  retriever.invoke("What is agent?", ConsistencyLevel="LOCAL_ONE")
  ```

## Future Enhancements
- Add support for multiple vector stores.
- Improve response ranking.
- Integrate additional knowledge sources.

## License
This project is open-source and available under the MIT License.
