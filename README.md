# CS410-Project
This repository contains the codebase of the project 'Tech News Summarization through RAG' developed for CS410.

## Dataset

The dataset we're using is https://research.signal-ai.com/datasets/signal1m.html

It contains 1 million articles that are mainly English, but they also include non-English and multi-lingual articles.
However as each article is extremely large, we are using approximately 1500 arcticles because of computational limitations.

The dataset is in JSONL format where each line is a JSON object representing an article. 

Each article has the following fields:

- id: a unique identifier for the article
- title: the title of the article
- content: the textual content of the article (may occasionally contain HTML and JavaScript content)
- source: the name of the article source (e.g. Reuters)
- published: the publication date of the article
- media-type: either "News" or "Blog"


A summary of general statistics of the dataset is:-
- The number of individual unique sources are over 93k
- The dataset contains 265,512 Blog articles and 734,488 News articles
- The average length of an article is 405 words

## Installation

>Download the model binary
    
    Link - https://github.com/ollama/ollama/releases/tag/v0.3.2
>>Download the 'ollama-linux-amd64' binary and keep it in the same folder as the ipynb file
>
>

>Installation
    Install all the dependencies and libraries with the command 'pip install -r requirements.txt'

In a terminal, run the command to start ollama locally.
    
    ./ollama-linux-amd64 serve


## VectorDB Creation

We have used PineCone (https://www.pinecone.io/) for our VectorDB where we store high dimensional embeddings of our collection's documents as chunks helping efficient similarity searches for information retrieval. PineCone provides scalable and fast indexing and integrates seamlessly with existing python workflows.

It is populated by running the below command in the jupyter notebook (the command is currently commented since the values in vectorDB will be duplicated if command is run)

    populate_vectorDB(dataset)

### Creation of new Index if needed:

    1. Login/SignUp on PineCone
    
    2. Select 'Create an index'

    3. 
        Enter an index name.
        Enter dimensions.
            We have chosen 384 dimensions as the embedding model we are using (sentence-transformers/all-MiniLM-L6-v2) has 384 dimensions
        
        Choose metric
            We have chosen cosine as we are working with text data.

    4. Use index

        PINECONE_API_KEY = "63651404-1b59-483a-9c49-0b5c6b3aa9ee"
        pc = Pinecone(api_key = PINECONE_API_KEY)
        index_name = "news-articles"
        index = pc.Index(index_name)

    5. We can then push or query data from the vectorDB
        similar_docs = index.query(vector, top_k, include_metadata, filter)

        index.upsert()
        
## Execution

Run all the cells in the 'RAG.ipynb' jupyter notebook.