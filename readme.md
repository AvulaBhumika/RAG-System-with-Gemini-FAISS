# RAG System with Gemini & FAISS

A production-ready Retrieval-Augmented Generation (RAG) system that combines Google's Gemini AI with efficient vector search to create an intelligent document Q&A system.

##  Overview

This RAG system allows you to upload PDF and DOCX documents and ask natural language questions about their content. It uses state-of-the-art embedding models for semantic search and Google's Gemini for generating contextually accurate answers.

### Key Features

- ** Multi-format Support**: PDF and DOCX document processing
- ** Smart Chunking**: Intelligent text segmentation with overlap for better context
- ** Vector Search**: FAISS-powered similarity search for relevant content retrieval
- ** AI-Powered Answers**: Google Gemini Pro for high-quality response generation
- ** Persistence**: Save and load your knowledge base for future use
- ** Fast Retrieval**: Optimized vector operations for quick query responses
- ** Configurable**: Adjustable chunk sizes, retrieval counts, and more

##  Architecture

```
Document Input (PDF/DOCX)
        ↓
   Text Extraction
        ↓
   Smart Chunking
        ↓
  Embedding Generation (SentenceTransformers)
        ↓
   Vector Storage (FAISS)
        ↓
[User Query] → Similarity Search → Context Retrieval → Gemini AI → Answer
```

##  Installation

### Prerequisites
- Python 3.8+
- Google Gemini API key

### Install Dependencies

```bash
pip install google-generativeai faiss-cpu sentence-transformers PyPDF2 python-docx numpy
```

### For GPU acceleration (optional):
```bash
pip install faiss-gpu  # Replace faiss-cpu with faiss-gpu
```

##  Quick Start

### 1. Get Your Gemini API Key
1. Visit [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Create a new API key
3. Keep it secure!

### 2. Basic Usage

```python
from rag_system import RAGSystem

# Initialize the system
rag = RAGSystem("your-gemini-api-key")

# Add documents to your knowledge base
rag.add_document("research_paper.pdf")
rag.add_document("company_handbook.docx")

# Ask questions
result = rag.query("What are the main findings in the research?")
print(result['answer'])
print(f"Sources: {result['sources']}")
```

### 3. Interactive Mode

Run the main script for an interactive Q&A session:

```python
python rag_system.py
```

## 📚 Detailed Usage

### Document Management

```python
# Add single document
rag.add_document("document.pdf")

# Add multiple documents
documents = ["doc1.pdf", "doc2.docx", "doc3.pdf"]
for doc in documents:
    rag.add_document(doc)

# Save your knowledge base
rag.save_index("my_knowledge_base")

# Load existing knowledge base
rag.load_index("my_knowledge_base")
```

### Advanced Querying

```python
# Get detailed results
result = rag.query("What is machine learning?", k=10)  # Retrieve top 10 chunks

print(f"Answer: {result['answer']}")
print(f"Sources: {result['sources']}")

# Examine retrieved documents
for i, doc in enumerate(result['retrieved_docs']):
    print(f"Doc {i+1} (Score: {doc['score']:.3f}): {doc['text'][:100]}...")
```

### Configuration Options

```python
# Custom embedding model
rag = RAGSystem(
    gemini_api_key="your-key",
    embedding_model="all-mpnet-base-v2"  # More accurate but slower
)

# Custom chunking parameters
chunks = DocumentProcessor.chunk_text(
    text, 
    chunk_size=1500,  # Larger chunks for more context
    overlap=300       # More overlap for better continuity
)
```

## 🎛️ Configuration

### Supported Embedding Models
- `all-MiniLM-L6-v2` (default) - Fast and efficient
- `all-mpnet-base-v2` - Higher accuracy
- `all-distilroberta-v1` - Good balance of speed and accuracy

### Chunk Size Guidelines
- **Small docs (< 10 pages)**: 500-800 tokens
- **Medium docs (10-50 pages)**: 800-1200 tokens  
- **Large docs (> 50 pages)**: 1000-1500 tokens

### Retrieval Settings
- **Quick answers**: k=3-5
- **Comprehensive answers**: k=8-15
- **Research queries**: k=10-20

##  API Reference

### RAGSystem Class

#### `__init__(gemini_api_key, embedding_model="all-MiniLM-L6-v2")`
Initialize the RAG system.

#### `add_document(file_path)`
Add a PDF or DOCX document to the knowledge base.

#### `query(question, k=5)`
Query the knowledge base and get an AI-generated answer.

**Returns:**
```python
{
    'answer': str,           # Generated answer
    'sources': List[str],    # Source documents
    'retrieved_docs': List[Dict]  # Retrieved chunks with scores
}
```

#### `save_index(save_path)` / `load_index(load_path)`
Persist and restore your knowledge base.

### DocumentProcessor Class

#### `extract_text_from_pdf(file_path)` / `extract_text_from_docx(file_path)`
Extract text from documents.

#### `chunk_text(text, chunk_size=1000, overlap=200)`
Split text into overlapping chunks.

## Performance Tips

### Speed Optimization
- Use smaller embedding models for faster processing
- Reduce chunk overlap for larger knowledge bases
- Consider using FAISS GPU version for large-scale deployments

### Accuracy Optimization
- Use larger, more sophisticated embedding models
- Increase chunk overlap for better context
- Retrieve more chunks (higher k) for complex questions
- Fine-tune chunk sizes based on your document types

### Memory Management
- For large knowledge bases, consider using FAISS IVF indices
- Batch process documents to avoid memory issues
- Regularly save indices to prevent data loss

##  Troubleshooting

### Common Issues

**"No text extracted from file"**
- Check if PDF is text-based (not scanned images)
- Verify file permissions
- Try a different PDF reader if issues persist

**"API key not valid"**
- Ensure you're using the correct Gemini API key
- Check if the API key has proper permissions
- Verify your Google Cloud billing is enabled

**Memory errors with large documents**
- Reduce chunk size
- Process documents one at a time
- Consider using FAISS with disk-based storage

**Poor answer quality**
- Increase the number of retrieved chunks (k parameter)
- Try different embedding models
- Adjust chunk size and overlap
- Check if your documents contain the information you're asking about

##  Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Commit changes: `git commit -am 'Add feature'`
4. Push to branch: `git push origin feature-name`
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

##  Acknowledgments

- **Google Gemini** for powerful language generation
- **FAISS** for efficient similarity search
- **SentenceTransformers** for high-quality embeddings
- **Hugging Face** for the transformer ecosystem

##  Future Enhancements

- [ ] Support for more file formats (TXT, RTF, HTML)
- [ ] Web scraping capabilities
- [ ] Multi-language support
- [ ] Advanced filtering and metadata search
- [ ] REST API interface
- [ ] Web-based UI
- [ ] Integration with cloud storage (S3, Google Drive)
- [ ] Advanced retrieval strategies (hybrid search, re-ranking)

## Support

For questions, issues, or contributions:
- Open an issue on GitHub
- Check the troubleshooting section
- Review existing issues for solutions

---

