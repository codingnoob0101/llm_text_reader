LLM text reader for long text summarization 

Background:
    - passing huge amount(100k) of tokens into a prompt(Context Stuffing approach) is viable to modern LLM, but lead to potential problems
    - failures of 'Context stuffing' approach:
        a) 'Lost in the middle' Phenoenon
            - LLM pay close attention to the beginning(primacy bias) and the end(recency bias) of the prompt
            - mathematically difficult for model to maintain attention over the 'middle'

        b) 'Resolution' Problem
            - summarization is a lossy compression task
            - often result in generic platitudes rather than specific insights

        c) Hallucination 
            - types: Factual errors, Fabricated content, Nonsensical outputs
            - causes: Insufficient or biased training data, Overfitting, Faulty model architecture, Generation methods

workflow:
1) Semantic Chunking
    - sliding window with an embedding model (bge-small)
    - measure cosine similarity between sentences
    - sudden drop of cosine similarity indicates topic change -> split to another chunk

2) Local summarization
    - prompt the LLM in parallel
    - sample prompt: summarize the following specific section 
    - result in high resolution mini-summaries and less 'lost in the middle' (due to shorter input)

3) Cluster related local summaries 
    - K-means clustering to group related local summaries
    - create thematically organized summary rather than chronological ones
    - resulting in few clusters

4) Global synthesis
    - pass each cluster into the LLM for cluster summaries

5) Final summary
    - take all cluster summaries and perform one final pass

Model evaluation:
    - 