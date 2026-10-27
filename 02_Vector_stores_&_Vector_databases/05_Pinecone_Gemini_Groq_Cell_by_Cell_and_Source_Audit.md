# Pinecone + Gemini + Groq — Cell-by-Cell Guide and Instructor Source Audit

## Deliverables

- `Pinecone_Gemini_Groq_VSCode.ipynb`: local Jupyter/VS Code with `uv` and `.env`.
- `Pinecone_Gemini_Groq_Colab.ipynb`: hosted Colab with three Colab Secrets.
- `pinecone.env.example`: blank local environment template (copy/rename it to `.env`).
- `requirements_pinecone_gemini_groq.txt`: dependency list.

## Provider boundary

- **Gemini**: embeddings only (`gemini-embedding-2`, explicit 768-dimensional output).
- **Pinecone**: managed vector index, metadata storage, similarity search.
- **Groq**: text generation only (`openai/gpt-oss-120b` via Groq API).
- The setup/smoke-test cells remain separate. The final RAG chain integrates the already-created retriever and Groq model by design.

## Original instructor notebook coverage

The uploaded source notebook contained 15 cells (including one empty cell). The following table records how each instructor cell was handled.

| Original cell | Instructor content | Treatment in revised notebooks |
|---:|---|---|
| 0 | Title only | Expanded to a project introduction and learning objectives. |
| 1 | Pinecone API-key placeholder | Replaced with secure environment/Colab Secrets instructions; no key is embedded. |
| 2 | Install LangChain, Pinecone, and OpenAI integration | Separated by environment; uses langchain-pinecone, langchain-groq, google-genai, and langchain-core. OpenAI embedding dependency removed. |
| 3 | Initialize Pinecone client | Preserved in Pinecone-only setup cell for both environments. |
| 4 | OpenAI text-embedding-3-small with dimension 1024 | Provider replaced with Gemini Embedding 2 at 768 dimensions; index dimension now matches actual Gemini output. |
| 5 | Create/check index and connect index client | Preserved index creation and client connection; uses a new dedicated index name and validates dimension/metric. |
| 6 | Display index object | Replaced with explicit index config/statistics verification, more useful for learning and debugging. |
| 7 | Create PineconeVectorStore | Preserved using GeminiEmbeddings and text_key="text". |
| 8 | Display vector store object | Replaced with an explanatory ready message; actual operations are demonstrated below. |
| 9 | Define ten Documents with metadata | Preserved all ten page_content values and source metadata. |
| 10 | Add documents | Preserved with stable demo IDs for repeatable reruns. |
| 11 | Similarity search, k=2, tweet filter | Preserved exactly in intent and query/metadata filter. |
| 12 | Similarity search with score, news filter | Preserved and adds explanation that score is not a probability. |
| 13 | Threshold retriever, k=1, threshold=0.4, news filter at invoke | Preserved with the same threshold example and added tuning caveat. |
| 14 | Empty cell | Removed as empty; no executable content was lost. |


## Instructor transcript flow preserved

1. Find/sign in to Pinecone and create an index.
2. Obtain the Pinecone API key.
3. Install the LangChain Pinecone integration.
4. Initialize the Pinecone client.
5. Configure an embedding model and keep its vector dimension aligned with the index.
6. Create/connect to the index and create a LangChain `PineconeVectorStore`.
7. Build the same ten sample `Document` objects and preserve their `source` metadata.
8. Insert the records into Pinecone.
9. Run direct similarity search (`k=2`, `source=tweet`).
10. Run similarity search with a score (`source=news`).
11. Convert the store into a threshold retriever (`k=1`, threshold 0.4) and use a news metadata filter.
12. Check stored records through index statistics and the Pinecone console.

Provider/model deviations are intentional: OpenAI embeddings are removed because the requested setup is Gemini + Groq. The instructor's 1024-dimensional index is not copied because Gemini Embedding 2 is explicitly configured for 768-dimensional outputs here. This prevents an index/model dimension mismatch.

## Cell-by-cell map of the revised notebooks

The tables below enumerate **every actual cell index** in both files. The first paragraph of a Markdown cell tells you the concept and rationale; the corresponding code cell performs the operation. Colab is one cell longer because it has a separate dependency-install cell and uses Secrets rather than `.env`.

### VS Code notebook — `Pinecone_Gemini_Groq_VSCode.ipynb`

| Cell index | Type | Heading / first code line | Purpose |
|---:|---|---|---|
| 0 | markdown | Pinecone Vector Database with Gemini Embeddings and Groq LLM | **Environment:** VS Code + local Python / uv |
| 1 | markdown | 1. Prepare the VS Code environment | Open **Command Prompt (CMD)** in your project directory. If the project already has a `pyproject.toml`, add these dependencies rather than running `uv init` again: |
| 2 | markdown | 2. Import shared components | These imports are provider-neutral LangChain pieces. Keeping them separate from provider setup makes it easier to see which code belongs to document data, the vector-store interface, or the RAG chain. |
| 3 | code | Code: import os | import os; from pathlib import Path |
| 4 | markdown | 3. Load local environment variables | Create a `.env` file in the project root using the accompanying `.env.example` template. `load_dotenv` loads values into `os.environ`; this cell does not print them. Add `.env` to `.gitignore` so secrets are not committed. |
| 5 | code | Code: loaded_env = load_dotenv(find_dotenv(usecwd=True)) | loaded_env = load_dotenv(find_dotenv(usecwd=True)); print(".env loaded:", bool(loaded_env)) |
| 6 | markdown | Expected `.env` structure | ```dotenv PINECONE_API_KEY=your_pinecone_key_here GEMINI_API_KEY=your_gemini_key_here GROQ_API_KEY=your_groq_key_here ``` |
| 7 | markdown | 4. Define the index and embedding configuration | Pinecone's dense-vector field must be created with exactly the same dimension as the vectors returned by the embedding model. We explicitly request **768 dimensions** from Gemini Embedding 2 and create a cosine index with 768 dimensions. |
| 8 | code | Code: EMBEDDING_MODEL = "gemini-embedding-2" | EMBEDDING_MODEL = "gemini-embedding-2"; EMBEDDING_DIMENSION = 768 |
| 9 | markdown | 5. Pinecone: authenticate and create the client | **Pinecone setup cell — no Gemini or Groq calls here.** The Pinecone SDK client manages the cloud index. The index itself stores vectors and metadata; the `PineconeVectorStore` wrapper that we create later gives it LangChain's standard s... |
| 10 | code | Code: PINECONE_API_KEY = os.getenv("PINECONE_API_KEY") | PINECONE_API_KEY = os.getenv("PINECONE_API_KEY"); if not PINECONE_API_KEY: |
| 11 | markdown | 6. Create or connect to the Pinecone index | An **index** is the named searchable vector resource in Pinecone. The index configuration includes the vector dimension, similarity metric, cloud, and region. For this lesson we use AWS `us-east-1` and cosine similarity. |
| 12 | code | Code: if not pc.has_index(INDEX_NAME): | if not pc.has_index(INDEX_NAME):; pc.create_index( |
| 13 | markdown | Check the index configuration | Inspecting the index helps confirm that the cloud resource matches the intended schema. If this cell fails because the SDK exposes a different description shape in your installed version, inspect `index_description` and compare the serve... |
| 14 | code | Code: print(index_description) | print(index_description) |
| 15 | markdown | 7. Gemini: configure the embedding provider | **Gemini-only section.** Embeddings convert texts to dense numerical vectors. Pinecone will index and compare those vectors, but it does not require Groq to create them. |
| 16 | code | Code: GEMINI_API_KEY = os.getenv("GEMINI_API_KEY") | GEMINI_API_KEY = os.getenv("GEMINI_API_KEY"); if not GEMINI_API_KEY: |
| 17 | markdown | Implement the LangChain `Embeddings` interface | LangChain vector stores expect an embedding object with two methods: - `embed_documents(texts)`: create vectors for stored passages. - `embed_query(text)`: create a vector for the user's search query. |
| 18 | code | Code: class GeminiEmbeddings(Embeddings): | class GeminiEmbeddings(Embeddings):; """Gemini Embedding 2 adapter compatible with LangChain vector stores.""" |
| 19 | markdown | Smoke-test Gemini embeddings | A smoke test verifies that credentials, model access, and the embedding response are working before we embed all ten documents. It is not a retrieval-quality evaluation. This call may consume API quota. |
| 20 | code | Code: sample_vector = embeddings.embed_query("What does LangChain help developers buil | sample_vector = embeddings.embed_query("What does LangChain help developers build?"); print("Vector dimension:", len(sample_vector)) |
| 21 | markdown | 8. Connect the Gemini embedder to Pinecone through LangChain | The `PineconeVectorStore` adapter connects the Pinecone index to the embedding implementation. When you add documents it calls the embedding model and writes the vectors/text/metadata to Pinecone. When you search with raw text, it embeds... |
| 22 | code | Code: from langchain_pinecone import PineconeVectorStore | from langchain_pinecone import PineconeVectorStore; vector_store = PineconeVectorStore( |
| 23 | markdown | 9. Prepare the ten instructor documents | A LangChain `Document` has `page_content` plus `metadata`. We preserve the instructor's original ten examples and source labels (`tweet`, `news`, and `website`) because later queries filter on them. Metadata is structured information att... |
| 24 | code | Code: from langchain_core.documents import Document | from langchain_core.documents import Document; document_1 = Document( |
| 25 | markdown | 10. Insert the documents into Pinecone | `add_documents()` embeds the document text through our Gemini adapter and sends the resulting vectors and metadata to the remote Pinecone index. Stable IDs make rerunning this teaching notebook repeatable: the same IDs upsert the same lo... |
| 26 | code | Code: document_ids = [f"demo-document-{i:02d}" for i in range(1, len(documents) + 1)] | document_ids = [f"demo-document-{i:02d}" for i in range(1, len(documents) + 1)]; upserted_ids = vector_store.add_documents(documents=documents, ids=document_ids) |
| 27 | markdown | Verify the index record count | Use Pinecone's index statistics to inspect the vector count. The vector count is a storage check, not a proof that retrieval is relevant. Cloud index updates can take a short time to become visible to queries; if immediate results are mi... |
| 28 | code | Code: stats = index.describe_index_stats() | stats = index.describe_index_stats(); print(stats) |
| 29 | markdown | 11. Direct similarity search with a metadata filter | The instructor's first query uses `similarity_search(query, k=2, filter={"source": "tweet"})`. |
| 30 | code | Code: query = "LangChain provides abstractions to make working with LLMs easy" | query = "LangChain provides abstractions to make working with LLMs easy"; results = vector_store.similarity_search( |
| 31 | markdown | 12. Similarity search with a score | `similarity_search_with_score()` returns each document together with Pinecone's similarity score. This helps inspect relative ranking, but the score is **not** a probability that the answer is correct. With a cosine index, larger similar... |
| 32 | code | Code: score_results = vector_store.similarity_search_with_score( | score_results = vector_store.similarity_search_with_score(; "Will it be hot tomorrow?", |
| 33 | markdown | 13. Inspect the metadata filter by itself | This separate query makes the filter visible and easy to teach. The `source` field is metadata from the `Document`, so Pinecone does not need to infer whether a record is a tweet or a news item from the text. |
| 34 | code | Code: news_only = vector_store.similarity_search( | news_only = vector_store.similarity_search(; "What happened in the city bank?", |
| 35 | markdown | 14. Convert the vector store to a retriever | A **vector store** exposes storage and search operations. A **retriever** is the narrower interface used by chains: given a query, it returns relevant documents. |
| 36 | code | Code: retriever = vector_store.as_retriever( | retriever = vector_store.as_retriever(; search_type="similarity_score_threshold", |
| 37 | markdown | 15. Groq: configure the generation model (separate provider section) | **Groq-only section — no Gemini calls here.** Groq receives natural-language prompts and generates text. It does not create the embeddings or perform Pinecone search; those tasks have already been handled by the Gemini adapter and Pineco... |
| 38 | code | Code: GROQ_API_KEY = os.getenv("GROQ_API_KEY") | GROQ_API_KEY = os.getenv("GROQ_API_KEY"); if not GROQ_API_KEY: |
| 39 | markdown | Smoke-test Groq without RAG | Test the generation model by itself first. This isolates Groq authentication/model errors from Pinecone retrieval errors. The call may consume your Groq quota. |
| 40 | code | Code: groq_test = llm.invoke("In one sentence, explain what a vector database does.") | groq_test = llm.invoke("In one sentence, explain what a vector database does."); print(groq_test.content) |
| 41 | markdown | 16. Bonus: build an end-to-end RAG chain | This is an extension of the instructor's vector-store lesson. The chain has a retriever fetch context from Pinecone, formats the selected documents, inserts the context and question into a prompt, then asks Groq to generate a grounded an... |
| 42 | markdown | Define how retrieved documents become context | The retriever returns `Document` objects, not one text string. `format_docs()` turns those objects into readable source-labelled context. Keeping source labels helps users inspect evidence and debug retrieval. |
| 43 | code | Code: def format_docs(docs: Sequence[Document]) -> str: | def format_docs(docs: Sequence[Document]) -> str:; if not docs: |
| 44 | markdown | Create the answer prompt | `{context}` is populated with retrieved text; `{question}` is the user's question. The instruction tells the model to abstain when the context does not support an answer. This reduces unsupported guesses but does not guarantee the model ... |
| 45 | code | Code: rag_prompt = ChatPromptTemplate.from_messages([ | rag_prompt = ChatPromptTemplate.from_messages([; ( |
| 46 | markdown | Compose the LCEL chain | `RunnablePassthrough.assign()` keeps the original question and adds a `context` field computed by the Pinecone retriever. The prompt then feeds the Groq model, and `StrOutputParser` extracts the final text content. |
| 47 | code | Code: rag_retriever = vector_store.as_retriever(search_kwargs={"k": 3}) | rag_retriever = vector_store.as_retriever(search_kwargs={"k": 3}); rag_chain = ( |
| 48 | markdown | Run one question and inspect the evidence | Use a question whose answer is represented in the ten-document sample. The answer comes from Groq; the evidence supplied to it comes from Pinecone. Inspect retrieved documents separately if the answer looks wrong. |
| 49 | code | Code: question = "What does the sample knowledge base say about LangChain and LangGrap | question = "What does the sample knowledge base say about LangChain and LangGraph?"; answer = rag_chain.invoke({"question": question}) |
| 50 | markdown | 17. Troubleshooting and interview recap | - **Dimension mismatch:** Pinecone index dimension, embedding output dimensions, and any existing vectors must match. Use a fresh index if changing embedding model/dimension. - **Authentication:** confirm `PINECONE_API_KEY`, `GEMINI_API_... |


### Google Colab notebook — `Pinecone_Gemini_Groq_Colab.ipynb`

| Cell index | Type | Heading / first code line | Purpose |
|---:|---|---|---|
| 0 | markdown | Pinecone Vector Database with Gemini Embeddings and Groq LLM | **Environment:** Google Colab |
| 1 | markdown | 1. Install dependencies in Colab | Colab runs on a hosted Python runtime. Install the separate LangChain integrations used in this lesson: `langchain-pinecone` connects LangChain to Pinecone; `langchain-groq` connects to Groq; `google-genai` calls Gemini; and `langchain-c... |
| 2 | code | Code: %pip install -qU langchain-core langchain-pinecone langchain-groq google-genai | %pip install -qU langchain-core langchain-pinecone langchain-groq google-genai |
| 3 | markdown | 2. Import shared components | These imports are provider-neutral LangChain pieces. Keeping them separate from provider setup makes it easier to see which code belongs to document data, the vector-store interface, or the RAG chain. |
| 4 | code | Code: import os | import os; from typing import Sequence |
| 5 | markdown | 3. Configure secrets in Colab | In Colab, open the **Secrets** panel (key icon) and create these three secrets: `PINECONE_API_KEY`, `GEMINI_API_KEY`, and `GROQ_API_KEY`. Grant this notebook access to each secret. Never paste keys into notebook code, print them, or uplo... |
| 6 | markdown | 3A. Read Pinecone's secret only | Pinecone's API key authorizes creating/connecting to the index and inserting/querying vectors. It is not a Gemini or Groq key. |
| 7 | code | Code: from google.colab import userdata | from google.colab import userdata; PINECONE_API_KEY = userdata.get("PINECONE_API_KEY") |
| 8 | markdown | 4. Define the index and embedding configuration | Pinecone's dense-vector field must be created with exactly the same dimension as the vectors returned by the embedding model. We explicitly request **768 dimensions** from Gemini Embedding 2 and create a cosine index with 768 dimensions. |
| 9 | code | Code: EMBEDDING_MODEL = "gemini-embedding-2" | EMBEDDING_MODEL = "gemini-embedding-2"; EMBEDDING_DIMENSION = 768 |
| 10 | markdown | 5. Pinecone: authenticate and create the client | **Pinecone setup cell — no Gemini or Groq calls here.** The Pinecone SDK client manages the cloud index. The index itself stores vectors and metadata; the `PineconeVectorStore` wrapper that we create later gives it LangChain's standard s... |
| 11 | code | Code: from pinecone import Pinecone, ServerlessSpec | from pinecone import Pinecone, ServerlessSpec; pc = Pinecone(api_key=PINECONE_API_KEY) |
| 12 | markdown | 6. Create or connect to the Pinecone index | An **index** is the named searchable vector resource in Pinecone. The index configuration includes the vector dimension, similarity metric, cloud, and region. For this lesson we use AWS `us-east-1` and cosine similarity. |
| 13 | code | Code: if not pc.has_index(INDEX_NAME): | if not pc.has_index(INDEX_NAME):; pc.create_index( |
| 14 | markdown | Check the index configuration | Inspecting the index helps confirm that the cloud resource matches the intended schema. If this cell fails because the SDK exposes a different description shape in your installed version, inspect `index_description` and compare the serve... |
| 15 | code | Code: print(index_description) | print(index_description) |
| 16 | markdown | 7. Gemini: configure the embedding provider | **Gemini-only section.** Embeddings convert texts to dense numerical vectors. Pinecone will index and compare those vectors, but it does not require Groq to create them. |
| 17 | code | Code: GEMINI_API_KEY = userdata.get("GEMINI_API_KEY") | GEMINI_API_KEY = userdata.get("GEMINI_API_KEY"); if not GEMINI_API_KEY: |
| 18 | markdown | Implement the LangChain `Embeddings` interface | LangChain vector stores expect an embedding object with two methods: - `embed_documents(texts)`: create vectors for stored passages. - `embed_query(text)`: create a vector for the user's search query. |
| 19 | code | Code: class GeminiEmbeddings(Embeddings): | class GeminiEmbeddings(Embeddings):; """Gemini Embedding 2 adapter compatible with LangChain vector stores.""" |
| 20 | markdown | Smoke-test Gemini embeddings | A smoke test verifies that credentials, model access, and the embedding response are working before we embed all ten documents. It is not a retrieval-quality evaluation. This call may consume API quota. |
| 21 | code | Code: sample_vector = embeddings.embed_query("What does LangChain help developers buil | sample_vector = embeddings.embed_query("What does LangChain help developers build?"); print("Vector dimension:", len(sample_vector)) |
| 22 | markdown | 8. Connect the Gemini embedder to Pinecone through LangChain | The `PineconeVectorStore` adapter connects the Pinecone index to the embedding implementation. When you add documents it calls the embedding model and writes the vectors/text/metadata to Pinecone. When you search with raw text, it embeds... |
| 23 | code | Code: from langchain_pinecone import PineconeVectorStore | from langchain_pinecone import PineconeVectorStore; vector_store = PineconeVectorStore( |
| 24 | markdown | 9. Prepare the ten instructor documents | A LangChain `Document` has `page_content` plus `metadata`. We preserve the instructor's original ten examples and source labels (`tweet`, `news`, and `website`) because later queries filter on them. Metadata is structured information att... |
| 25 | code | Code: from langchain_core.documents import Document | from langchain_core.documents import Document; document_1 = Document( |
| 26 | markdown | 10. Insert the documents into Pinecone | `add_documents()` embeds the document text through our Gemini adapter and sends the resulting vectors and metadata to the remote Pinecone index. Stable IDs make rerunning this teaching notebook repeatable: the same IDs upsert the same lo... |
| 27 | code | Code: document_ids = [f"demo-document-{i:02d}" for i in range(1, len(documents) + 1)] | document_ids = [f"demo-document-{i:02d}" for i in range(1, len(documents) + 1)]; upserted_ids = vector_store.add_documents(documents=documents, ids=document_ids) |
| 28 | markdown | Verify the index record count | Use Pinecone's index statistics to inspect the vector count. The vector count is a storage check, not a proof that retrieval is relevant. Cloud index updates can take a short time to become visible to queries; if immediate results are mi... |
| 29 | code | Code: stats = index.describe_index_stats() | stats = index.describe_index_stats(); print(stats) |
| 30 | markdown | 11. Direct similarity search with a metadata filter | The instructor's first query uses `similarity_search(query, k=2, filter={"source": "tweet"})`. |
| 31 | code | Code: query = "LangChain provides abstractions to make working with LLMs easy" | query = "LangChain provides abstractions to make working with LLMs easy"; results = vector_store.similarity_search( |
| 32 | markdown | 12. Similarity search with a score | `similarity_search_with_score()` returns each document together with Pinecone's similarity score. This helps inspect relative ranking, but the score is **not** a probability that the answer is correct. With a cosine index, larger similar... |
| 33 | code | Code: score_results = vector_store.similarity_search_with_score( | score_results = vector_store.similarity_search_with_score(; "Will it be hot tomorrow?", |
| 34 | markdown | 13. Inspect the metadata filter by itself | This separate query makes the filter visible and easy to teach. The `source` field is metadata from the `Document`, so Pinecone does not need to infer whether a record is a tweet or a news item from the text. |
| 35 | code | Code: news_only = vector_store.similarity_search( | news_only = vector_store.similarity_search(; "What happened in the city bank?", |
| 36 | markdown | 14. Convert the vector store to a retriever | A **vector store** exposes storage and search operations. A **retriever** is the narrower interface used by chains: given a query, it returns relevant documents. |
| 37 | code | Code: retriever = vector_store.as_retriever( | retriever = vector_store.as_retriever(; search_type="similarity_score_threshold", |
| 38 | markdown | 15. Groq: configure the generation model (separate provider section) | **Groq-only section — no Gemini calls here.** Groq receives natural-language prompts and generates text. It does not create the embeddings or perform Pinecone search; those tasks have already been handled by the Gemini adapter and Pineco... |
| 39 | code | Code: GROQ_API_KEY = userdata.get("GROQ_API_KEY") | GROQ_API_KEY = userdata.get("GROQ_API_KEY"); if not GROQ_API_KEY: |
| 40 | markdown | Smoke-test Groq without RAG | Test the generation model by itself first. This isolates Groq authentication/model errors from Pinecone retrieval errors. The call may consume your Groq quota. |
| 41 | code | Code: groq_test = llm.invoke("In one sentence, explain what a vector database does.") | groq_test = llm.invoke("In one sentence, explain what a vector database does."); print(groq_test.content) |
| 42 | markdown | 16. Bonus: build an end-to-end RAG chain | This is an extension of the instructor's vector-store lesson. The chain has a retriever fetch context from Pinecone, formats the selected documents, inserts the context and question into a prompt, then asks Groq to generate a grounded an... |
| 43 | markdown | Define how retrieved documents become context | The retriever returns `Document` objects, not one text string. `format_docs()` turns those objects into readable source-labelled context. Keeping source labels helps users inspect evidence and debug retrieval. |
| 44 | code | Code: def format_docs(docs: Sequence[Document]) -> str: | def format_docs(docs: Sequence[Document]) -> str:; if not docs: |
| 45 | markdown | Create the answer prompt | `{context}` is populated with retrieved text; `{question}` is the user's question. The instruction tells the model to abstain when the context does not support an answer. This reduces unsupported guesses but does not guarantee the model ... |
| 46 | code | Code: rag_prompt = ChatPromptTemplate.from_messages([ | rag_prompt = ChatPromptTemplate.from_messages([; ( |
| 47 | markdown | Compose the LCEL chain | `RunnablePassthrough.assign()` keeps the original question and adds a `context` field computed by the Pinecone retriever. The prompt then feeds the Groq model, and `StrOutputParser` extracts the final text content. |
| 48 | code | Code: rag_retriever = vector_store.as_retriever(search_kwargs={"k": 3}) | rag_retriever = vector_store.as_retriever(search_kwargs={"k": 3}); rag_chain = ( |
| 49 | markdown | Run one question and inspect the evidence | Use a question whose answer is represented in the ten-document sample. The answer comes from Groq; the evidence supplied to it comes from Pinecone. Inspect retrieved documents separately if the answer looks wrong. |
| 50 | code | Code: question = "What does the sample knowledge base say about LangChain and LangGrap | question = "What does the sample knowledge base say about LangChain and LangGraph?"; answer = rag_chain.invoke({"question": question}) |
| 51 | markdown | 17. Troubleshooting and interview recap | - **Dimension mismatch:** Pinecone index dimension, embedding output dimensions, and any existing vectors must match. Use a fresh index if changing embedding model/dimension. - **Authentication:** confirm `PINECONE_API_KEY`, `GEMINI_API_... |

## Critical technical notes

- `EMBEDDING_DIMENSION = 768` is deliberately matched across Gemini response config and Pinecone index creation.
- If the existing index has a different dimension or metric, the notebook stops instead of writing incompatible vectors.
- The index name is `rag-gemini-groq-768` to avoid reusing an existing `rag` index created with a different embedding model.
- Stable IDs prevent duplicate logical records when the notebook is re-run.
- Pinecone similarity scores are not confidence probabilities. Validate thresholds on representative queries.
- Live API execution has not been performed by this file-generation process; run top-to-bottom in your configured environment.

## Official references checked

- LangChain Pinecone integration: https://docs.langchain.com/oss/python/integrations/vectorstores/pinecone
- Pinecone serverless indexes: https://sdk.pinecone.io/python/how-to/indexes/serverless.html
- Gemini embedding docs: https://ai.google.dev/gemini-api/docs/embeddings
- Groq supported model IDs: https://console.groq.com/docs/models
- LangChain ChatGroq integration: https://docs.langchain.com/oss/python/integrations/chat/groq
