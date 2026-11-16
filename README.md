# My RAG From Scratch

A hands-on project to understand and implement Retrieval-Augmented Generation (RAG) from scratch using Python.

The project is currently centered on one Jupyter notebook. It demonstrates document loading, text splitting, embeddings, ChromaDB storage and retrieval, and optional answer generation with Groq. The PDF is loaded as a separate example; the current vector retrieval flow indexes the text files.

## What is RAG?

Retrieval-Augmented Generation (RAG) retrieves relevant information from a document collection and provides it to a language model as context for answering a question. This lets a model use information from documents that was not part of its training data.

## Technologies

- Python 3.12+
- UV for dependency and virtual-environment management
- Jupyter Notebook and VS Code
- LangChain document loaders, document types, and text splitters
- PyMuPDF for loading PDF files
- Sentence Transformers for embeddings
- ChromaDB for persistent vector storage and retrieval
- Groq via LangChain for the optional LLM response examples

## Project structure

```text
my-rag-from-scratch/
├── data/
│   ├── pdf_files/
│   │   └── sample.pdf
│   └── text_files/
│       ├── machine_learning.txt
│       └── python_intro.txt
├── notebook/
│   └── document.ipynb
├── src/
│   └── my_rag_from_scratch/
│       └── __init__.py
├── .env.example
├── .gitignore
├── .python-version
├── LICENSE
├── pyproject.toml
├── README.md
└── uv.lock
```

The notebook creates `data/vector_store/` when it runs. The vector database and the local `.venv/` are generated files and are not tracked by Git.

## Prerequisites

- Python 3.12 or newer
- Git
- UV
- Visual Studio Code

The notebook downloads the `all-MiniLM-L6-v2` embedding model the first time it is used, so an internet connection is needed. The final Groq-backed cells additionally require a Groq API key and network access to the Groq service. The document-loading, embedding, and retrieval examples do not require that API key.

## Install UV on Windows

In PowerShell, install UV with WinGet:

```powershell
winget install --id=astral-sh.uv -e
```

Open a new PowerShell terminal, then verify the installation:

```powershell
uv --version
```

For other installation options and operating systems, see the [official UV installation guide](https://docs.astral.sh/uv/getting-started/installation/).

## Clone the repository and install dependencies

In PowerShell:

```powershell
git clone https://github.com/Cyber-Infernox/my-rag-from-scratch.git
Set-Location my-rag-from-scratch
uv sync
```

`uv sync` creates `.venv/` if needed and installs the dependencies from `pyproject.toml` using the committed `uv.lock`. These files are the dependency source of truth; do not run `uv init` in this existing project.

## Open in VS Code and configure Python

Open the repository from its root:

```powershell
code .
```

If the `code` command is unavailable, open VS Code and use **File > Open Folder** to select the cloned repository.

Install the Microsoft **Python** and **Jupyter** extensions. You can also install them from PowerShell:

```powershell
code --install-extension ms-python.python
code --install-extension ms-toolsai.jupyter
```

Select the project interpreter:

1. Press `Ctrl+Shift+P`.
2. Run **Python: Select Interpreter**.
3. Choose `.venv\Scripts\python.exe`.

Open `notebook/document.ipynb`. Use **Select Kernel** in the notebook toolbar and choose the Python environment that points to the same `.venv`.

## Notebook execution order

There is one notebook, `notebook/document.ipynb`; there are no other notebooks to run first. Restart its kernel and run all cells from top to bottom, in the order displayed. The notebook proceeds through:

1. Creating a LangChain `Document` example.
2. Writing the sample text files under `data/text_files/`.
3. Loading a text file and then the text-file directory.
4. Loading the sample PDF into a separate variable.
5. Defining the text splitter.
6. Initializing the embedding model and ChromaDB vector store.
7. Splitting and embedding the text documents and adding them to ChromaDB.
8. Defining and using the retriever.
9. Configuring Groq and running the simple and advanced RAG examples.

The notebook uses paths such as `../data/text_files`, so its working directory must be the repository's `notebook/` directory. If a data file is reported missing, check the notebook's working directory before changing the paths.

To run the Groq examples, create a local `.env` file from the template if one does not already exist:

```powershell
if (-not (Test-Path .env)) {
    Copy-Item .env.example .env
}
```

Replace the placeholder `GROQ_API_KEY` value in `.env`, or keep the key already present there. Do not overwrite an existing `.env`, commit it, or paste a real key into a notebook cell or notebook output. The `.env` file is ignored by Git.

## Troubleshooting

- **`uv` is not recognized:** Open a new terminal after installing UV, then check `uv --version`.
- **A Python package cannot be imported:** From the repository root, run `uv sync`, then select `.venv\Scripts\python.exe` as both the VS Code interpreter and notebook kernel.
- **A data file cannot be found:** Confirm the notebook's working directory is `notebook/`; its relative paths start with `../data`.
- **The embedding model cannot load:** Check your internet connection and rerun the model-loading cell. The first run downloads the model.
- **Groq reports a missing API key:** If `.env` does not exist, create it from `.env.example`; otherwise update the existing file. Enter a valid `GROQ_API_KEY` and restart the notebook kernel before running the Groq cells.
- **You want a fresh vector store:** Stop the notebook kernel and remove the generated `data/vector_store/` directory before running the notebook again. It will be recreated from the notebook's current input documents.

## Contributing

Contributions and pull requests are welcome.

## License

This project is licensed under the [MIT License](LICENSE).
