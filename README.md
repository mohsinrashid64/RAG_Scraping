# Web Scrapping Using RAG

> **TLDR:**
> This Jupyter Notebook extracts specific HTML tags from an HTML document using a Language Model (LLM). It loads the HTML, splits it into chunks, processes it with Ollama’s nuextract embeddings, stores it in PostgreSQL via PGVector, and uses a retrieval chain to search and extract relevant HTML content based on a custom prompt.

## Getting Started
These instructions will help you set up and run the project locally.

### Setting Up Python Virtual Environment
Ensure Python is installed. We recommend version 3.9.

1. Open your terminal and navigate to your project directory:
    ```sh
    cd path/to/your-project-directory
    ```

2. Set up a virtual environment:
    ```sh
    python3 -m venv .venv
    ```

3. Create a file named `requirements.txt` in the root of your project, listing the necessary libraries.

4. Activate the virtual environment:
   - **Windows**:
     ```sh
     .\.venv\Scriptsctivate
     ```
   - **MacOS/Linux**:
     ```sh
     source .venv/bin/activate
     ```

5. Upgrade pip within the environment:
    ```sh
    .venv\Scripts\python.exe -m pip install --upgrade pip
    ```

### Installing Dependencies
Once your virtual environment is activated, install the required dependencies:
```sh
pip install -r requirements.txt
```

### Using Ollama
1. Download and install Ollama by clicking [here](https://ollama.com/download).
2. To set up the nuextract model, download it by visiting this [link](https://ollama.com/library/nuextract) OR Run the following command in your terminal to install the model:
    ```sh
    ollama run nuextract
    ```


### Setup PgVector

> **Note:** If you encounter any issues setting up PgVector, please refer to this helpful [video tutorial](https://www.youtube.com/watch?v=FDBnyJu_Ndg&ab_channel=BugBytes).

#### 1. Pull the Docker Image
Start by pulling the official PgVector Docker image:
```bash
docker pull pgvector/pgvector:pg17
```

#### 2. Run the Docker Container
Run the Docker container and set your own PostgreSQL password (make sure to remember it):
```bash
docker run -d --name pgvector-demo-test -e POSTGRES_PASSWORD=<YOUR_PASSWORD> -p 5432:5432 pgvector/pgvector:pg17
```
This will start a PostgreSQL container with PgVector installed, and you'll use `<YOUR_PASSWORD>` as the password for the `postgres` user.

#### 3. Connect to PgVector Using PgAdmin
- Open **PgAdmin** and create a new server connection:
  - **Name:** Choose any name for the connection.
  - **Hostname:** `localhost`
  - **Port:** `5432`
  - **Username:** `postgres`
  - **Password:** Enter the password you provided when running the Docker container.

- Create a new database (e.g., `vector_db`) within PgAdmin.

#### 4. Sample Connection String
To connect to the PgVector instance from your project, use the following connection string format:
```
postgresql+psycopg2://postgres:<YOUR_PASSWORD>@localhost:5432/vector_db
```

- Replace `<YOUR_PASSWORD>` with the password you set earlier.
- Create a `.env` file and add this connection string in the file like this:
	```
	PGVECTOR_CONNECTION_STRING="<YOUR_CONNECTION_STRING>"
	```


### Running the Application

#### 1. Prepare Your HTML File
The provided Jupyter Notebook expects a file named `sample.html`. You can use this file or replace it with your own HTML file containing the content you want to extract.

####  2. Run the Jupyter Notebook
Open the Jupyter Notebook and execute the cells in sequence:
1. **Load Libraries**: Import necessary libraries for text processing, LLM handling, and scraping.
2. **Setup LLM and Embeddings**: The code uses Ollama's `nuextract` model for language processing.
3. **Load HTML Content**: Load the HTML file (`sample.html`), which is used as input for scraping.
4. **Text Splitting**: Split the HTML text into smaller chunks for more efficient processing.
5. **PGVector Setup**: Connect to PostgreSQL and store the vectorized documents.
6. **Retrieve and Extract HTML Tags**: Use the retrieval chain to scrape the HTML tags based on your input query.
