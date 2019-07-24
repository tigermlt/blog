### Use TF-IDF to compute query and document similarity
[TF-IDF](https://janav.wordpress.com/2013/10/27/tf-idf-and-cosine-similarity/)
  The idea is to: 
    - compute term frequency *for each term in each document*
    - compute inverse document frequency *for each term in all documents and query* as a weight for each term
    - compute tf-idf for each document and the query
    - use cosine similarity as a metric to compute query-document similarity
  
