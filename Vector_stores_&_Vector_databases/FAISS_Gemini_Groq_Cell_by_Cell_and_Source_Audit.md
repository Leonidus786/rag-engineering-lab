# FAISS + Gemini Embeddings + Groq RAG — Cell-by-Cell Guide and Source Audit

## Deliverables and status

- Notebook: `FAISS_Gemini_Groq_RAG_Instructor_Complete_Clean.ipynb`
- Notebook cells: 95
- Markdown cells: 53
- Code cells: 42
- No saved code outputs/execution counts are included.
- Code syntax is checked statically; live Gemini/Groq execution has not been performed because it requires user credentials and may consume quota.

## How to use the notebook

Read each Markdown cell and then run the code cell below it. Gemini is exclusively the embedding provider; Groq is exclusively the generation provider. FAISS is the local vector index. Do not run only isolated later cells in a fresh kernel because they depend on earlier variables.

## Actual cell index

The numbering below is zero-based Jupyter cell numbering. Each code cell has an explanatory Markdown cell immediately before it.

- **Cell 0 — Markdown:** Building a RAG System with LangChain + FAISS, Gemini Embeddings, and Groq Generation
- **Cell 1 — Markdown:** Learning outcomes and lesson map
- **Cell 2 — Markdown:** Source coverage: what was retained and what was improved
- **Cell 3 — Markdown:** 1. Prepare the Python environment
- **Cell 4 — Markdown:** 2. Configure credentials safely
- **Cell 5 — Code:** `from pathlib import Path`
- **Cell 6 — Markdown:** 3. Import shared LangChain, FAISS, and data utilities
- **Cell 7 — Code:** `import numpy as np`
- **Cell 8 — Markdown:** 4. Check the FAISS runtime
- **Cell 9 — Code:** `print("FAISS module imported:", faiss.__name__)`
- **Cell 10 — Markdown:** 5. Understand FAISS before coding
- **Cell 11 — Markdown:** 6. Create the instructor's sample documents
- **Cell 12 — Code:** `sample_documents = [`
- **Cell 13 — Markdown:** 7. Inspect the original `Document` objects
- **Cell 14 — Code:** `for i, doc in enumerate(sample_documents, start=1):`
- **Cell 15 — Markdown:** 8. Optional: understand text and PDF loaders
- **Cell 16 — Code:** `DATA_DIR = Path("data/faiss_gemini_groq")`
- **Cell 17 — Markdown:** 9. Split documents into chunks
- **Cell 18 — Code:** `text_splitter = RecursiveCharacterTextSplitter(`
- **Cell 19 — Markdown:** 10. Inspect chunk content and metadata
- **Cell 20 — Code:** `for i, chunk in enumerate(chunks, start=1):`
- **Cell 21 — Markdown:** 11. Gemini section: validate the embedding API key
- **Cell 22 — Markdown:** 11A. Initialize Gemini independently
- **Cell 23 — Code:** `from google import genai`
- **Cell 24 — Markdown:** 12. Generate one embedding directly with Gemini
- **Cell 25 — Code:** `sample_text = "What is machine learning?"`
- **Cell 26 — Markdown:** 13. Adapt Gemini to LangChain's `Embeddings` interface
- **Cell 27 — Code:** `DOCUMENT_PREFIX = "title: none | text: "`
- **Cell 28 — Markdown:** 14. Test the LangChain embedding adapter
- **Cell 29 — Code:** `adapter_query = "What is deep learning?"`
- **Cell 30 — Markdown:** 15. Create embeddings for a list of texts
- **Cell 31 — Code:** `texts_for_embedding = [`
- **Cell 32 — Markdown:** 16. Inspect example embeddings
- **Cell 33 — Code:** `for text, vector in zip(texts_for_embedding, batch_embeddings):`
- **Cell 34 — Markdown:** 17. Cosine similarity: definition and formula
- **Cell 35 — Code:** `def cosine_similarity(vec_a: list[float], vec_b: list[float]) -> float:`
- **Cell 36 — Markdown:** 18. Compare semantic similarity: related and unrelated examples
- **Cell 37 — Code:** `def compare_embeddings(text1: str, text2: str) -> float:`
- **Cell 38 — Markdown:** 19. Create the FAISS vector store
- **Cell 39 — Code:** `vectorstore = FAISS.from_documents(`
- **Cell 40 — Markdown:** 20. Inspect the built index
- **Cell 41 — Code:** `print("Index type:", type(vectorstore.index).__name__)`
- **Cell 42 — Markdown:** 21. Save the FAISS index for reuse
- **Cell 43 — Code:** `FAISS_INDEX_DIR = Path("faiss_index_gemini_groq")`
- **Cell 44 — Markdown:** 22. Load the saved index — important pickle security warning
- **Cell 45 — Code:** `loaded_vectorstore = FAISS.load_local(`
- **Cell 46 — Markdown:** 23. Verify the loaded index
- **Cell 47 — Code:** `assert loaded_vectorstore.index.ntotal == vectorstore.index.ntotal`
- **Cell 48 — Markdown:** 24. Run basic top-k similarity search
- **Cell 49 — Code:** `query = "What is deep learning?"`
- **Cell 50 — Markdown:** 25. Similarity search with scores
- **Cell 51 — Code:** `results_with_scores = loaded_vectorstore.similarity_search_with_score(query, k=3)`
- **Cell 52 — Markdown:** 26. Filter search by metadata
- **Cell 53 — Code:** `filter_dict = {"topic": "ML"}`
- **Cell 54 — Markdown:** 27. Convert the vector store into a retriever
- **Cell 55 — Code:** `retriever = loaded_vectorstore.as_retriever(`
- **Cell 56 — Markdown:** 28. Groq section: initialize the answer-generation model separately
- **Cell 57 — Markdown:** 28A. Validate the Groq key and initialize Groq
- **Cell 58 — Code:** `from langchain_groq import ChatGroq`
- **Cell 59 — Markdown:** 29. Test Groq generation without RAG
- **Cell 60 — Code:** `groq_test_response = groq_llm.invoke(`
- **Cell 61 — Markdown:** 30. Create the basic RAG prompt
- **Cell 62 — Code:** `simple_prompt = ChatPromptTemplate.from_template(`
- **Cell 63 — Markdown:** 31. Format retrieved `Document` objects for the prompt
- **Cell 64 — Code:** `def format_docs(docs: list[Document]) -> str:`
- **Cell 65 — Markdown:** 32. Build the simple LCEL RAG chain
- **Cell 66 — Code:** `simple_rag_chain = (`
- **Cell 67 — Markdown:** 33. Run a first simple RAG question
- **Cell 68 — Code:** `first_rag_question = "What is the difference between AI and machine learning?"`
- **Cell 69 — Markdown:** 34. Inspect the context retrieved for that question
- **Cell 70 — Code:** `evidence_docs = retriever.invoke(first_rag_question)`
- **Cell 71 — Markdown:** 35. Build a streaming RAG chain
- **Cell 72 — Code:** `streaming_rag_chain = (`
- **Cell 73 — Markdown:** 36. Stream the generated answer token/chunk by chunk
- **Cell 74 — Code:** `stream_question = "Explain deep learning in simple terms."`
- **Cell 75 — Markdown:** 37. Test the simple and streaming RAG variants together
- **Cell 76 — Code:** `def test_rag_chains(question: str) -> None:`
- **Cell 77 — Markdown:** 38. Run the chain-test helper once
- **Cell 78 — Code:** `test_rag_chains("How does NLP work?")`
- **Cell 79 — Markdown:** 39. Test several questions through simple RAG
- **Cell 80 — Code:** `test_questions = [`
- **Cell 81 — Markdown:** 40. Conversational RAG: prompt with chat history
- **Cell 82 — Code:** `conversational_prompt = ChatPromptTemplate.from_messages([`
- **Cell 83 — Markdown:** 41. Build the conversational RAG chain
- **Cell 84 — Code:** `conversational_rag_chain = (`
- **Cell 85 — Markdown:** 42. Start a conversation with empty history
- **Cell 86 — Code:** `chat_history = []`
- **Cell 87 — Markdown:** 43. Add the first turn to chat history
- **Cell 88 — Code:** `chat_history.extend([`
- **Cell 89 — Markdown:** 44. Ask a follow-up question
- **Cell 90 — Code:** `q2 = "How is it different from traditional programming?"`
- **Cell 91 — Markdown:** 45. Interview-ready vocabulary and definitions
- **Cell 92 — Markdown:** 46. Troubleshooting checklist
- **Cell 93 — Markdown:** 47. Practical exercises — prove that you understand the lesson
- **Cell 94 — Markdown:** Official references and source notes

## Instructor notebook and transcript coverage

### Instructor notebook (`2-faiss.ipynb`)

| Original cells | Original teaching purpose | New notebook section |
|---|---|---|
| 0–1 | RAG and FAISS: definition, dense vectors, similarity/nearest-neighbor search, CPU/GPU overview | Opening + Sections 5, 45–47 |
| 2 | Imports and `.env` | Sections 1–4 and separate Gemini/Groq setup cells |
| 3–6 | Four `Document` objects, metadata, chunking, display chunk content | Sections 6–10 |
| 7–10 | OpenAI embedding setup, query embedding, batch embeddings | Replaced by Gemini Embedding 2 direct call, adapter, and batch examples in Sections 11–16 |
| 11–14 | Cosine similarity examples | Sections 17–18 |
| 15–17 | Create and inspect FAISS index | Sections 19–20 |
| 18–19 | Save/load local FAISS index | Sections 21–23; includes pickle warning |
| 20–21 | Similarity search and print top-k results | Section 24 |
| 22–23 | Search with scores and inspect chunks | Section 25 and Section 10 |
| 24–25 | Metadata filter `topic=ML` | Section 26 |
| 26–34 | Groq model, basic prompt, retriever, format docs, LCEL simple RAG | Sections 28–34 |
| 35–37 | Conversational prompt and chain | Sections 40–44 |
| 38–41 | Streaming RAG and test helper/question set | Sections 35–39 |
| 42–44 | Conversation history and follow-up | Sections 42–44 |
| 45 | Empty trailing cell | Removed |

### Transcript coverage

**Transcript 1** (`Pasted text(20261026-085833).txt`) covers: FAISS as efficient dense-vector search; CPU/GPU distinction; imports/dependencies; Document-based ingestion; chunk size 500, overlap 50; embedding models; query vs document embedding; cosine-similarity comparisons; vector-store creation; save/load; top-k search; score output; metadata filter; and transition to building RAG. These are all represented in the notebook. The original OpenAI-specific examples are replaced by Gemini as requested.

**Transcript 2** (`Pasted text (2)(20261026-085932).txt`) covers: Groq/chat model setup; prompt template; retriever with `search_kwargs`; formatting documents; `RunnablePassthrough`; `StrOutputParser`; simple LCEL RAG; conversational prompt/history; streaming chain without a final string parser; testing one question and multiple questions; updating `HumanMessage`/`AIMessage` history; and asking a follow-up. These topics are represented in Sections 28–44.

## Important corrections and rationale

1. **No OpenAI API:** `OpenAIEmbeddings` and `OPENAI_API_KEY` from the instructor file are replaced with Gemini. Generation uses `ChatGroq` and `GROQ_API_KEY` in a separate section.
2. **Provider separation:** Gemini client/key/embedding adapter are in their own cells; Groq key/model/smoke test are in separate later cells. Shared LangChain types do not perform provider requests.
3. **Model ID:** the notebook uses Groq model ID `openai/gpt-oss-120b`, but calls it via Groq. The prefix is the model ID namespace, not an OpenAI API call. Re-check availability against Groq's current model catalogue when running.
4. **Chunking:** `separators=[" "]` was too restrictive for recursive splitting; the notebook uses paragraph, line, space, and character fallback separators.
5. **FAISS score semantics:** scores from the default flat L2 index are distances, with lower values closer. Do not interpret them as percentages or as cosine similarity.
6. **Persistence safety:** `allow_dangerous_deserialization=True` is used only for the trusted self-created local index. Untrusted pickle files must not be loaded.
7. **FAISS is a library/index:** the specific notebook demonstrates CPU flat L2 search, not GPU or advanced approximate index configuration.
8. **Conversational behavior:** chat history is supplied to the generation prompt; the source pattern does not automatically rewrite the raw follow-up query before retrieval. The notebook explains that limitation rather than overclaiming.
9. **Loaders:** `TextLoader` is demonstrated; `PyPDFLoader` is an optional guarded example. Neither is silently mixed into the four-document core corpus.
10. **Runtime:** no API outputs are saved. Static compilation validates Python syntax, not live provider behavior.

## Official documentation used

- [FAISS repository and overview](https://github.com/facebookresearch/faiss)
- [LangChain FAISS integration](https://docs.langchain.com/oss/python/integrations/vectorstores/faiss)
- [faiss-cpu on PyPI](https://pypi.org/project/faiss-cpu/)
- [Gemini embeddings](https://ai.google.dev/gemini-api/docs/embeddings)
- [Groq supported models](https://console.groq.com/docs/models)
- [Groq GPT-OSS 120B model page](https://console.groq.com/docs/model/openai/gpt-oss-120b)
- [LangChain Groq integration](https://python.langchain.com/docs/integrations/chat/groq/)

## Recommended validation checklist

- [ ] Create `.env` with Gemini and Groq keys; never show their values.
- [ ] Run the Gemini direct embedding smoke test.
- [ ] Run the Groq direct generation smoke test.
- [ ] Confirm four sample Documents are created and chunks preserve topic metadata.
- [ ] Confirm FAISS vector count matches the number of chunks.
- [ ] Save and reload only the index that you created and trust.
- [ ] Inspect top-k results and L2 distances.
- [ ] Confirm `topic=ML` filters out the other topics.
- [ ] Run simple, streaming, and conversational demos.
- [ ] Test an unanswerable question and a follow-up query.
- [ ] Do not treat a smoke test as proof of retrieval quality or production readiness.
