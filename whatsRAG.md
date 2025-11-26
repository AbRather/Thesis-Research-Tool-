What is RAG? (Retrieval-Augmented Generation)
If you are wondering how this "News Research Tool" actually works under the hood, you are looking at a technique called RAG.
It sounds complicated, but the concept is actually very simple.
The Problem: The "Closed Book" Exam
Imagine ChatGPT (or any AI model) is a very smart student taking a test. This student has read millions of books, but there is a catch: they stopped reading in 2023.
If you ask them: "Who won the game last night?" or "What is in this specific news article from today?", they will fail. They simply don't have that information in their memory. They might even try to guess (hallucinate) just to sound helpful.
The Solution: The "Open Book" Exam (RAG)
RAG (Retrieval-Augmented Generation) is the equivalent of letting that student take an open-book exam.
Instead of forcing the AI to rely on its memory, we give it a specific "textbook" (in this case, your news articles) and say: "Use this book to answer the question."
How It Works (The Engineering Process)
This application performs RAG in three distinct phases. Here is the translation from "Concept" to "Code":
Phase 1: Ingestion & Indexing (The Librarian)
Before the AI can answer, we need to prepare the data. This involves three critical engineering steps:
Loading Data (Ingestion):
Concept: Gathering the raw materials.
Technical Process: We use a Loader (UnstructuredURLLoader). It visits the websites, scrapes the HTML, and extracts just the readable text, ignoring ads and navigation bars.
Splitting (Chunking):
Concept: Cutting a long book into bite-sized index cards.
Technical Process: LLMs have a limit on how much text they can read at once (context window). We use a Text Splitter (RecursiveCharacterTextSplitter) to break the long articles into smaller "chunks" (e.g., 1000 characters each). We use "overlap" to ensure sentences aren't cut in half awkwardly.
Embedding & Storage (Vectorization):
Concept: Filing the index cards by meaning, not just alphabetically.
Technical Process: This is the magic. We use an Embedding Model (OpenAIEmbeddings) to convert text chunks into lists of numbers called Vectors. These numbers represent the meaning of the text. We store these in a Vector Database (FAISS) which allows for ultra-fast "Similarity Search."
Phase 2: Retrieval (The Setup)
When you ask a question, the app doesn't send it straight to the AI.
Semantic Search:
The app converts your question into numbers (vectors) too.
It asks FAISS: "Find me the 3 chunks of text that are most mathematically similar to this question."
Phase 3: Generation (The Answer)
Now we have the user's question and the relevant facts found by the search.
Prompt Engineering:
The app constructs a strict instructions prompt for the LLM:System: Answer the question using ONLY the following context.
Context: [Chunk 1] [Chunk 2] [Chunk 3]
User Question: "What is AFP?"
Inference:
The LLM (OpenAI) reads the context and generates the final answer. It cites the sources because it knows exactly which chunk provided the information.
Why Use RAG?
Accuracy: The AI doesn't guess; it reads from the source.
Up-to-Date: You can feed it news from 5 minutes ago, and it will understand it.
Privacy: You can use RAG on your own private company documents without training the public AI on your secrets.
Source Citing: Because the app knows exactly which paragraphs it retrieved, it can tell you where it found the answer (e.g., "Source: Link 1").
