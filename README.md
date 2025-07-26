# Setup guide

## 1. Install Required Tools
- Install **Ollama** and pull the following models:
  - `llama3.2`
  - `nomic-embed-text`
- Install **Tesseract OCR** and ensure it is added to your system `PATH`.
- Install **Poppler** (required for `pdf2image`).
- Install necessary libraries:
  - `langchain`
  - `pdf2image`
  - `pytesseract`
  - `chromadb`
  - and others as needed.

## 2. Extract Text from PDF
- Convert PDF pages to images using `pdf2image`.
- Extract text from the images using `pytesseract` (OCR).

## 3. Chunk the Text
- Use a **character-based chunking** strategy with **overlap** to preserve context across segments.

## 4. Generate Embeddings
- Use `nomic-embed-text` model via Ollama to generate embeddings.
- Convert each chunk into a **dense vector representation**.

## 5. Store in ChromaDB
- Store both the text chunks and their embeddings in **ChromaDB**.
- Enables **similarity-based search** and fast retrieval.

## 6. Retrieval and Generation
- When a question is asked:
  - LangChain embeds the query.
  - Similar chunks are retrieved from ChromaDB.
  - Combine the query and relevant chunks.
  - Pass them to `LLaMA 3.2` via Ollama for response generation.

# Used Tools
- **Ollama** – For running local language and embedding models (`llama3.2`, `nomic-embed-text`)
- **Tesseract OCR** – For extracting text from images
- **Poppler** – Dependency for `pdf2image`, used to render PDF pages as images

# Used Python Libraries & Packages
- **LangChain** – To orchestrate the RAG flow (retrieval + generation)
- **pdf2image** – To convert PDF pages into images
- **pytesseract** – Python wrapper for Tesseract, used for OCR
- **ChromaDB** – To store and retrieve document embeddings
- **nomic-embed-text** – Embedding model via Ollama for semantic search
- **llama3.2** – Language model via Ollama for answering queries

# Sample Queries and Outputs

**Input:** "who is onupom?"  
**Output:** 'অনুপম হলেন গল্পের কথক।'

**Input:** "অনুপমের ভাষায় সুপুরুষ কাকে বলা হয়েছে?"  
**Output:** 'শস্তুনাথ'


## What method or library did you use to extract the text, and why? Did you face any formatting challenges with the PDF content?
I chose these methods to solve the problem:

- `pdf2image` handles PDFs that are scanned or image-based.
- `pytesseract` is a widely used OCR engine that supports multiple languages, including Bengali, which was essential for my case.
  
Because I faced formatting and recognition issues, especially:

- Incorrect character rendering of complex Bengali ligatures.


## What chunking strategy did you choose (e.g. paragraph-based, sentence-based, character limit)? Why do you think it works well for semantic retrieval?

I used a character-based chunking strategy to split the OCR-extracted text. It ensures consistent chunk sizes and handles unstructured content well. Overlapping chunks help retain context across boundaries. This improves the quality of semantic retrieval results.

## What embedding model did you use? Why did you choose it? How does it capture the meaning of the text?

I used the nomic-embed-text embedding model for text representation. It supports multilingual input and performs well for semantic tasks. I chose it because it's lightweight and easy to run locally with Ollama. The model converts text into dense vectors based on context. These vectors help retrieve semantically similar chunks during search.

## How are you comparing the query with your stored chunks? Why did you choose this similarity method and storage setup?

I compare the query with stored chunks using cosine similarity. For storage, I use Chroma as the vector store to hold embeddings. Cosine similarity measures the angle between vectors, making it effective for semantic matching. Chroma is simple to set up and integrates well with local workflows. This setup is efficient for fast and relevant similarity search.

## How do you ensure that the question and the document chunks are compared meaningfully? What would happen if the query is vague or missing context?

I ensure meaningful comparison by embedding both the query and chunks using the same model, so they share the same vector space. This allows semantic similarity to be measured accurately. If the query is vague or lacks context, the system may retrieve less relevant chunks. In such cases, adding user guidance or rephrasing helps improve results. Chunk overlap also helps maintain context across sections.

## Do the results seem relevant? If not, what might improve them (e.g. better chunking, better embedding model, larger document)?

The results can sometimes be wrong or irrelevant due to issues like inappropriate chunk size or limitations of the embedding model. Using a more advanced embedding model could also improve overall text representation quality.

