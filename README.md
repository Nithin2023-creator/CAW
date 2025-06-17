##  API DOCUMENTATION
                                                                       


 Overview:
This is a document processing pipeline that allows users to:
- Upload and process various document types (PDF, DOCX, TXT, HTML, Markdown)
- Generate embeddings at document, section, and chunk levels
- Store and query documents using Pinecone vector database
- Get AI-powered responses using Google's Gemini model
    
     API KYES REQUIRED:
  gemini_api_key = "YOUR_GEMINI_API_KEY"
pinecone_api_key = "YOUR_PINECONE_API_KEY"
pinecone_index_name = "YOUR_INDEX_NAME"
Required Python packages:
pip install streamlit langchain langchain-community langchain-google-genai 
pip install langchain-pinecone pinecone-client google-generativeai 
pip install PyPDF2 python-docx markdown beautifulsoup4 unstructured






core components:
1. Document Processing
DOCUMENT LOADER:
def load_document(file_path, file_type):
    """
    Loads documents based on file type
    Supported types: pdf, docx, txt, html, md
    Returns: List of document chunks
    """
Chunking Strategies:
Three chunking strategies available:
1. semantic_chunking(docs, chunk_size, chunk_overlap)
2. sliding_window_chunking(docs, chunk_size, chunk_overlap)
3. dynamic_chunking(docs, file_type)
2. Embedding Generation
def generate_embeddings(docs, sections, chunks, file_name, file_type):
    """
    Generates hierarchical embeddings at three levels:
    - Document level
    - Section level
    - Chunk level
    
    Returns: Dictionary containing embeddings and metadata
    """
3. Vector Storage (Pinecone):
def store_in_pinecone(embeddings_data, pc, index_name):
    """
    Stores embeddings in Pinecone with metadata
    Namespace: "chunks"
    """

def query_pinecone(query, pc, index_name, top_k=3):
    """
    Queries Pinecone for similar content
    Returns: Top k most similar results
    """
4. AI Response Generation
def get_gemini_response(query, retrieved_content):
    """
    Generates AI response using Gemini model
    Model: gemini-1.5-flash
    Returns: Generated response text
    """
5. Interaction Tracking
def store_interaction(query, pc, index_name):
    """
    Stores user queries in Pinecone
    Namespace: "interactions"
    """

def store_feedback(query_id, feedback, rating, pc, index_name):
    """
    Stores user feedback
    Namespace: "feedback"
    """

def store_query_answer(query, answer, pc, index_name):
    """
    Stores query-answer pairs
    Namespace: "query_answer_pairs"
    """
Usage flow:
1. Document Upload
   - Upload document through Streamlit interface
   - Supported formats: PDF, DOCX, TXT, HTML, Markdown

2. Document Processing
   - Document is split into sections and chunks
   - Embeddings are generated at multiple levels
   - Data is stored in Pinecone

3. Querying
   - Enter query in the search interface
   - System retrieves relevant content from Pinecone
   - Gemini generates response based on retrieved content

4. Feedback
   - Users can provide feedback on results
   - Feedback is stored for future improvements

Deployment:

The application is deployed using Streamlit and ngrok:
python
# Start Streamlit
streamlit run app.py --server.port 8501

# Start ngrok tunnel
ngrok http 8501

Important Notes:

1. The application requires valid API keys for:
   - Google Gemini AI
   - Pinecone

2. The Pinecone index should be created with:
   - Dimension: 768
   - Metric: cosine
   - Cloud: AWS
   - Region: us-east-1

3. The application uses multiple namespaces in Pinecone:
   - "chunks" for document chunks
   - "interactions" for user queries
   - "feedback" for user feedback
   - "query_answer_pairs" for Q&A pairs

4. The application maintains session state using Streamlit's session state for user tracking.

This documentation covers the main components and functionality of the document processing pipeline. The application is designed to be run in Google Colab, with Streamlit serving as the web interface and ngrok providing public access to the application.




