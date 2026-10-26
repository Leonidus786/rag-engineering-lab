# RAG ChromaDB + Gemini + Groq — Cell-by-cell guide and source audit

**Primary file:** `RAG_Chroma_Gemini_Groq_Instructor_Complete_Clean.ipynb`

## How to use this guide

Open the notebook and work from the first cell downward. Each Markdown cell explains the next code step. The API smoke tests and embedding calls may consume quota; they were not executed against personal API credentials during static review.

## Cell-by-cell explanation

| Cell | Type | Label | Purpose / why it exists |
|---:|---|---|---|
| 0 | Markdown | Title and objectives | Teaching explanation: Title and objectives; introduces the concepts, rationale, and expected observations for this stage. |
| 1 | Markdown | Provider responsibility map | Teaching explanation: Provider responsibility map; introduces the concepts, rationale, and expected observations for this stage. |
| 2 | Markdown | Environment and installation | Teaching explanation: Environment and installation; introduces the concepts, rationale, and expected observations for this stage. |
| 3 | Markdown | API keys and .env | Teaching explanation: API keys and .env; introduces the concepts, rationale, and expected observations for this stage. |
| 4 | Code | Load and validate environment variables | Finds and loads .env, checks both required credentials, and avoids printing secrets. |
| 5 | Markdown | Imports | Teaching explanation: Imports; introduces the concepts, rationale, and expected observations for this stage. |
| 6 | Code | Import libraries | Imports each needed integration once with current package boundaries. |
| 7 | Markdown | Gemini embedding setup | Teaching explanation: Gemini embedding setup; introduces the concepts, rationale, and expected observations for this stage. |
| 8 | Code | Initialize Gemini client | Creates Gemini client and declares the embedding model in a dedicated cell. |
| 9 | Markdown | Direct embedding test | Teaching explanation: Direct embedding test; introduces the concepts, rationale, and expected observations for this stage. |
| 10 | Code | Create a direct query embedding | Makes one direct Gemini request and inspects vector dimensionality and sample values. |
| 11 | Markdown | Groq setup | Teaching explanation: Groq setup; introduces the concepts, rationale, and expected observations for this stage. |
| 12 | Code | Initialize Groq chat model | Creates the single generation model that is used throughout; this variable is never overwritten by Gemini. |
| 13 | Markdown | Groq smoke test | Teaching explanation: Groq smoke test; introduces the concepts, rationale, and expected observations for this stage. |
| 14 | Code | Call Groq with a short prompt | Verifies that the Groq-hosted chat model returns generated text independently of RAG. |
| 15 | Markdown | Sample corpus | Teaching explanation: Sample corpus; introduces the concepts, rationale, and expected observations for this stage. |
| 16 | Code | Define the instructor sample documents | Preserves the instructor’s three sample documents and prints the corpus for initial inspection. |
| 17 | Markdown | Save corpus files | Teaching explanation: Save corpus files; introduces the concepts, rationale, and expected observations for this stage. |
| 18 | Code | Write sample files | Writes UTF-8 files in one stable folder; removes duplicate and mismatched file-saving cells. |
| 19 | Markdown | Load files | Teaching explanation: Load files; introduces the concepts, rationale, and expected observations for this stage. |
| 20 | Code | Load text files and attach topics | Loads all text files, sorts them deterministically, checks for an empty directory, and adds topic metadata. |
| 21 | Markdown | Chunking explanation | Teaching explanation: Chunking explanation; introduces the concepts, rationale, and expected observations for this stage. |
| 22 | Code | Split documents | Uses recursive splitting with sensible fallback separators, keeps source metadata, and records starting character positions. |
| 23 | Markdown | Gemini adapter explanation | Teaching explanation: Gemini adapter explanation; introduces the concepts, rationale, and expected observations for this stage. |
| 24 | Code | Implement GeminiEmbeddings adapter | Implements the two methods LangChain requires and uses separate query/document formatting. |
| 25 | Markdown | Adapter test | Teaching explanation: Adapter test; introduces the concepts, rationale, and expected observations for this stage. |
| 26 | Code | Test embedding adapter | Checks the wrapper LangChain and ChromaDB will actually use. |
| 27 | Markdown | Embedding caching | Teaching explanation: Embedding caching; introduces the concepts, rationale, and expected observations for this stage. |
| 28 | Code | Wrap Gemini embeddings with persistent caching | Creates one reusable persistent cache for document and query embeddings instead of duplicate cross-notebook JSON snippets. |
| 29 | Markdown | Cache test | Teaching explanation: Cache test; introduces the concepts, rationale, and expected observations for this stage. |
| 30 | Code | Request a query embedding twice | Demonstrates repeatable cache behavior without a custom standalone JSON cache implementation. |
| 31 | Markdown | ChromaDB setup | Teaching explanation: ChromaDB setup; introduces the concepts, rationale, and expected observations for this stage. |
| 32 | Code | Create the persistent Chroma collection | Creates a persistent collection with cosine distance and optionally resets this tutorial's collection to make repeated Run All executions deterministic. |
| 33 | Markdown | Verify storage | Teaching explanation: Verify storage; introduces the concepts, rationale, and expected observations for this stage. |
| 34 | Code | Inspect stored Chroma records | Checks storage through a public interface and avoids private collection methods. |
| 35 | Markdown | Similarity search explanation | Teaching explanation: Similarity search explanation; introduces the concepts, rationale, and expected observations for this stage. |
| 36 | Code | Run the instructor search questions | Retains all three distinct instructor query examples but reuses one helper instead of repeating the same code cell three times. |
| 37 | Markdown | Score interpretation | Teaching explanation: Score interpretation; introduces the concepts, rationale, and expected observations for this stage. |
| 38 | Code | Print retrieved documents with distances | Shows scores and explains their direction under the explicitly configured cosine-distance metric. |
| 39 | Markdown | Metadata filtering | Teaching explanation: Metadata filtering; introduces the concepts, rationale, and expected observations for this stage. |
| 40 | Code | Search only within one topic | Demonstrates metadata filtering via the public similarity-search API. |
| 41 | Markdown | Retriever explanation | Teaching explanation: Retriever explanation; introduces the concepts, rationale, and expected observations for this stage. |
| 42 | Code | Create the retriever | Uses the corrected retriever option and confirms that invoke returns Document objects. |
| 43 | Markdown | Prompt template | Teaching explanation: Prompt template; introduces the concepts, rationale, and expected observations for this stage. |
| 44 | Code | Build the reusable RAG prompt | Creates one canonical prompt used by both the classic chain and LCEL alternative. |
| 45 | Markdown | Document chain | Teaching explanation: Document chain; introduces the concepts, rationale, and expected observations for this stage. |
| 46 | Code | Create the document chain | Connects the Groq generation model with the prompt; no embedding model is used here. |
| 47 | Markdown | Retrieval chain | Teaching explanation: Retrieval chain; introduces the concepts, rationale, and expected observations for this stage. |
| 48 | Code | Create the end-to-end RAG chain | Builds the instructor-style RAG pipeline while retaining the single Groq LLM variable. |
| 49 | Markdown | First RAG query | Teaching explanation: First RAG query; introduces the concepts, rationale, and expected observations for this stage. |
| 50 | Code | Invoke the RAG chain and inspect context | Runs a sample question and inspects both the answer and retrieved source documents. |
| 51 | Markdown | Reusable query function | Teaching explanation: Reusable query function; introduces the concepts, rationale, and expected observations for this stage. |
| 52 | Code | Define and test query_rag_modern | Preserves the instructor reusable query helper and three test questions without duplicating chain setup. |
| 53 | Markdown | LCEL alternative | Teaching explanation: LCEL alternative; introduces the concepts, rationale, and expected observations for this stage. |
| 54 | Code | Format documents for LCEL | Defines how multiple retrieved Document objects become one context string for the LCEL prompt. |
| 55 | Markdown | Create the LCEL-specific prompt | Teaching explanation: Create the LCEL-specific prompt; introduces the concepts, rationale, and expected observations for this stage. |
| 56 | Code | Create the LCEL-specific prompt | Preserves the instructor's distinct custom prompt and explains the {context}/{question} keys used by the LCEL chain. |
| 57 | Markdown | Compose LCEL runnables | Teaching explanation: Compose LCEL runnables; introduces the concepts, rationale, and expected observations for this stage. |
| 58 | Code | Build the LCEL chain | Composes retrieval, context formatting, the instructor-specific prompt, Groq generation, and string parsing in one pipeline. |
| 59 | Markdown | Test one LCEL question | Teaching explanation: Test one LCEL question; introduces the concepts, rationale, and expected observations for this stage. |
| 60 | Code | Test LCEL RAG chain | Runs one LCEL question without reinitializing the LLM or vector store. |
| 61 | Markdown | Reusable LCEL query function | Teaching explanation: Reusable LCEL query function; introduces the concepts, rationale, and expected observations for this stage. |
| 62 | Code | Define query_rag_lcel | Preserves the instructor's reusable LCEL query helper and modernizes it around one consistent `.invoke()` call. |
| 63 | Markdown | Run several LCEL questions | Teaching explanation: Run several LCEL questions; introduces the concepts, rationale, and expected observations for this stage. |
| 64 | Code | Run the LCEL test set | Preserves the instructor's LCEL batch tests while correcting the misspelled test query and avoiding repeated one-off cells. |
| 65 | Markdown | Add new document | Teaching explanation: Add new document; introduces the concepts, rationale, and expected observations for this stage. |
| 66 | Code | Create the reinforcement-learning Document | Preserves the instructor's new reinforcement-learning document with meaningful source and topic metadata. |
| 67 | Markdown | Split and insert the new passage | Teaching explanation: Split and insert the new passage; introduces the concepts, rationale, and expected observations for this stage. |
| 68 | Code | Split and add the new document | Splits and adds new content through the public LangChain vector-store API with stable IDs. |
| 69 | Markdown | Test new document | Teaching explanation: Test new document; introduces the concepts, rationale, and expected observations for this stage. |
| 70 | Code | Query after updating ChromaDB | Checks that the new chunks participate in the existing RAG pipeline. |
| 71 | Markdown | Conversational memory concepts | Teaching explanation: Conversational memory concepts; introduces the concepts, rationale, and expected observations for this stage. |
| 72 | Code | Create the history-rewrite prompt | Creates the prompt for reformulating follow-up questions without answering them. |
| 73 | Markdown | Create the history-aware retriever | Teaching explanation: Create the history-aware retriever; introduces the concepts, rationale, and expected observations for this stage. |
| 74 | Code | Create history-aware retriever | Uses the same Groq model and Chroma retriever, adding question reformulation when history is present. |
| 75 | Markdown | Conversational answer prompt | Teaching explanation: Conversational answer prompt; introduces the concepts, rationale, and expected observations for this stage. |
| 76 | Code | Create conversational QA chain and RAG chain | Connects history-aware retrieval to a distinct answer chain; avoids swapping the global LLM variable. |
| 77 | Markdown | First conversational query | Teaching explanation: First conversational query; introduces the concepts, rationale, and expected observations for this stage. |
| 78 | Code | Ask the first conversational question | Runs the first conversational turn and prints the retrieved document metadata. |
| 79 | Markdown | Update history | Teaching explanation: Update history; introduces the concepts, rationale, and expected observations for this stage. |
| 80 | Code | Append the first exchange to chat history | Adds the first user/assistant exchange to the explicit history list. |
| 81 | Markdown | Follow-up query | Teaching explanation: Follow-up query; introduces the concepts, rationale, and expected observations for this stage. |
| 82 | Code | Run the context-dependent follow-up | Demonstrates context-dependent retrieval and inspects the sources that supported the answer. |
| 83 | Markdown | Troubleshooting and definitions | Teaching explanation: Troubleshooting and definitions; introduces the concepts, rationale, and expected observations for this stage. |
| 84 | Markdown | Instructor and custom notebook source audit | Teaching explanation: Instructor and custom notebook source audit; introduces the concepts, rationale, and expected observations for this stage. |

## Important implementation choices

1. **Gemini is only the embedding provider.** The direct test and `GeminiEmbeddings` adapter use `google-genai` and `gemini-embedding-2`.
2. **Groq is only the generation provider.** One `groq_llm` variable is initialized once and reused for smoke testing, classic RAG, LCEL, and conversational RAG.
3. **Groq is the generation API provider.** The notebook uses `ChatGroq` with the Groq-hosted model ID `qwen/qwen3.8-27b` and `GROQ_API_KEY`; it does not instantiate an OpenAI client or use an OpenAI API key. Groq currently labels this model Preview, so verify availability/status and select an appropriate model before production use.
4. **Gemini Embedding 2 uses role-specific text formatting.** Query strings use `task: question answering | query: ...`; document strings use `title: none | text: ...`. Model changes or formatting changes require a new cache namespace and re-embedding the affected corpus.
5. **The cache is integrated.** `CacheBackedEmbeddings` wraps the Gemini adapter and is passed to Chroma; it is not a separate JSON exercise that Chroma ignores.
6. **Chroma uses a public interface.** The notebook does not write through `vectorstore._collection`; it uses a persistent Chroma client plus `Chroma.from_documents`, `vectorstore.get`, `vectorstore.similarity_search`, and `vectorstore.add_documents`. A reset flag makes Run All repeatable.
7. **Both RAG styles are preserved.** `rag_chain` uses the instructor’s classic helper pattern. `rag_chain_lcel` is the alternative composition pattern with its own instructor-style `custom_prompt`, plus a reusable `query_rag_lcel` helper; both use the same retriever and Groq model.
8. **Conversation history is in memory only.** It demonstrates history-aware retrieval but is not a production multi-user persistence mechanism.

## Source coverage map

| Instructor cells | Coverage in cleaned notebook |
|---|---|
| 0–3 | Intro, architecture, environment/imports; OpenAI provider details replaced by requested Gemini + Groq separation. |
| 4–14 | Sample corpus, file writing/loading, document preview, chunking. |
| 15–18 | Direct embedding and vector preview, implemented with Gemini. |
| 19–29 | Chroma collection, similarity search examples, distance-score explanation. |
| 30–34 | LLM configuration and smoke test, implemented with Groq only. |
| 35–49 | Retriever, prompt, stuff-document chain, retrieval chain, query helper, test questions. |
| 50–61 | LCEL alternative, separate `custom_prompt`, formatting helper, `query_rag_lcel` helper, and test questions. |
| 62–72 | Add reinforcement learning document, split/add to vector store, query it. |
| 73–83 | Conversational memory, message placeholders, first turn and follow-up. |
| 84–91 | Groq code moved to the dedicated early setup section and consolidated. |
| 92 | Empty trailing cell removed. |

## Changes from the custom notebook

- Removed the second OpenAI-themed introduction.
- Removed duplicate single-vector cache cells and duplicate chunk-cache/Chroma upsert cells; replaced them with one cache-backed embedding wrapper used by the main pipeline.
- Removed the additional Gemini chat-generation model because this architecture uses Gemini for embeddings and Groq for generation only.
- Removed repeated prompt/chain setup and inconsistent `llm` versus `groq_llm` names.
- Removed private `vectorstore._collection` usage.
- Retained the valuable embedding cache idea, semantic search, distance inspection, metadata filtering, LCEL alternative, add-document workflow, and conversational memory.
- Added explicit source metadata, stable IDs, validation guards, and explanations before each major code cell.

## What has been validated

- Notebook JSON structure and code-cell syntax were checked.
- The notebook contains no saved code outputs or execution counters, avoiding stale state from out-of-order execution.
- API calls, ChromaDB runtime behavior, and LangChain package compatibility were **not** executed in this review. Run all cells in the target environment before teaching or publishing.

## Official references

- Gemini Embeddings: https://ai.google.dev/gemini-api/docs/embeddings
- Groq supported models: https://console.groq.com/docs/models
- LangChain Chroma integration: https://docs.langchain.com/oss/python/integrations/vectorstores/chroma
- LangChain Groq integration: https://docs.langchain.com/oss/python/integrations/chat/groq
- LangChain embedding cache reference: https://docs.langchain.com/oss/python/integrations/embeddings

## Detailed source-cell crosswalk

### Instructor notebook — `1-chromadb(2).ipynb`

Every original cell index appears below. “Merged” means its teaching purpose is still represented but the output-only, duplicated, or conflicting cell itself was not copied verbatim. OpenAI-specific executable calls are intentionally replaced because this notebook is configured for Gemini embeddings and Groq generation only.

| Source cell | Original first line | Cleaned cell(s) | Treatment |
|---:|---|---|---|
| 0 | ### Building a RAG System with LangChain and ChromaDB | 0 | Intro and learning goals retained; updated title/provider architecture. |
| 1 | import os | 3–4 | Environment setup retained; OpenAI key replaced with safe Gemini + Groq credential validation. |
| 2 | ## langchain imports | 5–6 | Imports consolidated and modernized; OpenAI integrations intentionally replaced. |
| 3 | # RAG Architecture Overview | 0–1 | Architecture explanation retained in Markdown instead of a print-only diagram cell. |
| 4 | ### 1. Sample Data | 15 | Sample-data section retained. |
| 5 | ## create sample documents | 16 | All three instructor sample documents retained. |
| 6 | ## save sample documents to files | 17–18 | File-writing stage retained with one stable project directory. |
| 7 | ## save sample documents to files | 17–18 | Duplicate file-writing cell consolidated; conflicting output path removed. |
| 8 | temp_dir | 18 | Temporary-path display replaced by an explicit resolved project path. |
| 9 | ### 2. Document Loading | 19 | Document-loading explanation retained. |
| 10 | from langchain_community.document_loaders import DirectoryLoader,TextLoader | 6, 20 | Loader imports consolidated; loading logic retained with UTF-8 encoding and metadata. |
| 11 | documents | 20 | Document inspection merged into the loading cell. |
| 12 | ### Document Splitting | 21 | Chunking explanation retained and expanded. |
| 13 | # Initialize text splitter | 22 | Splitter retained with recursive separators and source-position metadata. |
| 14 | chunks | 22 | Chunk inspection merged into the splitting cell. |
| 15 | ### Embedding Models | 23 | Embedding section retained, provider changed to Gemini as requested. |
| 16 | os.environ["OPENAI_API_KEY"]=os.getenv("OPENAI_API_KEY") | 3–4 | OpenAI key line intentionally replaced by Gemini and Groq key validation. |
| 17 | sample_text="MAchine LEarning is fascinating" | 10 | Direct embedding example retained with Gemini Embedding 2. |
| 18 | vector=embeddings.embed_query(sample_text) | 10, 24–26 | Vector creation/inspection preserved through direct API and LangChain-adapter tests. |
| 19 | ### Intilialize the ChromaDB Vector Store And Stores the chunks in Vector Representation | 31 | Chroma storage section retained. |
| 20 | chunks | 22, 31–34 | Chunk inspection and vector storage retained in separate, explicit steps. |
| 21 | ## Create a Chromdb vector store | 32 | Chroma initialization retained; stable IDs and clean rerun behavior added. |
| 22 | ### Test Similarity Search | 35 | Similarity-search section retained. |
| 23 | query="What are the types of machine learning?" | 36 | All three distinct search questions retained in a reusable helper/list. |
| 24 | query="what is NLP?" | 36 | All three distinct search questions retained in a reusable helper/list. |
| 25 | query="what is Deep Learning?" | 36 | All three distinct search questions retained in a reusable helper/list. |
| 26 | print(f"Query: {query}") | 36 | All three distinct search questions retained in a reusable helper/list. |
| 27 | ### Advanced Similarity Search With Scores | 37 | Score section retained. |
| 28 | results_scores=vectorstore.similarity_search_with_score(query,k=3) | 38 | Distance-search code retained. |
| 29 | #### Understanding Similarity Scores | 37–38 | Score meaning explained with cosine-distance direction; scores not presented as confidence. |
| 30 | #### Initialize LLM, RAG Chain, Prompt Template,Query the RAG system | 11–14, 43–50 | LLM/RAG section retained; Gemini for embeddings and Groq for generation, in separate cells. |
| 31 | from langchain_openai import ChatOpenAI | 12 | ChatOpenAI initialization replaced by the dedicated Groq model cell. |
| 32 | test_response=llm.invoke("What is Large Language Models") | 14 | Direct generation smoke-test concept retained using Groq. |
| 33 | from langchain.chat_models.base import init_chat_model | 12 | OpenAI `init_chat_model` initialization replaced by the single Groq setup. |
| 34 | llm.invoke("What is AI") | 14 | Direct LLM invocation retained as a Groq smoke test. |
| 35 | ### Modern RAG Chain | 45–50 | Modern RAG chain lesson retained. |
| 36 | from langchain.chains import create_retrieval_chain | 6, 48 | Chain imports consolidated; retrieval-chain creation remains explicit. |
| 37 | ## Convert vector store to retriever | 41–42 | Retriever conversion retained; `search_kwarg` typo corrected to `search_kwargs`. |
| 38 | ## Create a prompt template | 43–44 | Prompt template and prompt inspection consolidated into one explained cell. |
| 39 | prompt | 43–44 | Prompt template and prompt inspection consolidated into one explained cell. |
| 40 | ##### What is create_stuff_documents_chain? | 45 | `create_stuff_documents_chain` explanation retained. |
| 41 | ### Create a document chain | 46 | Document-chain creation retained, using Groq. |
| 42 | This chain: | 45–46 | “Stuff” behavior explanation retained and clarified. |
| 43 | #### What is create_retrieval_chain? | 47–48 | Retrieval-chain explanation retained. |
| 44 | ### Create The Final RAG Chain | 48 | Final classic RAG chain creation retained. |
| 45 | [empty cell] | — | Empty cell removed. |
| 46 | response=rag_chain.invoke({"input":"What is Deep LEarning"}) | 49–50 | Invocation, answer, and context inspection merged into one end-to-end example. |
| 47 | response | 49–50 | Invocation, answer, and context inspection merged into one end-to-end example. |
| 48 | response['answer'] | 49–50 | Invocation, answer, and context inspection merged into one end-to-end example. |
| 49 | # Function to query the modern RAG system | 51–52 | Reusable classic RAG query helper retained. |
| 50 | ### Create RAG Chain Alternative - Using LCEL (LangChain Expression Language) | 53 | LCEL section retained as a separately labelled alternative. |
| 51 | # Even more flexible approach using LCEL | 58 | LCEL runnable imports kept where the LCEL chain is built. |
| 52 | # Create a custom prompt | 55–56 | Instructor’s separate LCEL custom prompt retained. |
| 53 | retriever | 42 | Retriever behavior verified by invocation rather than a display-only cell. |
| 54 | ## Format the output documents for the prompt | 54 | Document-formatting helper retained. |
| 55 | ## Build the chain ussing LCEL | 57–58 | LCEL composition retained with matching `context` and `question` keys. |
| 56 | response=rag_chain_lcel.invoke("What is Deep Learning") | 59–60 | Single-question LCEL invocation retained. |
| 57 | retriever.get_relevant_documents("What is Deep Learning") | 42 | Deprecated retriever call replaced by current `.invoke()` pattern. |
| 58 | # Query using the LCEL approach - Fixed version | 61–64 | Reusable `query_rag_lcel` helper retained; tests use valid question wording. |
| 59 | # Test LCEL chain | 63–64 | LCEL test section retained. |
| 60 | query_rag_lcel("What is machine learning?") | 63–64 | Machine-learning LCEL test retained in the loop. |
| 61 | query_rag_lcel("What is depe learning?") | 63–64 | Misspelled “depe learning” corrected; valid deep-learning test uses same helper. |
| 62 | ### Add New Documents To Existing Vector Store | 65 | Add-document section retained. |
| 63 | vectorstore | 33–34 | Display-only vectorstore inspection merged into public storage verification. |
| 64 | # Add new documents to the existing vector store | 65–66 | Reinforcement-learning document text retained. |
| 65 | new_document | 66 | Document preview merged into document-creation cell. |
| 66 | chunks | 67–68 | Chunking/insertion outputs merged into insertion verification. |
| 67 | new_doc=Document( | 66 | Document object creation retained with source/topic metadata. |
| 68 | new_doc | 66 | Document-object display merged into creation cell. |
| 69 | ## split the documents | 67–68 | New-document splitting retained and explained. |
| 70 | ### Add new documents to vectorstore | 68 | Adding chunks to Chroma retained using public `add_documents()` API. |
| 71 | print(f"Added {len(new_chunks)} new chunks to the vector store") | 68 | Count/ID feedback retained; private `_collection.count()` removed. |
| 72 | ## query with the updated vector | 69–70 | Follow-up retrieval after insertion retained, with spelling corrected and source context displayed. |
| 73 | ### Advanced Rag Techniques- Conversational Memory | 71 | Conversational-memory explanation retained. |
| 74 | - create_history_aware_retriever: Makes the retriever understand conversation context | 71, 72–74 | History-aware retriever and message-placeholder concepts retained. |
| 75 | from langchain.chains import create_history_aware_retriever | 6, 72 | Imports consolidated at the top; history-aware retriever implementation kept later. |
| 76 | ## create a prompt that includes the chat history | 72 | Question-reformulation prompt retained. |
| 77 | ## create history aware retriever | 74 | History-aware retriever creation retained with Groq. |
| 78 | # Create a new document chain with history | 75–76 | Separate conversational answer prompt and chain retained. |
| 79 | chat_history=[] | 77–78 | First conversational turn retained. |
| 80 | chat_history.extend([ | 79–80 | History update retained. |
| 81 | chat_history | 80 | History inspection merged into history-update cell. |
| 82 | ## Follow up question | 81–82 | Follow-up question and context-dependent retrieval retained. |
| 83 | result2['answer'] | 82 | Answer/context inspection merged into follow-up output cell. |
| 84 | ### Using GROQ LLM's | 11–14 | Groq section retained but moved before the first generation use, so top-to-bottom execution works. |
| 85 | llm | 12 | Display-only `llm` cell replaced by model details during Groq initialization. |
| 86 | load_dotenv() | 3–4 | Repeated dotenv loading consolidated into credential setup. |
| 87 | os.getenv("GROQ_API_KEY") | 4 | Groq key presence validated without printing the secret. |
| 88 | from langchain_groq import ChatGroq | 6, 12 | Groq import consolidated; model setup is in its own cell. |
| 89 | os.environ["GROQ_API_KEY"]=os.getenv("GROQ_API_KEY") | 4 | Repeated environment assignment removed; secret remains in environment only. |
| 90 | llm=ChatGroq(model="gemma2-9b-it",api_key=os.getenv("GROQ_API_KEY")) | 12 | Groq ChatGroq initialization retained early; model ID updated to a currently documented Groq-hosted model. |
| 91 | llm=init_chat_model(model="groq:gemma2-9b-it") | 12 | Duplicate `init_chat_model` Groq setup consolidated into the single `ChatGroq` initialization. |
| 92 | [empty cell] | — | Empty trailing cell removed. |

### Custom notebook — `self_learning_chromadb(4).ipynb`

Every original cell index appears below. “Merged” means its teaching purpose is still represented but the output-only, duplicated, or conflicting cell itself was not copied verbatim. OpenAI-specific executable calls are intentionally replaced because this notebook is configured for Gemini embeddings and Groq generation only.

| Source cell | Original first line | Cleaned cell(s) | Treatment |
|---:|---|---|---|
| 0 | # Building a RAG System with Gemini, ChromaDB, and Groq | 0 | Custom title retained. |
| 1 | ### Building a RAG System with LangChain and ChromaDB | 0 | Duplicate OpenAI-themed title merged into the main title/architecture explanation. |
| 2 | ## 6. Understanding RAG in Simple Language | 0–1 | Plain-language RAG overview retained and expanded. |
| 3 | ## 7. Understanding API Keys and Environment Variables | 3–4 | Credential explanation retained and made safe. |
| 4 | ## 8. Preparing the Python Environment | 2 | Environment/package setup retained and clarified. |
| 5 | ## 9. Importing the Required Python Libraries | 5–6 | Import explanation retained; repeated imports consolidated. |
| 6 | # Standard Python libraries | 6 | Standard-library import lines consolidated with the single import cell. |
| 7 | ## 10. Loading API Keys from the .env File | 3 | API-key safety explanation retained. |
| 8 | # Find the .env file, including parent folders | 4 | Environment loader retained and validates both required keys without printing values. |
| 9 | ## 11. Initializing the Gemini and Groq Clients | 7, 11 | Provider initialization explanation retained and separated by provider responsibility. |
| 10 | # Initialize the Gemini API client | 8 | Gemini embedding client initialization retained. |
| 11 | ## 12. Testing Gemini Embeddings | 9 | Gemini embedding test explanation retained. |
| 12 | test_text = "RAG retrieves relevant information before generating an answer." | 10 | Direct embedding smoke test retained. |
| 13 | ## 13. Testing the Groq Language Model | 13 | Groq test explanation retained. |
| 14 | test_response = llm.invoke( | 14 | Direct LLM smoke test retained with the one Groq model variable. |
| 15 | # 14. Creating Sample Documents | 15 | Sample-document explanation retained. |
| 16 | sample_docs = [ | 16 | Three sample documents retained. |
| 17 | ## 15. Saving Sample Documents as Text Files | 17 | Save-file explanation retained. |
| 18 | # Define a stable folder for our sample text files | 18 | File-writing logic retained with stable path and UTF-8 encoding. |
| 19 | ## 16. Document Loading | 19 | Document-loading explanation retained. |
| 20 | # Import the document-loading tools | 6, 20 | Loader imports consolidated; same file-loading function remains. |
| 21 | documents | 20 | Document inspection merged into loader cell. |
| 22 | ## 17. Document Splitting | 21 | Chunking explanation retained and improved. |
| 23 | # Initialize the text splitter | 22 | Splitter code retained with improved recursive separators. |
| 24 | chunks | 22 | Chunk preview merged into split cell. |
| 25 | ## 18. Gemini Embedding Model | 23 | Gemini embedding explanation retained. |
| 26 | # Code: Gemini adapter | 24 | Gemini adapter retained and integrated with Chroma. |
| 27 | sample_text = "Machine learning is fascinating" | 10, 26 | Duplicate direct embedding test merged into direct and adapter tests. |
| 28 | vectorstore = Chroma.from_documents( | 32 | Chroma creation retained but integrated with cached embeddings and resettable persistence. |
| 29 | vectorstore | 34 | Vectorstore inspection moved to a public API verification cell. |
| 30 | ## Reusing an Embedding Across Notebooks | 27–30 | Embedding cache concept retained and integrated into the actual Chroma embedding path. |
| 31 | import json | 27–30 | Redundant `json` cache code/import removed; one supported persistent embedding cache replaces it. |
| 32 | ## Reusing a Previously Generated Gemini Embedding | 27–30 | Previously-generated embedding idea explained through cache reuse test. |
| 33 | import json | 27–30 | Duplicate JSON cache cell removed. |
| 34 | ## 19. Save and Reuse Document Chunk Embeddings | 27–30 | Document chunk cache idea retained through LangChain cache-backed embeddings. |
| 35 | import json | 27–30 | Standalone JSON chunk-cache code removed to avoid two competing cache implementations. |
| 36 | ## 20. Initializing ChromaDB with Precomputed Gemini Embeddings | 31–32 | Precomputed-vector/Chroma section retained with a single consistent collection setup. |
| 37 | # Confirm that every chunk has a corresponding embedding | 22, 28, 32 | Embedding coverage check replaced by adapter/cache tests and Chroma insertion verification. |
| 38 | # ## 20. Initializing ChromaDB with Precomputed Gemini Embeddings | 31–32 | Duplicate initialization heading removed. |
| 39 | # Confirm that every chunk has a corresponding embedding | 32 | Duplicate embedding/Chroma code removed; single collection initialization retained. |
| 40 | ## 21. Verifying the ChromaDB Collection | 33–34 | Collection verification retained and expanded. |
| 41 | stored_records = vectorstore._collection.get( | 34 | Private `_collection.get/count` access replaced with public `vectorstore.get()`. |
| 42 | ## 22. Testing Similarity Search | 35 | Similarity-search explanation retained. |
| 43 | # First query | 35–36 | All three ML/NLP/DL queries retained in one reusable query helper/test list. |
| 44 | # NLP query | 35–36 | All three ML/NLP/DL queries retained in one reusable query helper/test list. |
| 45 | # Deep Learning query | 35–36 | All three ML/NLP/DL queries retained in one reusable query helper/test list. |
| 46 | # Inspect similarity scores | 37–38 | Score inspection retained with explicit cosine-distance interpretation. |
| 47 | ## 23. Initializing the Large Language Model (LLM) | 11 | LLM explanation retained but role clarified: Groq generates; Gemini embeds. |
| 48 | import os | 3–4 | Credential validation centralized; duplicate Gemini chat-key setup removed. |
| 49 | from langchain_google_genai import ChatGoogleGenerativeAI | — | Gemini chat-generation implementation removed because the requested architecture uses Gemini embeddings + Groq generation only; its explanatory purpose remains in the provider responsibility map. |
| 50 | ## 23. Building the Complete RAG Chain | 11–14, 43–50 | Complete RAG-chain section retained; provider setup placed before use. |
| 51 | from langchain_groq import ChatGroq | 6, 12 | Groq import consolidated; one model setup cell initializes `groq_llm`. |
| 52 | # Confirm that our existing Groq LLM is available | 12 | LLM class/model confirmation moved to the dedicated Groq initialization cell. |
| 53 | ## 24. Creating the RAG Prompt Template | 43 | Prompt explanation retained. |
| 54 | from langchain_core.prompts import ChatPromptTemplate | 44 | Main prompt retained and made the single canonical prompt for the classic chain. |
| 55 | ## 25. Creating the Document Chain | 45 | Document-chain explanation retained. |
| 56 | from langchain_classic.chains.combine_documents import ( | 46 | Document chain retained. |
| 57 | ## 26. Connecting Retrieval with Generation | 47 | Retrieval-chain explanation retained. |
| 58 | from langchain_classic.chains import create_retrieval_chain | 42, 48 | Retriever and retrieval-chain construction retained in separate cells. |
| 59 | ## 27. Asking Questions to the RAG System | 49 | Question-test explanation retained. |
| 60 | # First Rag Query | 50 | First RAG invocation and context output retained. |
| 61 | ## 24. Creating the RAG Prompt Template | 43 | Duplicate prompt section removed; main prompt heading retained once. |
| 62 | from langchain_core.prompts import ChatPromptTemplate | 44 | Duplicate main prompt merged; instructor-specific LCEL prompt is separately retained at cleaned cell 56. |
| 63 | ## 25. Building the Retrieval-Augmented Generation (RAG) Chain | 47–48 | Duplicate RAG architecture explanation consolidated. |
| 64 | from langchain_classic.chains import create_retrieval_chain | 6, 46, 48 | Duplicate imports/chain setup consolidated into one classic chain. |
| 65 | # Retrieve the top 3 relevant chunks from ChromaDB | 42, 46, 48 | Repeated retriever/document chain/retrieval chain removed; one reliable chain remains. |
| 66 | response = rag_chain.invoke({ | 50 | Repeated question invocation merged into the first end-to-end test. |
| 67 | from langchain_classic.chains import ( | 6, 46, 48 | Duplicate chain imports removed. |
| 68 | ## 26. Querying the RAG System | 51 | Query helper explanation retained. |
| 69 | question = "What are the types of machine learning?" | 52 | Reusable classic query helper retained. |
| 70 | ### What is create_stuff_documents_chain? | 45 | Explanation of `create_stuff_documents_chain` retained. |
| 71 | from langchain_classic.chains.combine_documents import ( | 46 | Repeated document chain code consolidated. |
| 72 | from langchain_classic.chains import create_stuff_documents_chain | 6, 46 | Duplicate import/initialization removed. |
| 73 | ### What is create_retrieval_chain? | 47 | Explanation of `create_retrieval_chain` retained. |
| 74 | from langchain_classic.chains import create_retrieval_chain | 48 | Retrieval chain initialization retained once. |
| 75 | [empty cell] | — | Empty cell removed. |
| 76 | response = rag_chain.invoke({ | 50 | Repeated RAG call merged into the end-to-end example. |
| 77 | response["answer"] | 50 | Answer-only inspection merged into the end-to-end example. |
| 78 | response["context"] | 50 | Retrieved-context inspection merged into the end-to-end example. |
| 79 | ### Function to Query the Modern RAG System | 51 | Query-helper section retained. |
| 80 | def query_rag_modern(question): | 52 | Query-helper implementation retained. |
| 81 | test_questions = [ | 52 | Test-question list retained with the helper tests. |
| 82 | ## Create RAG Chain Alternative — Using LCEL (LangChain Expression Language) | 53 | LCEL introduction retained. |
| 83 | from langchain_core.output_parsers import StrOutputParser | 58 | Runnable/output-parser imports retained in the LCEL chain cell. |
| 84 | # Create a custom prompt | 55–58 | The custom LCEL prompt idea retained in its own cell. |
| 85 | # Format retrieved documents | 54 | Document formatting helper retained. |
| 86 | # Build the LCEL RAG chain | 58 | LCEL chain retained with consistent `context`/`question` inputs. |
| 87 | # Test the LCEL chain | 60 | Single LCEL invocation retained. |
| 88 | for question in test_questions: | 63–64 | Batch LCEL questions retained through `query_rag_lcel` helper. |
| 89 | from dotenv import load_dotenv | 3–4 | OpenAI key read removed; required Gemini/Groq keys are checked safely in the environment cell. |
| 90 | [empty cell] | — | Empty trailing cell removed if present. |

### What this crosswalk guarantees—and what it does not

This crosswalk accounts for every original cell, including blank cells and display-only cells. It does **not** claim literal line-for-line identity with the instructor notebook: provider-specific OpenAI calls were replaced by Gemini/Groq equivalents, repeated setup and output-only cells were merged, the legacy retriever typo was fixed, and model/provider initialization was moved before use. The clean notebook is a coherent, runnable teaching sequence intended to retain all distinct instructional stages while avoiding duplicate or contradictory implementations.
