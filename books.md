## Google's PageRank and Beyond: The Science of Search Engine Rankings
Chapter 1: Introduction to Web Search Engines
- Information retrieval is the process of searching within a document collection for a particular information need (called a query)
- In the past, people learned valuable websites from other people's recommendation. All this changed in 1998 when **link analysis** hit the information retrieval scene. The most successful search engines began using link analyasis, a technique that exploited the additional information inherent in the hyperlink structure of the web to improve the quality of search results.
    1. PageRank link analysis algorithm used by Google
    2. HITs algorithm used by Teoma
- Traditional nonlinked information retrieval
    1. boolean model: operates by considering which keywords are present or absent in a document
        - pros:
            - simple to implement
            - query can be processed quickly and in parallel
            - scale well
        - cons:
            - no concept of a partial match (hard to deal with **synonymy** and **polysemy**)
            - sometimes require user to have knowledge about engine's specialized syntax in order to have better result
     2. Vector space model: transform textual data into numeric vectors and matrices, then employ matrix analysis techniques to discover key features and connections in the document collection (e.g. **LSI** engine).
        - pros:
            - it has relevance scoring: the vector space odel allows documents to partially match a query by assigning each document a number between 0 and 1 which can be interpreted as the likelihood of relevance to the query.
            - it has relevance feedback: information retrieval tuning technique. It allows the user to select a subset of the retrieved documents that are useful and then resubmit with this additional relevance feedback information.
        - cons:
            - hard to scale because need to compute distance between each document and the query.
     3. probabilistic model: 
            
