# AI Email Response System (RAG + ML)

## 📌 Overview
This project automatically generates replies to customer emails using:
- Machine Learning (TF-IDF + Logistic Regression)
- RAG (Retrieval-Augmented Generation)
- FAISS Vector Database
- LLM (Groq / LLaMA)

## ⚙️ Features
- Classifies emails (product-related vs other)
- Retrieves relevant products
- Generates smart replies using LLM

## 🧠 Tech Stack
- Python
- Scikit-learn
- LangChain
- FAISS
- Sentence Transformers

## 📂 Dataset
- Excel file with:
  - Products
  - Emails

## 🚀 How to Run
1. Upload dataset
2. Add your API key:
```python
os.environ["GROQ_API_KEY"] = "YOUR_KEY"
