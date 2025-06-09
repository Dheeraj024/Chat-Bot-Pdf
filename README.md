# 🤖 RAG-based PDF Chatbot with LangChain, FAISS, and HuggingFace

Welcome to the official repository for a **Retrieval-Augmented Generation (RAG)** powered chatbot that lets you upload PDF documents and query them conversationally. Built using **LangChain**, **FAISS**, **HuggingFace** models, and deployed via **Streamlit**.

---

## 🎡 Live Demo

Try it now: [📅 Chat with your PDF](https://chat-bot-pdf.streamlit.app/)

---

## ✅ Features

* 📄 **PDF Document Ingestion**
* 🏛️ **Semantic Indexing** with FAISS + MiniLM Embeddings
* 🔍 **Contextual Retrieval** using LangChain Retriever
* 🌐 **Natural Language Answer Generation** with Mixtral 8x7B via HuggingFace
* 📈 **Modular Chain Construction** for easy extension
* 📊 RAGAS-compatible for performance evaluation (planned)

---

## 📚 Theory: What is RAG?

**RAG (Retrieval-Augmented Generation)** is an architecture that improves generative responses by providing relevant retrieved context from an external knowledge base.

```
Your Query ➔ Retriever ➔ Top-K Chunks ➔ Prompted LLM ➔ Response
```

This method avoids hallucinations and enables querying large documents efficiently.

---

## ⚙️ Tech Stack

| Component      | Tool / Model                          |
| -------------- | ------------------------------------- |
| Language Model | Mixtral 8x7B Instruct via HuggingFace |
| Embeddings     | Sentence-Transformers (MiniLM-L6-v2)  |
| Vector DB      | FAISS                                 |
| LLM Framework  | LangChain                             |
| UI             | Streamlit                             |
| Deployment     | Streamlit Cloud                       |

---

## 🔧 Setup Instructions

```bash
# 1. Clone the repo
$ git clone https://github.com/<your-username>/rag-pdf-chatbot.git
$ cd rag-pdf-chatbot

# 2. Install dependencies
$ pip install -r requirements.txt

# 3. Set environment variables
Create a .env file:
OPENAI_API_KEY=<your_key_if_using_openai>
HUGGINGFACEHUB_API_TOKEN=<your_hf_token>

# 4. Run Streamlit app
$ streamlit run app.py
```

---

## 📃 Code Architecture

```
.
├── app.py                  # Streamlit frontend & integration
├── rag_pipeline.py        # LangChain RAG pipeline (retriever, prompt, generation)
├── evaluate.py            # (Optional) RAGAS-based evaluation setup
├── requirements.txt       # Dependencies
└── .env                   # API tokens
```

---

## 🔢 Usage Flow (Backend Logic)

1. **Document Ingestion**

   ```python
   loader = PyPDFLoader("file.pdf")
   docs = loader.load()
   ```

2. **Chunking**

   ```python
   splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
   chunks = splitter.split_documents(docs)
   ```

3. **Embedding & Indexing**

   ```python
   embeddings = HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")
   vector_store = FAISS.from_documents(chunks, embeddings)
   ```

4. **Query Handling**

   ```python
   retriever = vector_store.as_retriever()
   retrieved_docs = retriever.invoke(question)
   ```

5. **LLM Answer Generation**

   ```python
   context_text = "\n\n".join([doc.page_content for doc in retrieved_docs])
   prompt = PromptTemplate(...)
   final_prompt = prompt.invoke({"context": context_text, "question": question})
   result = chat_model.invoke(final_prompt)
   ```

---

## 🔮 Evaluation (Coming Soon)

* Will integrate **RAGAS** to evaluate:

  * Context Precision
  * Faithfulness
  * Answer Relevancy
* Planned metrics will be shown in-app as visual dashboards.

---

## 📢 Contribution

We welcome contributions! Please open issues or submit PRs.

---

## 📍 Author

Built by **Dheeraj Kumar**
MTech in AI/ML | BTech in Electrical Engineering (IIT Jodhpur)

---

## 🔧 Sample Output

```
User: Tell me about GPS spoofing.

Assistant: GPS spoofing is the act of manipulating GPS signals to deceive the target into thinking it is in a different location... [from context]
```

---

## 🔗 License

MIT License. See `LICENSE` file.

---

## 🚀 Future Work

* ✏️ Multi-file PDF support
* 📊 RAGAS dashboard
* 🛠️ HuggingFace Spaces deployment
* 👤 User authentication + document privacy

---

Thank you for visiting ✨
