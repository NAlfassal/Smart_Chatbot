# SANAD - Cosmetics Compliance Chatbot

An intelligent Arabic chatbot for querying Saudi Food and Drug Authority (SFDA) cosmetics regulations and banned substances using Retrieval Augmented Generation (RAG).

## Features

- 🤖 Arabic language support with intelligent text processing
- 📚 Multi-source knowledge base (PDF, JSON, JSONL, Excel)
- 🔍 Smart retrieval of regulations and banned substances
- 💬 Natural language question answering
- 🎯 Direct article lookup by number
- 🌐 Web interface using Gradio
- 🧠 Vector embeddings using multilingual-e5-large
- ⚡ Streaming responses for better UX

## System Architecture

```
┌─────────────────┐
│  User Query     │
│  (Arabic)       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Gradio UI      │
│  (Web Interface)│
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  SFDAChatbot    │
│  (RAG Logic)    │
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌────────┐ ┌──────────┐
│Chroma  │ │OpenRouter│
│Vector  │ │LLM       │
│Store   │ │(DeepSeek)│
└────────┘ └──────────┘
```

## Project Structure

```
smart_chatbot/
├── config.py                      # Centralized configuration
├── app_gradio.py                  # Original Gradio application
├── app_gradio_improved.py         # Improved version with better structure
├── ingest_database.py             # Original database ingestion
├── ingest_database_improved.py    # Improved version with logging
├── build_chroma_from_json.py      # Build ChromaDB from JSON
├── ingest_from_json_dict.py       # Ingest from JSON dictionary format
├── requirements.txt               # Python dependencies
├── .env.example                   # Environment variables template
├── .gitignore                     # Git ignore rules
├── knowledge/                     # Knowledge base directory
│   ├── sfda_articles.json        # SFDA regulations (Arabic)
│   ├── banned_list.json          # Banned substances list
│   └── *.xlsx                    # Excel files with data
├── chroma_db/                     # ChromaDB vector store (generated)
└── scripts/                       # Utility scripts
    ├── clean_flat_json.py
    ├── clean_text.py
    ├── ingest_to_chroma.py
    ├── prepare_chunks.py
    ├── query_filtered.py
    └── rag_answer.py
```

## Prerequisites

- Python 3.9 or higher
- OpenRouter API key (for LLM access)
- 4GB+ RAM recommended
- Windows/Linux/MacOS

## Installation

### 1. Clone the repository

```bash
git clone <repository-url>
cd smart_chatbot
```

### 2. Create virtual environment

```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# Linux/Mac
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

```bash
# Copy the example file
cp .env.example .env

# Edit .env and add your API key
# OPENROUTER_API_KEY=your_api_key_here
```

Get your OpenRouter API key from: https://openrouter.ai/keys

### 5. Prepare knowledge base

Place your knowledge files in the `knowledge/` directory:
- `sfda_articles.json` - SFDA regulations in JSON format
- `banned_list.json` - Banned substances list
- `*.xlsx` - Excel files with cosmetics data

### 6. Ingest data into vector store

```bash
# Use the improved version with logging
python ingest_database_improved.py

# Or use the original version
python ingest_database.py
```

This will:
- Load documents from `knowledge/` directory
- Process and chunk the text
- Generate embeddings using multilingual-e5-large
- Store in ChromaDB vector database

### 7. Run the application

```bash
# Use the improved version
python app_gradio_improved.py

# Or use the original version
python app_gradio.py
```

The application will:
- Load the vector store
- Initialize the LLM
- Launch a Gradio web interface
- Provide a shareable public URL

## Usage

### Web Interface

1. Open the Gradio interface in your browser
2. Select the search source:
   - **لوائح التجميل (PDF)** - SFDA regulations
   - **محظورات التجميل** - Banned substances
   - **الكل** - All sources
3. Type your question in Arabic
4. View the streaming response with sources

### Example Queries

**Regulations:**
- "ما هي المادة الرابعة؟" (What is Article 4?)
- "اذكر التزامات المُدرج في النظام" (List the obligations of the registrant)

**Banned Substances:**
- "هل Mercury محظور في التجميل؟" (Is Mercury banned in cosmetics?)
- "اذكر لي 5 مواد محظورة تبدأ بحرف M" (List 5 banned substances starting with M)

## Configuration

All configuration is centralized in `config.py`. You can override settings using environment variables in `.env`:

### Database Configuration
- `CHROMA_PATH` - Vector store directory (default: chroma_db)
- `COLLECTION_NAME` - Collection name (default: sfda_collection)
- `DATA_PATH` - Knowledge base directory (default: knowledge)

### Model Configuration
- `EMBEDDING_MODEL` - Embedding model (default: intfloat/multilingual-e5-large)
- `EMBEDDING_DEVICE` - Device for embeddings (default: cpu)
- `LLM_MODEL` - LLM model (default: deepseek/deepseek-chat)
- `LLM_TEMPERATURE` - Response randomness (default: 0.0)
- `LLM_MAX_TOKENS` - Maximum response length (default: 700)

### RAG Configuration
- `RETRIEVAL_K` - Number of documents to retrieve (default: 8)
- `CHUNK_SIZE` - Text chunk size (default: 1000)
- `CHUNK_OVERLAP` - Chunk overlap (default: 150)
- `BATCH_SIZE` - ChromaDB batch size (default: 2000)

### Application Configuration
- `DEBUG` - Enable debug mode (default: False)

## Key Improvements in Enhanced Version

### Code Quality
✅ Type hints on all functions
✅ Comprehensive docstrings
✅ Proper error handling with try-catch blocks
✅ Logging system for debugging
✅ Class-based organization

### Configuration
✅ Centralized config.py
✅ Environment variable support
✅ Configuration validation

### Error Handling
✅ Graceful error recovery
✅ User-friendly error messages
✅ Detailed logging for debugging

### Performance
✅ Efficient batch processing
✅ Optimized text processing
✅ Connection pooling ready

### Maintainability
✅ Separation of concerns
✅ Reusable components
✅ Clear code structure
✅ Documentation

## Troubleshooting

### API Key Error
```
ValueError: OPENROUTER_API_KEY not found in .env file
```
**Solution:** Create a `.env` file and add your OpenRouter API key.

### ChromaDB Not Found
```
FileNotFoundError: Chroma database not found
```
**Solution:** Run `python ingest_database_improved.py` to create the vector store.

### Empty Results
**Solution:**
- Check if knowledge files exist in `knowledge/` directory
- Verify the vector store has documents (check logs)
- Try broader queries

### Encoding Issues
**Solution:** Ensure all files are saved in UTF-8 encoding.

### Memory Issues
**Solution:**
- Reduce `CHUNK_SIZE` in config
- Reduce `BATCH_SIZE` for ingestion
- Use a smaller embedding model

## Development

### Adding New Features

1. Create a feature branch
2. Implement with proper type hints and docstrings
3. Add error handling
4. Test thoroughly
5. Update documentation

### Code Style

- Follow PEP 8 guidelines
- Use type hints
- Add docstrings (Google style)
- Keep functions focused and small
- Use meaningful variable names

### Testing

```bash
# Run the application in debug mode
DEBUG=True python app_gradio_improved.py
```

## Performance Optimization

### Embedding Model
- Current: `intfloat/multilingual-e5-large` (1.12GB)
- Alternative: `intfloat/multilingual-e5-base` (560MB, faster)
- Alternative: `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2` (420MB, fastest)

### LLM Model
- Current: `deepseek/deepseek-chat` (cost-effective)
- Alternative: `gpt-4o-mini` (faster, more expensive)
- Alternative: `gpt-3.5-turbo` (cheaper, less capable)

## Security Considerations

- ✅ API keys in environment variables
- ✅ .env file in .gitignore
- ⚠️ Add input validation for production
- ⚠️ Add rate limiting for public deployment
- ⚠️ Sanitize user inputs


## Support

For issues and questions:
- Create an issue on GitHub
- Contact: [your-email@example.com]

## Acknowledgments

- SFDA for regulations data
- LangChain for RAG framework
- Gradio for UI framework
- HuggingFace for embedding models
- OpenRouter for LLM access

## Changelog

### Version 2.0 (Improved)
- Added centralized configuration
- Improved error handling and logging
- Better code organization with classes
- Type hints and comprehensive docstrings
- Enhanced documentation

### Version 1.0 (Original)
- Basic RAG implementation
- Gradio web interface
- Multi-source document support
- Arabic text processing
