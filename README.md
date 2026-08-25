# RAG Mini Q&A Bot

A small Retrieval-Augmented Generation (RAG) question-answering system built in Python as part of the MLSA SRM recruitment task.

## Motivation

I was already interested in RAG and AI/ML research, but most of my understanding was conceptual rather than implementation-focused. This task gave me an opportunity to go from understanding the idea of RAG to actually building one from scratch.

I used AI as a learning and debugging partner throughout the process. Rather than treating it as a black-box solution, I built and tested the system step by step, including document loading, chunking, embeddings, retrieval, similarity scoring, local LLM generation, source citation, and handling questions that are outside the document set.

The process helped me understand not only how a RAG pipeline works, but also where it can fail and why retrieval quality matters.

## What This Project Does

The bot answers questions using information retrieved from a fixed set of NimbusNote documents.

Instead of sending a question directly to a language model, the system:

1. Loads the provided documents.
2. Splits them into smaller chunks.
3. Converts the chunks into embeddings using `sentence-transformers`.
4. Uses cosine similarity to retrieve relevant passages.
5. Passes the retrieved context to a local language model.
6. Generates an answer based on the retrieved information.
7. Displays the source document, chunk, similarity score, and retrieved passage.

If no relevant passage is found, the bot responds that it does not know based on the provided documents.

## RAG Pipeline

```text
                    User Question
                          |
                          v
                  Question Embedding
                          |
                          v
              Cosine Similarity Search
                          |
                          v
                Relevant Text Chunks
                          |
                          v
                    Local LLM
                          |
                          v
                Answer + Citations
```

## Technologies Used

- Python
- Google Colab
- `sentence-transformers`
- Sentence embeddings
- Cosine similarity
- Hugging Face Transformers
- Qwen2.5-0.5B-Instruct
- GitHub

## Document Set

The system retrieves information from the three provided documents:

- `01-getting-started.md`
- `02-pricing-and-plans.md`
- `03-troubleshooting.md`

## Example: Answer Found

**Question:**

> How often does NimbusNote sync?

**Answer:**

> NimbusNote syncs every 15 seconds while the app is in the foreground, and every 5 minutes in the background.

The system also shows the source document, chunk number, similarity score, and retrieved passage supporting the answer.

## Example: Answer Not Found

**Question:**

> What is the capital of France?

**Answer:**

> I don't know based on the provided documents.

This demonstrates that the system does not simply generate an answer when the required information is absent from the document set.

## Retrieval Demonstration

For a question such as:

> How often does NimbusNote sync?

the retriever identifies the relevant passage from:

```text
01-getting-started.md
```

and returns the corresponding chunk and similarity score before the answer is generated.

This makes the retrieval stage visible instead of hiding it behind a single LLM call.

## What I Learned

During development, I learned how the different parts of a RAG system connect:

- How documents are loaded and divided into chunks
- What embeddings represent and why they are useful for semantic search
- How cosine similarity can be used to compare a question with document chunks
- Why retrieval quality directly affects the final answer
- How grounding an LLM with retrieved context can reduce unsupported answers
- Why source citations are important in a document-based Q&A system
- How to test both questions that are covered and questions that are not covered by the documents

I also encountered and debugged issues during development, including model/API limitations and imperfect retrieval results. These experiments helped me understand that building a working RAG system involves evaluating each stage rather than assuming that a generated answer is automatically correct.

## Evaluation

I tested the system using both:

### 1. A question covered by the documents

The system retrieved the correct passage and produced a cited answer.

### 2. A question not covered by the documents

The system returned:

> I don't know based on the provided documents.

This tests the system's ability to avoid confidently answering questions that cannot be supported by the provided documents.

## Limitations

This is a small educational RAG system designed around the provided document set.

The retrieval system uses a relatively simple embedding + cosine similarity approach, so semantically related but less relevant passages can sometimes rank highly.

It is intentionally kept simple to make the retrieval and generation pipeline visible and understandable rather than hiding the process behind a high-level RAG framework.

## How to Run

The project was developed in Google Colab.

1. Open the `.ipynb` notebook in Google Colab.
2. Run the setup and document-loading cells.
3. Run the chunking and embedding steps.
4. Run the retrieval and RAG pipeline.
5. Start the interactive Q&A loop.
6. Ask questions about the provided documents.

## Project Structure

```text
rag-mini-qa-bot/
├── 01-getting-started.md
├── 02-pricing-and-plans.md
├── 03-troubleshooting.md
├── rag_mini_qa_bot.ipynb
└── README.md
```

## Future Improvements

Possible improvements include:

- Better chunking strategies
- More advanced retrieval/reranking
- Hybrid keyword + semantic retrieval
- A larger or stronger local language model
- A simple web interface
- More systematic retrieval evaluation

## Author

Built as part of the MLSA SRM RAG Mini Q&A Bot recruitment task.
