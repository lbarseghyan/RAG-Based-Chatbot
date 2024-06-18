# RAG Based Chatbot

The implementation of our RAG based chatbot includes several components that should be designed and integrated. These components are a PDF Processor, Embedder, Vector Database, Retriever, LLM Response Generation, and Chatbot Interface. Now, I will describe each component separately. 

## 1. PDF Processor
The first component of our RAG based chatbot is the PDF processor. It will be used to extract text data from the PDF documents.<br/>

**Tool Choice:** For PDF processing, I would use PyMuPDF (fitz). This Python library provides a robust API for extracting text, images, and metadata from PDFs, it is well-documented and efficient when handling various PDF structures and formats.

#### Challenges:
- Most installation manuals include a lot of images that may contain important information. We may integrate OCR (Optical Character Recognition) to extract text from images when necessary. This also will be helpful for cases when the manuals are scanned documents
- Large documents can be slow to process and load into memory. I think this is not an important issue because each manual is processed only once. But even if this is an issue, we can implement a chunking strategy. In this case, the PDF is processed in smaller sections. This will also be effective when doing text embedding. 


## 2. Embedder
Embedder will be used to convert text chunks into dense vector representations.<br/>

**Tool Choice:** I would choose OpenAI’s “text-embedding-3-large” model for generating embeddings. This model can effectively handle a wide range of textual data and has high performance in semantic understanding, which is important to understand and respond to user queries.
#### Challenges: 
- Embedding a large volume of text data will result in high API costs. To reduce the number of API calls we can split the manuals into smaller chunks, pre-process and clean the text and cache embeddings for previously processed documents.
- Embedding vector size may be too large for efficient storage and retrieval. By default, the length of the embedding vector will be 3072 for “text-embedding-3-large”. To make the ebedding vectors smaller, we can use dimensionality reduction techniques like PCA (Principal Component Analysis). PCA will reduce the size of embeddings while retaining most of the semantic information.

## 3. Vector Database
We will use vector database to store the embeddings and provide efficient retrieval based on similarity search.<br/>
**Tool Choice:** For vector database I would use “Pinecone”, which is known for its high performance and scalability. Pinecone is fast, supports real-time updates and efficient retrieval.
#### Challenges:
- When adding new manuals vector database should remain performant. For this we  should implement efficient indexing. The vector index should be regularly updated and optimized.
- We will have high storage requirements for large numbers of vectors. In this case we can optimize the embedding process to store only essential vectors.

## 4. LLM Response Generation
We should use an LLM model to generate human-like responses based on the retrieved information. This LLM model will be fine-tuned using our dataset, which we collected using the installation manuals.<br/>
**Tool Choice:** As an LLM model I will choose Llama 2, which is a 7–70 billion-parameter generative text model. This is powerful and open-source model, which demonstrates proficiency across various NLP tasks.
#### Challenges:
- In long conversations we should also keep the context. To do this we can Implement a context window management strategy that keeps track of the conversation history efficiently. 
- Llama 2 is not very fast and it may take around a minute to generate a responese. To reduce this time we can use caching strategies for common queries and pre-generate responses for frequently asked questions.

<br/>

## Example Questions
#### The Chatbot will be able to answer to the following questions:
1. My air conditioner is making a strange noise, and the airflow seems weaker than usual. What is the problem, and how to troubleshoot it?
2. How do I troubleshoot a recurring error code on my dishwasher that affects water drainage?
3. How can I ensure optimal performance of my air conditioner during winter months?
4. How do I connect and configure my smart water heater with a mobile app for remote control?
5. My tankless water heater is displaying error code E002. What does this indicate, and how can I resolve it?<br/>
All of the questions above will be successfully answered by the chatbot, if our training data includes the necessary information about the equipements mentioned in the query. Additionally, if our training dataset includes information about different models of the same equipment, it it would be better to mention the model name too. 


#### The Chatbot will fail to answer to the following questions accurately:
1. What are the local building codes and regulations regarding the installation of a gas water heater in my city?<br/>
**Reason:** Legal and regulatory information can vary widely by location and may require up-to-date knowledge of local laws and building codes, which are not typically included in installation manuals.
2. What are the best brands for air conditioners?<br/>
**Reason:** This is outside the scope of the provided manuals.
3. Can you predict when my water heater will fail?<br/>
**Reason:** Predicting future events based on limited information is beyond its capabilities.
4. What is the energy efficiency rating of all available air conditioners?<br/>
**Reason:** The chatbot cannot access external databases or aggregate data not present in the provided manuals.
5. Can you tell me the best water heater on the market?<br/>
**Reason:** The chatbot is not designed to provide market comparisons. It does not have information about all of the equipment available in the market.<br/>
All of the questions above will get a response by the chatbot, but the response will probably not be correct, based on the reasons mentioned above. 
