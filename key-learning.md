# Key Learning

## Sequential Flow: From Text to Answer

The complete RAG pipeline follows this sequential process:

### Step 1: TEXT
Raw input from user or document.

### Step 2: EMBEDDING
Text is converted to numerical representation using a machine learning model.

**Action**: Pass text through `SentenceTransformer` or similar embedding model.

**Output**: 384-dimensional vector (in our case with all-MiniLM-L6-v2)

### Step 3: VECTOR
The output embedding is stored as a vector in vector space.

**Action**: Store in OpenSearch with vector field type.

**Benefit**: Similar documents are positioned close together in mathematical space.

### Step 4: OPENSEARCH
Vector database stores and indexes all embeddings.

**Action**: Perform approximate nearest neighbor (ANN) search using KNN plugin.

**Benefit**: Fast retrieval of similar vectors without scanning all documents.

### Step 5: SEMANTIC SEARCH
Query is converted to embedding and compared against stored vectors.

**Action**: Generate embedding for user query using same model.

**Result**: Find most similar documents by cosine similarity or distance metrics.

**Benefit**: Finds meaning-based matches, not keyword matches.

### Step 6: CONTEXT
Retrieved documents are combined with the original query.

**Action**: Format: "Context from documents: [retrieved_text]\n\nOriginal Question: [query]"

**Benefit**: Provides LLM with relevant information to answer accurately.

### Step 7: LLM
Language model processes the context and generates answer.

**Action**: Send prompt with context to Ollama (or other LLM).

**Models available**: Llama, Mistral, DeepSeek, etc.

**Benefit**: Generates human-like responses based on company-specific documents.

### Step 8: ANSWER
Final response generated using retrieved knowledge.

**Output**: Answer grounded in retrieved documents, not hallucination.

**Benefit**: Company information stays private; answers are traceable to sources.


**This is the foundation of modern enterprise GenAI systems.**

---

## Key Takeaways

* Embeddings convert meaning into numbers.
* Vectors represent semantic meaning.
* OpenSearch can act as a vector database.
* Semantic search finds meaning, not keywords.
* LLMs generate answers.
* RAG combines retrieval with generation.
* OpenSearch + Ollama can create a complete local GenAI platform.
