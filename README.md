# Motive Hackathon: RAG (Retrieval-Augmented Generation) for text retrival.

## Overview
This project implements a Retrieval-Augmented Generation (RAG) pipeline to enhance the quality of responses by combining retrieval-based techniques with generative models.

## Table of Contents
1. [Chunking Technique](#chunking-technique)
2. [Vector Database Creation](#vector-database-creation)
3. [Retrieval Technique](#retrieval-technique)
4. [Final Response Generation](#final-response-generation)

---

## Chunking Technique

The input data followed a fixed format, as shown below:

```
METADATA:
categories: telling
tags: dashcam  
CONTENT: Dashcams, or dashboard cameras, are devices mounted on a vehicle's dashboard or windshield to record video footage of the road and surroundings. They are commonly used for safety, evidence collection in case of accidents, and monitoring driving behavior. Modern dashcams often include features like GPS tracking, night vision, and cloud storage for enhanced functionality. 
TITLE: Dashcam
```

Initially, only the paragraph inside the `CONTENT` field was considered for chunking. However, the results were suboptimal. Consequently, the entire section, including metadata, was treated as a single chunk for better performance.

## Vector Database Creation

The vector database was built using **FAISS** with the following considerations:
- **Embedding Model**: `cohere.embed-multilingual-v3`, capable of handling multiple languages.
- **Data Storage**: Each entry included the URL of the page, the data, and the vector embeddings.
- **Indexing**: Vector representations of the chunks were indexed and stored using FAISS.

## Retrieval Technique

The retrieval process involved the following steps:
- **Similarity Metric**: Cosine similarity was used to measure relevance.
- **Query Expansion**: The input query was passed to an LLM to generate three additional related questions using a predefined prompt (`prompt_gen.txt`). Keywords for all questions were also extracted.
- **Ranking**: All retrieved results were ranked, and the top three were returned.

## Final Response Generation

The final response was generated using the following approach:
- **Generative Model**: `anthropic.claude-3-sonnet-20240229-v1`.
- **Prompt**: A standard prompt provided by Anthropic was used, as defined in `prompt.txt`.

---

## Future Improvements

- The current RAG pipeline is a standard implementation, leaving room for experimentation in all stages.
- Prompt optimization for the final generation was not performed. The default prompt suggested by Anthropic was used.
- Manual inspection of the dataset and tuning the retrieval prompts could improve the accuracy of retrieved results. This would ensure that the pipeline retrieves the most relevant text effectively.
- Further experimentation with chunking strategies and embedding models could enhance overall performance.
