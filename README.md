# My RAG with Typesense

A hands-on project to understand and implement Retrieval-Augmented Generation (RAG) using Python, LangChain, and Typesense.

The project explores how to connect Typesense with a RAG pipeline, perform document ingestion, generate embeddings, store and retrieve information, and use semantic search to find relevant content.

## What is RAG?

Retrieval-Augmented Generation (RAG) retrieves relevant information from a document collection and provides it to a language model as context for answering a question. This lets a model use information from documents that was not part of its training data.

## What is Typesense?

Typesense is an open-source search engine that supports fast, typo-tolerant search, filtering, faceting, and vector search.

In a RAG application, Typesense can be used to store document chunks and their vector embeddings, allowing the system to retrieve relevant information efficiently when a user submits a query.

## Technologies

- Python 3.12+
- UV for dependency and virtual-environment management
- Jupyter Notebook and VS Code
- Typesense for search and vector storage
- Typesense Python Client
- LangChain for document loading and vector store integration
- LangChain Text Splitters for splitting documents into chunks
- Hugging Face Sentence Transformers for generating embeddings
- Python-dotenv for environment variable management
- Groq API configuration for the LLM integration

## Project Structure

```text
my-rag-with-typesense/
│
├── data/
│   ├── json_files/
│   │   └── books.jsonl
│   │
│   └── text_files/
│       └── test.txt
│
├── notebook/
│   ├── document.ipynb
│   └── typesense.ipynb
│
├── src/
│   └── my_rag_from_scratch/
│       └── __init__.py
│
├── .env.example
├── .gitignore
├── .python-version
├── LICENSE
├── pyproject.toml
├── README.md
└── uv.lock
```

The repository contains two Jupyter notebooks. This README focuses on `notebook/typesense.ipynb`, which explores Typesense integration and RAG.

## Prerequisites

- Python 3.12 or newer
- Git
- UV
- Visual Studio Code
- A Typesense Cloud account with a running cluster
- A Groq API key

The notebook downloads the Hugging Face embedding model the first time it is used, so an internet connection is required.

## Install UV on Windows

In PowerShell, install UV using WinGet:

```powershell
winget install --id=astral-sh.uv -e
```

Open a new PowerShell terminal and verify the installation:

```powershell
uv --version
```

For other installation options and operating systems, see the [official UV installation guide](https://docs.astral.sh/uv/getting-started/installation/).

## Clone the Repository and Install Dependencies

In PowerShell:

```powershell
git clone https://github.com/Cyber-Infernox/my-rag-with-typesense.git

Set-Location my-rag-with-typesense

uv sync
```

`uv sync` creates the virtual environment (`.venv/`) if needed and installs the dependencies from `pyproject.toml` using the committed `uv.lock`.

The Typesense notebook also imports `HuggingFaceEmbeddings` from `langchain_huggingface`. If this dependency is not yet included in your project, install it using:

```powershell
uv add langchain-huggingface
```

## Open in VS Code and Configure Python

Open the repository from its root directory:

```powershell
code .
```

If the `code` command is unavailable, open VS Code and use File > Open Folder to select the cloned repository.

Install the Microsoft Python and Jupyter extensions:

```powershell
code --install-extension ms-python.python

code --install-extension ms-toolsai.jupyter
```

Select the project interpreter:

1. Press `Ctrl+Shift+P`.
2. Run `Python: Select Interpreter`.
3. Choose `.venv\Scripts\python.exe`.

Open `notebook/typesense.ipynb`. Use Select Kernel in the notebook toolbar and choose the Python environment that points to the same `.venv`.

## Environment Variables and API Keys

The project uses environment variables to securely manage API keys and Typesense connection details.

Create a local `.env` file in the repository root using `.env.example` as a template.

Add the following variables to your `.env` file:

```env
GROQ_API_KEY=your_groq_api_key_here

TYPESENSE_HOST=your_typesense_host
TYPESENSE_PORT=443
TYPESENSE_PROTOCOL=https
TYPESENSE_API_KEY=your_typesense_api_key
TYPESENSE_COLLECTION_NAME=rag-docs
```

Replace the placeholder values with your actual credentials.

- `GROQ_API_KEY`: Your Groq API key.
- `TYPESENSE_HOST`: Your Typesense Cloud cluster hostname.
- `TYPESENSE_PORT`: The Typesense connection port (443 for Typesense Cloud).
- `TYPESENSE_PROTOCOL`: The connection protocol (https for Typesense Cloud).
- `TYPESENSE_API_KEY`: Your Typesense API key.
- `TYPESENSE_COLLECTION_NAME`: The collection used for storing document chunks in the RAG example. The default is `rag-docs`.

**Important:** Never commit your `.env` file or expose actual API keys in your notebooks, code, or GitHub repository. The `.env` file is ignored by Git.

## Notebook Execution Order

The main focus of this project is `notebook/typesense.ipynb`.

Restart the notebook kernel and execute the cells from top to bottom, in the order displayed.

The notebook covers the following:

### 1. Connecting to Typesense

- Importing the Typesense Python client.
- Loading connection details from environment variables.
- Creating a Typesense client.
- Establishing a connection with Typesense Cloud.

### 2. Creating a Collection in Typesense

- Defining a collection schema named `books`.
- Configuring fields such as title, authors, publication year, ratings count, and average rating.
- Specifying field types and faceting options.
- Creating the collection using the Typesense client.

### 3. Importing Data into Typesense

- Loading book records from `data/json_files/books.jsonl`.
- Importing the JSONL data into the `books` collection.
- Understanding how documents are stored in Typesense.

### 4. Performing Search Operations

- Creating search parameters.
- Searching books using their titles and authors.
- Sorting search results using ratings count.
- Applying filters based on publication year.
- Understanding how Typesense returns matching documents and search results.

### 5. Integrating LangChain with Typesense

- Importing LangChain document loaders and vector store integrations.
- Loading text documents using `TextLoader`.
- Splitting documents into smaller chunks using `CharacterTextSplitter`.
- Configuring chunk size and overlap.
- Initializing Hugging Face embeddings.

### 6. Creating a RAG Vector Store

- Connecting LangChain with Typesense.
- Configuring the Typesense collection and connection parameters.
- Generating embeddings for document chunks.
- Storing documents and their embeddings in Typesense using `Typesense.from_documents()`.

### 7. Retrieving Relevant Documents

- Performing similarity search using a natural-language query.
- Retrieving relevant document chunks from Typesense.
- Creating a retriever using `as_retriever()`.
- Using `retriever.invoke()` to retrieve relevant documents for a query.

The notebook uses relative paths such as `../data/json_files/books.jsonl` and `../data/text_files/test.txt`. Make sure the notebook's working directory is the repository's `notebook/` directory.

**Note:** The current notebook demonstrates Typesense search, vector storage, similarity search, and document retrieval. Although it checks for a Groq API key and introduces the Groq integration in its heading, the current implementation does not yet demonstrate generating a final LLM answer using Groq.

## Troubleshooting

- **`uv` is not recognized:** Open a new terminal after installing UV and check `uv --version`.
- **A Python package cannot be imported:** Run `uv sync` from the repository root and ensure the correct `.venv` interpreter and notebook kernel are selected. If `langchain_huggingface` is missing, run `uv add langchain-huggingface`.
- **Typesense connection error:** Check your Typesense hostname, API key, port, and protocol in the `.env` file.
- **Missing environment variables:** Ensure the `.env` file exists in the repository root and contains the required Typesense configuration.
- **Collection already exists:** Typesense returns an error if you try to create a collection that already exists. Reuse the existing collection or remove it before creating it again.
- **A data file cannot be found:** Confirm that the notebook's working directory is `notebook/`. The relative paths start with `../data`.
- **The embedding model cannot load:** Check your internet connection and rerun the embedding initialization cell. The first run downloads the model.

## Contributing

Contributions and pull requests are welcome.

## License

This project is licensed under the MIT License.
