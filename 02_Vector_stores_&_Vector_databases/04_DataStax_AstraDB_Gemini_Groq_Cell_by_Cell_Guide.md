# DataStax Astra DB + Gemini + Groq — Cell-by-cell guide and source audit

## Deliverables
- `DataStax_AstraDB_Gemini_Groq_VSCode.ipynb`: use VS Code with a `uv` project and local `.env`.
- `DataStax_AstraDB_Gemini_Groq_Colab.ipynb`: use Colab Secrets and notebook installation cell.
- `.env.example`: blank local configuration template.
- `requirements_astra_gemini_groq.txt`: Python dependencies.

## Security warning
The uploaded instructor notebook contained credential-like strings for Astra DB and OpenAI. Those values are intentionally excluded from these revisions. If they were real, revoke/rotate them. Never commit `.env`, application tokens, or provider API keys.

## Source fidelity and modifications
The supplied DataStax notebook has 16 cells. Its transcript covers: the vector store versus database context; creating a Serverless Vector database; endpoint and token setup; selecting an embedding provider; creating `AstraDBVectorStore`; inserting ten sample documents; basic similarity search; metadata filtering; score inspection; score-threshold retrievers; and MMR retrievers.

The revised notebooks preserve all ten sample document texts and their `source` metadata categories, the instructor's search phrases, top-k behavior, metadata filtering, score retrieval, score-threshold retrieval, and MMR retrieval. OpenAI embeddings and embedded credentials are replaced with Gemini Embedding 2 and a custom LangChain `Embeddings` adapter. Groq is configured in its own section and used by a clearly-labelled optional LCEL RAG extension.

### Corrections and engineering choices
1. The store is created with `AstraDBVectorStore(embedding=...)` and a dedicated collection name for the chosen Gemini model and dimension.
2. `gemini-embedding-2` uses `title: none | text: ...` for documents and `task: search result | query: ...` for search queries, following Google's current guidance.
3. The output dimension is set to 768; changing the model/dimensions requires compatible vectors and collection configuration.
4. Stable document IDs make reruns replace the tutorial's corresponding records instead of adding unrelated IDs on every run.
5. Scores are printed as backend-provided numeric scores, not as percentage relevance; threshold values must be calibrated on actual results.
6. MMR is a relevance/diversity strategy, not hybrid search.
7. VS Code uses `uv` and `.env`; Colab uses Colab Secrets. Provider setup/test cells are separate: Gemini creates embeddings, Groq generates text, Astra DB stores/searches vectors. The final RAG chain is intentionally the integration point.
8. The source notebook's OpenAI cells are replaced rather than left executable alongside Gemini. No source credentials are copied.

## VS Code notebook cell index

| Cell | Type | First line / heading |
|---:|---|---|
| 0 | markdown | # DataStax Astra DB Vector Store with Gemini Embeddings and Groq |
| 1 | markdown | ## 1. Course map and prerequisites |
| 2 | markdown | ## 2. VS Code setup (Windows + `uv`) |
| 3 | markdown | ### 2.1 Load local environment variables |
| 4 | code | import os |
| 5 | markdown | ## 3. Shared LangChain and Astra imports |
| 6 | code | import os |
| 7 | markdown | ## 4. Astra DB configuration (VS Code) |
| 8 | code | ASTRA_DB_API_ENDPOINT = os.getenv("ASTRA_DB_API_ENDPOINT") |
| 9 | markdown | ## 5. Gemini embeddings — provider setup only |
| 10 | markdown | ### 5.1 Configure the Gemini API key |
| 11 | code | from google import genai |
| 12 | markdown | ### 5.2 Implement a LangChain-compatible Gemini adapter |
| 13 | code | class GeminiEmbeddings(Embeddings): |
| 14 | markdown | ### 5.3 Test Gemini embeddings before touching Astra DB |
| 15 | code | sample_query_vector = embeddings.embed_query("What is the weather forecast?") |
| 16 | markdown | ## 6. Connect LangChain to Astra DB |
| 17 | code | vector_store = AstraDBVectorStore( |
| 18 | markdown | ## 7. Create the instructor's ten sample documents |
| 19 | code | document_1 = Document( |
| 20 | markdown | ## 8. Insert documents into the remote vector database |
| 21 | code | document_ids = [f"astra_lesson_doc_{i}" for i in range(1, len(documents) + 1)] |
| 22 | markdown | ## 9. Basic semantic search |
| 23 | code | weather_results = vector_store.similarity_search("What is the weather", k=4) |
| 24 | markdown | ## 10. Search with metadata filtering |
| 25 | code | filtered_results = vector_store.similarity_search( |
| 26 | markdown | ## 11. Search with a score |
| 27 | code | scored_results = vector_store.similarity_search_with_score( |
| 28 | markdown | ## 12. Convert the vector store into a retriever |
| 29 | code | retriever = vector_store.as_retriever( |
| 30 | markdown | ## 13. Similarity score threshold retriever |
| 31 | code | threshold_retriever = vector_store.as_retriever( |
| 32 | markdown | ## 14. Maximum Marginal Relevance (MMR) retriever |
| 33 | code | mmr_retriever = vector_store.as_retriever( |
| 34 | markdown | ## 15. Groq — generation provider setup only |
| 35 | code | from langchain_groq import ChatGroq |
| 36 | markdown | ### 15.1 Test Groq independently |
| 37 | code | groq_smoke_test = groq_llm.invoke("Explain a vector database in one simple sentence.") |
| 38 | markdown | ## 16. Optional extension — connect Astra retrieval to Groq with LCEL |
| 39 | code | from langchain_core.output_parsers import StrOutputParser |
| 40 | markdown | ### 16.1 Run an end-to-end RAG question |
| 41 | code | question = "What does the sample dataset say about tomorrow's weather?" |
| 42 | markdown | ## 17. Troubleshooting and production cautions |
| 43 | markdown | ## 18. Self-check — explain before moving on |

## Colab notebook cell index

| Cell | Type | First line / heading |
|---:|---|---|
| 0 | markdown | # DataStax Astra DB Vector Store with Gemini Embeddings and Groq |
| 1 | markdown | ## 1. Course map and prerequisites |
| 2 | markdown | ## 2. Google Colab setup |
| 3 | markdown | ### 2.1 Install dependencies |
| 4 | code | %pip -q install -U langchain-core langchain-astradb google-genai langchain-groq |
| 5 | markdown | ### 2.2 Read Astra DB secrets |
| 6 | code | from google.colab import userdata |
| 7 | markdown | ## 3. Shared LangChain and Astra imports |
| 8 | code | import os |
| 9 | markdown | ## 4. Confirm Astra DB configuration |
| 10 | markdown | ## 5. Gemini embeddings — provider setup only |
| 11 | markdown | ### 5.1 Configure the Gemini API key |
| 12 | code | from google import genai |
| 13 | markdown | ### 5.2 Implement a LangChain-compatible Gemini adapter |
| 14 | code | class GeminiEmbeddings(Embeddings): |
| 15 | markdown | ### 5.3 Test Gemini embeddings before touching Astra DB |
| 16 | code | sample_query_vector = embeddings.embed_query("What is the weather forecast?") |
| 17 | markdown | ## 6. Connect LangChain to Astra DB |
| 18 | code | vector_store = AstraDBVectorStore( |
| 19 | markdown | ## 7. Create the instructor's ten sample documents |
| 20 | code | document_1 = Document( |
| 21 | markdown | ## 8. Insert documents into the remote vector database |
| 22 | code | document_ids = [f"astra_lesson_doc_{i}" for i in range(1, len(documents) + 1)] |
| 23 | markdown | ## 9. Basic semantic search |
| 24 | code | weather_results = vector_store.similarity_search("What is the weather", k=4) |
| 25 | markdown | ## 10. Search with metadata filtering |
| 26 | code | filtered_results = vector_store.similarity_search( |
| 27 | markdown | ## 11. Search with a score |
| 28 | code | scored_results = vector_store.similarity_search_with_score( |
| 29 | markdown | ## 12. Convert the vector store into a retriever |
| 30 | code | retriever = vector_store.as_retriever( |
| 31 | markdown | ## 13. Similarity score threshold retriever |
| 32 | code | threshold_retriever = vector_store.as_retriever( |
| 33 | markdown | ## 14. Maximum Marginal Relevance (MMR) retriever |
| 34 | code | mmr_retriever = vector_store.as_retriever( |
| 35 | markdown | ## 15. Groq — generation provider setup only |
| 36 | code | from langchain_groq import ChatGroq |
| 37 | markdown | ### 15.1 Test Groq independently |
| 38 | code | groq_smoke_test = groq_llm.invoke("Explain a vector database in one simple sentence.") |
| 39 | markdown | ## 16. Optional extension — connect Astra retrieval to Groq with LCEL |
| 40 | code | from langchain_core.output_parsers import StrOutputParser |
| 41 | markdown | ### 16.1 Run an end-to-end RAG question |
| 42 | code | question = "What does the sample dataset say about tomorrow's weather?" |
| 43 | markdown | ## 17. Troubleshooting and production cautions |
| 44 | markdown | ## 18. Self-check — explain before moving on |

## Setup commands

### VS Code / Windows CMD
```cmd
cd /d E:\RAG
uv add langchain-core langchain-astradb google-genai langchain-groq python-dotenv
uv add --dev ipykernel jupyter
uv run jupyter lab
```
Copy `.env.example` to `.env`, fill in your own values locally, and choose the project's Python kernel in VS Code. Do not use `uv run requirements.txt` as an install command.

### Google Colab
Run the dependency installation cell. Add Colab Secrets named `ASTRA_DB_API_ENDPOINT`, `ASTRA_DB_APPLICATION_TOKEN`, `GEMINI_API_KEY`, and `GROQ_API_KEY`, and grant notebook access. The notebook defaults to the Astra default namespace; edit `ASTRA_DB_NAMESPACE` if your database uses a custom namespace.

## Validation limits
The static check verifies notebook structure and Python syntax. It does not prove that live Gemini calls, Astra DB writes/queries, or Groq generation will succeed with your credentials. Run the notebook in your own configured environment; cloud database operations and provider calls may use quota or incur charges.

## Official references
- Astra DB LangChain integration: https://docs.datastax.com/en/astra-db-serverless/integrations/langchain.html
- `AstraDBVectorStore` reference: https://reference.langchain.com/python/langchain-astradb/vectorstores/AstraDBVectorStore
- Astra score search: https://reference.langchain.com/python/langchain-astradb/vectorstores/AstraDBVectorStore/similarity_search_with_score
- Gemini embeddings: https://ai.google.dev/gemini-api/docs/embeddings
- Groq model catalogue: https://console.groq.com/docs/models
