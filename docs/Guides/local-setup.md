# 💻 Local Setup Guide

### Prerequisites
*   Python 3.10+ installed
*   Ollama running locally (`ollama pull llama3.2` and `ollama pull mxbai-embed-large`)
*   Docker Engine active (for native storage containers)

### Setup Checklist
1. Clone your backend server framework and initialize virtual properties.
2. Fire up infrastructure layers using standard container initializations:
   ```bash
   docker run -d -p 6333:6333 qdrant/qdrant
