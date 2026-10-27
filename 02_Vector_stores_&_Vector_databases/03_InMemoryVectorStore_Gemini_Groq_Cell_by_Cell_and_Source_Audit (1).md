# In-Memory Vector Store — Gemini + Groq: Cell-by-Cell Guide and Source Audit

This guide accompanies `InMemoryVectorStore_Gemini_Groq_Complete_Clean.ipynb`. It is based on the uploaded `3-Othervectorstores.ipynb` and the instructor transcript supplied in the conversation. It distinguishes source coverage from additional explanatory/implementation cells.

## Validation summary

- Notebook cells: **65** (Markdown: 37; code: 28).
- Python syntax parsing: **passed for all 28 code cells**.
- Every code cell has explanatory Markdown immediately before it.
- Exact duplicate code-cell bodies: **none found**.
- Live Gemini/Groq API calls: **not executed during static validation**.

## Every notebook cell

- **Cell 0 — Markdown: Title.** Introduces `In-Memory Vector Store with Gemini Embeddings and Groq` and explains purpose/context before the code that follows.

- **Cell 1 — Markdown: Learning objectives and source boundary.** Introduces `What you should be able to explain after this notebook` and explains purpose/context before the code that follows.

- **Cell 2 — Markdown: Install dependencies.** Introduces `1. Environment setup` and explains purpose/context before the code that follows.

- **Cell 3 — Markdown: Shared imports.** Introduces `2. Shared imports and environment loading` and explains purpose/context before the code that follows.

- **Cell 4 — Code: Shared imports and environment loading.** `import os`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 5 — Markdown: In-memory definitions.** Introduces `3. What is `InMemoryVectorStore`?` and explains purpose/context before the code that follows.

- **Cell 6 — Markdown: Provider 1: Gemini intro.** Introduces `4. Provider A — Gemini embeddings (retrieval only)` and explains purpose/context before the code that follows.

- **Cell 7 — Code: Gemini client configuration.** `from google import genai`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 8 — Markdown: Gemini smoke test explanation.** Introduces `4.1 Test Gemini independently` and explains purpose/context before the code that follows.

- **Cell 9 — Code: Gemini direct API smoke test.** `sample_embedding_response = gemini_client.models.embed_content(`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 10 — Markdown: Gemini adapter explanation.** Introduces `4.2 Adapt Gemini to LangChain's `Embeddings` interface` and explains purpose/context before the code that follows.

- **Cell 11 — Code: GeminiEmbeddings adapter implementation.** `class GeminiEmbeddings(Embeddings):`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 12 — Markdown: Test adapter explanation.** Introduces `4.3 Validate the adapter` and explains purpose/context before the code that follows.

- **Cell 13 — Code: Gemini adapter query test.** `adapter_test_vector = gemini_embeddings.embed_query("How is tomorrow's weather expected to be?")`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 14 — Markdown: Source dataset explanation.** Introduces `5. Create the instructor's ten sample documents` and explains purpose/context before the code that follows.

- **Cell 15 — Code: Create all ten original Documents.** `document_1 = Document(`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 16 — Markdown: Inspect documents.** Introduces `5.1 Inspect the data before indexing` and explains purpose/context before the code that follows.

- **Cell 17 — Code: Inspect sample Documents.** `for index, document in enumerate(documents[:3], start=1):`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 18 — Markdown: Initialize vector store.** Introduces `6. Initialize the in-memory vector store` and explains purpose/context before the code that follows.

- **Cell 19 — Code: Create InMemoryVectorStore.** `vector_store = InMemoryVectorStore(embedding=gemini_embeddings)`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 20 — Markdown: Index documents.** Introduces `6.1 Add the documents` and explains purpose/context before the code that follows.

- **Cell 21 — Code: Add documents with stable IDs.** `document_ids = [f"sample-doc-{index:02d}" for index in range(1, len(documents) + 1)]`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 22 — Markdown: Basic similarity search.** Introduces `7. Semantic similarity search` and explains purpose/context before the code that follows.

- **Cell 23 — Code: Instructor weather search, default k.** `weather_query = "hows the weather forecast"`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 24 — Markdown: Top-k explanation.** Introduces `7.1 Control the number of results with `k`` and explains purpose/context before the code that follows.

- **Cell 25 — Code: Weather search with k=2.** `top_two_results = vector_store.similarity_search(weather_query, k=2)`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 26 — Markdown: Search by vector explanation.** Introduces `7.2 Search using a query vector directly` and explains purpose/context before the code that follows.

- **Cell 27 — Code: Query vector and similarity_search_by_vector.** `query_vector = gemini_embeddings.embed_query(weather_query)`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 28 — Markdown: Score explanation.** Introduces `7.3 Retrieve documents with similarity scores` and explains purpose/context before the code that follows.

- **Cell 29 — Code: Similarity search with scores.** `scored_results = vector_store.similarity_search_with_score(weather_query, k=3)`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 30 — Markdown: Metadata filter explanation.** Introduces `7.4 Filter the search space using metadata` and explains purpose/context before the code that follows.

- **Cell 31 — Code: Filter search to news-source documents.** `news_results = vector_store.similarity_search(`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 32 — Markdown: Retriever definition.** Introduces `8. Turn the vector store into a retriever` and explains purpose/context before the code that follows.

- **Cell 33 — Code: Create retriever with default settings.** `retriever = vector_store.as_retriever()`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 34 — Markdown: Retriever invoke default.** Introduces `8.1 Invoke the retriever` and explains purpose/context before the code that follows.

- **Cell 35 — Code: Invoke retriever with the instructor query.** `default_retrieved_docs = retriever.invoke(weather_query)`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 36 — Markdown: Retriever k=2.** Introduces `8.2 Set `k=2` with `search_kwargs`` and explains purpose/context before the code that follows.

- **Cell 37 — Code: Create and invoke top-two retriever.** `top_two_retriever = vector_store.as_retriever(search_kwargs={"k": 2})`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 38 — Markdown: Filtered retriever.** Introduces `8.3 Apply a metadata filter through the retriever` and explains purpose/context before the code that follows.

- **Cell 39 — Code: Create and invoke a news-only retriever.** `news_retriever = vector_store.as_retriever(`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 40 — Markdown: Groq provider section.** Introduces `9. Provider B — Groq chat model (answer generation only)` and explains purpose/context before the code that follows.

- **Cell 41 — Code: Groq chat model configuration.** `from langchain_groq import ChatGroq`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 42 — Markdown: Groq smoke test explanation.** Introduces `9.1 Test Groq independently` and explains purpose/context before the code that follows.

- **Cell 43 — Code: Groq direct generation smoke test.** `groq_test_response = groq_llm.invoke(`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 44 — Markdown: RAG architecture.** Introduces `10. Connect retrieval to generation: a basic RAG pipeline` and explains purpose/context before the code that follows.

- **Cell 45 — Markdown: Prompt template explanation.** Introduces `10.1 Create the answer prompt` and explains purpose/context before the code that follows.

- **Cell 46 — Code: Define the RAG prompt.** `rag_prompt = ChatPromptTemplate.from_messages(`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 47 — Markdown: Document formatter explanation.** Introduces `10.2 Format retrieved documents into context` and explains purpose/context before the code that follows.

- **Cell 48 — Code: Define format_docs.** `def format_docs(docs: list[Document]) -> str:`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 49 — Markdown: LCEL composition explanation.** Introduces `10.3 Build the LCEL chain` and explains purpose/context before the code that follows.

- **Cell 50 — Code: Compose the RAG LCEL chain.** `rag_chain = (`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 51 — Markdown: RAG invoke explanation.** Introduces `10.4 Ask a question through RAG` and explains purpose/context before the code that follows.

- **Cell 52 — Code: Invoke the RAG chain.** `rag_question = "How is the weather forecast for tomorrow?"`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 53 — Markdown: Inspect source context.** Introduces `10.5 Inspect the evidence separately` and explains purpose/context before the code that follows.

- **Cell 54 — Code: Inspect retrieved context for the RAG question.** `rag_evidence = retriever.invoke(rag_question)`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 55 — Markdown: Alternative helper chain explanation.** Introduces `11. Optional alternative: helper-based retrieval chain` and explains purpose/context before the code that follows.

- **Cell 56 — Markdown: Helper chain imports.** Introduces `11.1 Import the optional helpers` and explains purpose/context before the code that follows.

- **Cell 57 — Code: Import optional helper chain functions.** `from langchain_classic.chains.combine_documents import create_stuff_documents_chain`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 58 — Markdown: Helper prompt.** Introduces `11.2 Create the helper-compatible prompt and chains` and explains purpose/context before the code that follows.

- **Cell 59 — Code: Build the optional helper-based RAG chain.** `helper_prompt = ChatPromptTemplate.from_messages(`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 60 — Markdown: Invoke helper chain.** Introduces `11.3 Invoke the helper-based alternative` and explains purpose/context before the code that follows.

- **Cell 61 — Code: Invoke optional helper chain.** `helper_result = helper_rag_chain.invoke({"input": "How is the weather forecast for tomorrow?"})`. See the immediately preceding Markdown cell for why this code is used and what the output means.

- **Cell 62 — Markdown: Tradeoffs.** Introduces `12. When to use InMemoryVectorStore vs. other stores` and explains purpose/context before the code that follows.

- **Cell 63 — Markdown: Source mapping.** Introduces `13. Source coverage and changes from the instructor notebook` and explains purpose/context before the code that follows.

- **Cell 64 — Markdown: Interview recap.** Introduces `14. Interview-ready recap` and explains purpose/context before the code that follows.


## Instructor source coverage crosswalk

| Instructor source cell | Original content | Revised notebook mapping |
|---:|---|---|
| 0 | Definition of InMemoryVectorStore, dictionary-backed storage, cosine similarity | Cell 5 and Section 12: definition, mechanics, trade-offs |
| 1 | OpenAI Chat model initialized with gpt-4o-mini | Replaced by separate Groq configuration/test cells 41–43; no OpenAI API key dependency |
| 2 | OpenAIEmbeddings + InMemoryVectorStore construction | Gemini adapter/test cells 6–13 plus vector store initialization cell 19 |
| 3 | Ten original `Document` objects and source metadata | Cell 15; all ten original `page_content` strings and source labels preserved |
| 4 | Display the list of documents | Cell 17; readable data inspection instead of implicit notebook repr only |
| 5 | Add documents to the vector store | Cell 21; preserved with stable explicit IDs |
| 6 | Default `similarity_search` for weather query | Cell 23; preserved exact query wording |
| 7 | `similarity_search(..., k=2)` | Cell 25; preserved |
| 8 | `as_retriever(search_kwargs={"k": 2})` | Cells 33–39; default, k=2, and filter examples |
| 9 | `retriever.invoke` weather query | Cells 35–39; direct invoke, k=2 and metadata-filtered retriever |
| 10 | Empty cell | Removed as non-content |

## Transcript topics and how they are covered

- **In-memory vector store uses a dictionary and NumPy cosine similarity:** Cell 5 and Section 12 explain the implementation and limitations; Section 7 demonstrates score interpretation.

- **Ten sample documents in a list with source metadata:** All ten strings and source metadata are preserved in Cell 15.

- **Add documents and inspect returned IDs:** Cell 21 uses explicit IDs and reports the return values.

- **Search, top-k, and filtering:** Cells 23–31 demonstrate default search, k=2, vector search, score search and news-only filtering.

- **Convert to a retriever and invoke it:** Cells 33–39 cover `as_retriever`, `invoke`, `search_kwargs`, k=2 and filter.

- **Use LCEL or helper-based RAG:** Cells 50–54 build an explicit LCEL RAG pipeline; Cells 57–61 provide an optional helper-based alternative.

- **Use separate Gemini and Groq setups:** Gemini imports/config/client/adapter are separate from Groq import/config/client test; only the labelled RAG composition/query cells integrate retrieval and generation.


## Important technical choices

- **Provider naming:** the source uses `ChatGroq` and `GROQ_API_KEY`; that means Groq. Grok is xAI’s distinct service.

- **Embedding model:** `gemini-embedding-2` is the model ID in Google’s current embedding documentation. Query and document task formatting are different by design.

- **Multiple text inputs:** the didactic adapter performs one Gemini embed request per input text so it always receives a separate vector per document. This is clear and predictable, but not a throughput-optimized batching strategy.

- **Score semantics:** for this in-memory implementation, `similarity_search_with_score` returns cosine similarity, where higher indicates more similar. Do not transfer that interpretation to FAISS distance scores without checking the metric.
- **Relevance-score caveat:** the generic LangChain `similarity_search_with_relevance_scores()` API depends on a vector store-specific normalization function. The current in-memory implementation specifically documents raw cosine scores from `similarity_search_with_score`; do not assume normalized relevance scores are supported or calibrated without checking the installed version.

- **Filtering semantics:** this in-memory store accepts a callable predicate such as `lambda doc: doc.metadata.get("source") == "news"`. Other integrations may use dictionary filters.

- **Persistence:** populated memory is not automatically durable. This notebook keeps the main lesson focused on in-memory behavior and explains why Chroma/FAISS/hosted databases make different trade-offs.

- **API-key hygiene:** never print or commit keys; set them in `.env`.

- **Integrated pipeline:** an end-to-end RAG query must trigger retrieval and generation. Provider creation/test cells remain separate; the LCEL composition and RAG invocation are explicitly labelled integration points.


## Run instructions (Windows CMD)

From the project directory, install the main dependencies:

```cmd
uv add python-dotenv google-genai langchain-core langchain-groq numpy
uv add --dev jupyter ipykernel
```

Create `.env` with `GEMINI_API_KEY=...` and `GROQ_API_KEY=...`. Open Jupyter using the intended `uv` project environment and run cells top-to-bottom. The optional helper-based section additionally requires `uv add langchain-classic`. Do not use `uv run -r requirements.txt`; `uv run` expects a command.


## Official references

- [LangChain InMemoryVectorStore reference](https://reference.langchain.com/python/langchain-core/vectorstores/in_memory/InMemoryVectorStore)
- [LangChain vector-store guide](https://docs.langchain.com/oss/python/integrations/vectorstores)
- [Gemini embeddings](https://ai.google.dev/gemini-api/docs/embeddings)
- [Groq + LangChain](https://console.groq.com/docs/langchain)
- [Groq supported models](https://console.groq.com/docs/models)
- [create_stuff_documents_chain](https://reference.langchain.com/python/langchain-classic/chains/combine_documents/stuff/create_stuff_documents_chain)
- [create_retrieval_chain](https://reference.langchain.com/python/langchain-classic/chains/retrieval/create_retrieval_chain)
