# Customer Support Email Responder

Built this to automatically handle product inquiry emails using RAG — the idea being that the model should only ever respond with products that actually exist in the database, nothing fabricated.

---

## The Problem

Customer support teams get a lot of repetitive emails asking about product prices, availability, and seasons. Most of these can be answered automatically — but only if the model is constrained to your actual inventory. Generic LLMs will just make products up if they don't know. This fixes that.

---

## How it works

```
Customer email
    ↓
Embedded using all-MiniLM-L6-v2
    ↓
FAISS searches the product vector store
    ↓
Top-k products passed into the prompt
    ↓
LLaMA 3.1 (via Groq) generates the reply
```

The LLM never sees your full product list — just the ones semantically close to the email. If nothing relevant comes back, it tells the customer the product isn't in the database.

---

## Stack

| | |
|---|---|
| Embeddings | HuggingFace `all-MiniLM-L6-v2` |
| Vector Store | FAISS |
| LLM | LLaMA 3.1 8B via Groq |
| Pipeline | LangChain |
| Data | Excel (Pandas + OpenPyXL) |

---

## Setup

```bash
pip install langchain langchain-groq langchain_community langchain-huggingface \
            faiss-cpu pandas openpyxl sentence-transformers
```

Get a free Groq key at [console.groq.com](https://console.groq.com) and set it:

```python
os.environ["GROQ_API_KEY"] = "your_key_here"
```

---

## Dataset

One Excel file, two sheets:

- **Products** — ID, name, category, description, stock, season, price  
- **Client Emails** — subject and body

---

## Running it

Upload the Excel file to Colab and run the notebook cells in order. To try your own email:

```python
generate_response(
    email_subject="Do you have a wireless speaker?",
    email_body="Looking for something portable under $100."
)
```

---

## Sample Outputs

### Product found
![Response - product exists](sample_response_1.png)

### Product not in database
![Response - product missing](sample_response_2.png)

### Off-topic email
![Response - unrelated](sample_response_3.png)

---

## Known limitation

The similarity score cutoff (`< 1.0`) is a bit blunt — it occasionally drops valid products if the email wording is far from the description. Worth tuning if you're expanding the dataset.

---

## To do

- Streamlit or Gradio front-end  
- Connect to a real inbox via Gmail API  
- Better score threshold or hybrid search

---

**Rubab Tahir** · [GitHub](https://github.com/rubabkhanbzp) · rubabtahirbzp@gmail.com
