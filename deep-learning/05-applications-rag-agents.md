# 5. Ứng dụng Deep Learning: RAG, Agents và Công nghệ liên quan

Các ứng dụng xây trên **LLM** và **mô hình đặc thù** (OCR, STT, TTS): **RAG** (Retrieval-Augmented Generation), **Agents** (tool use, reasoning), **chatbot**, **hỏi đáp trên tài liệu**, và pipeline đa bước.

**Ứng dụng thực tế:** Hỏi đáp trên tài liệu nội bộ (RAG); chatbot dùng tool (tìm kiếm, API); agent tự lên kế hoạch và gọi tool; tổng hợp pipeline OCR + STT + LLM cho tài liệu đa phương tiện.

---

## 5.1. RAG – Retrieval-Augmented Generation

### Ý tưởng

- **Vấn đề**: LLM chỉ dựa trên kiến thức học khi train → có thể sai, lỗi thời, hoặc không biết dữ liệu nội bộ.
- **RAG**: Khi trả lời câu hỏi, **lấy thêm tài liệu liên quan** (retrieve) từ kho (corpus, vector DB), đưa vào **context** cho LLM → LLM **sinh câu trả lời** dựa trên context đó (generation).

### Pipeline cơ bản

1. **Chuẩn bị kho (offline)**  
   - Thu thập tài liệu (PDF, web, DB).  
   - **Chunk**: Chia tài liệu thành đoạn nhỏ (theo đoạn văn, slide, hoặc kích thước cố định với overlap).  
   - **Embed**: Mỗi chunk → vector (encoder: BERT, E5, multilingual-E5, …).  
   - **Lưu**: Vector store (FAISS, Chroma, Pinecone, Weaviate, Milvus, …).

2. **Truy vấn (online)**  
   - User hỏi → **embed câu hỏi** (cùng encoder).  
   - **Retrieve**: Tìm top-k chunk **gần nhất** (cosine similarity hoặc inner product).  
   - **Prompt**: Nối câu hỏi + các chunk retrieval → đưa vào LLM.  
   - **Generate**: LLM sinh câu trả lời dựa trên prompt.

### Cải tiến thường gặp

| Kỹ thuật | Mô tả |
|----------|--------|
| **Hybrid search** | Kết hợp tìm kiếm vector (semantic) + keyword (BM25) rồi fusion. |
| **Reranking** | Sau retrieval, dùng mô hình rerank (cross-encoder) chấm điểm (query, chunk) → lấy top nhỏ hơn cho context. |
| **Chunk strategy** | Chunk theo câu/đoạn, overlap, hoặc semantic split (model tách theo nghĩa). |
| **Query expansion** | Tạo nhiều biến thể câu hỏi (LLM) rồi retrieve cho từng câu, gộp kết quả. |
| **Citation / source** | Trả về kèm nguồn (chunk, trang) để người dùng kiểm chứng. |

### RAG với tài liệu ảnh / đa phương tiện

- **OCR**: Ảnh tài liệu → text (TrOCR, PaddleOCR) → chunk → embed → RAG như trên.  
- **Multimodal**: Ảnh → embed bằng vision encoder (CLIP, SigLIP); câu hỏi → text embed; retrieve ảnh hoặc ảnh+text.

---

## 5.2. Agents và Tool Use

### Agent

- **Agent**: Hệ thống LLM + **công cụ (tools)**; LLM quyết định **gọi công cụ nào**, với **thông số gì**, rồi dựa trên **kết quả** để bước tiếp hoặc trả lời user.
- **Vòng lặp**: User query → LLM (có thể gọi tool) → tool trả về → LLM (gọi thêm tool hoặc trả lời) → … → response cuối.

### Tool (Function) Calling

- **Định nghĩa tool**: Tên, mô tả, tham số (schema JSON). Ví dụ: `search(query)`, `calculator(expression)`, `get_weather(city)`.
- **LLM** sinh ra **structured output** (tool name + arguments) thay vì chỉ text; hệ thống gọi API với arguments đó và đưa kết quả lại cho LLM.
- **Ứng dụng**: Tìm kiếm web, truy vấn DB, gọi API bên ngoài, đọc file, chạy code (có kiểm soát).

### ReAct và Reasoning

- **ReAct** (Reasoning + Acting): LLM luân phiên **suy luận** (thought) và **hành động** (action = gọi tool); dựa trên observation (kết quả tool) để bước tiếp. Giúp chia nhỏ bài toán, giảm hallucination khi có tool đúng.
- **Chain-of-Thought (CoT)**: Yêu cầu LLM “suy từng bước” trước khi trả lời; có thể kết hợp với tool (suy luận cần số liệu → gọi tool → tiếp tục suy luận).

---

## 5.3. Ứng dụng tổng hợp

| Ứng dụng | Thành phần | Mô tả ngắn |
|----------|------------|------------|
| **Chatbot hỏi đáp tài liệu** | RAG + LLM | Upload PDF → chunk → embed → store; user hỏi → retrieve → LLM trả lời + trích nguồn. |
| **Trợ lý giọng nói** | STT + LLM + TTS | Voice → STT → text → LLM (có thể RAG/tool) → text → TTS → voice. |
| **Số hóa & tìm kiếm tài liệu** | OCR + RAG | Ảnh/PDF → OCR → text → chunk → embed → RAG. |
| **Agent đa bước** | LLM + Tools | Lên kế hoạch, gọi search/DB/API, tổng hợp → trả lời hoặc thực hiện tác vụ. |
| **Code assistant** | LLM + retrieval (codebase) | Retrieve code liên quan → LLM sinh/sửa code, giải thích. |

---

## 5.4. Kỹ thuật và công cụ thường dùng

| Công cụ / Khái niệm | Mô tả |
|---------------------|--------|
| **LangChain / LlamaIndex** | Framework: kết nối LLM, vector store, retriever, tool; chain RAG, agent. |
| **Vector DB** | Chroma, FAISS, Pinecone, Weaviate, Milvus, pgvector (PostgreSQL). |
| **Embedding** | sentence-transformers (all-MiniLM, all-mpnet), E5, BGE, multilingual-E5. |
| **LLM API** | OpenAI GPT, Anthropic Claude, open LLM (Llama, Mistral) qua vLLM, Ollama, Hugging Face. |
| **Evaluation RAG** | Faithfulness (trả lời dựa trên context?), Relevance (retrieval đúng?), có thể dùng LLM-as-judge hoặc metric có sẵn. |

---

## 5.5. Rủi ro và lưu ý

- **Hallucination**: LLM vẫn có thể bịa dù có context; cần prompt rõ (“chỉ dựa vào context sau”), rerank, và citation.  
- **Retrieval kém**: Chunk quá nhỏ/lớn, embedding không phù hợp → không lấy đúng đoạn → trả lời sai. Cần tune chunk size, thử model embed, rerank.  
- **Bảo mật & quyền**: RAG trên tài liệu nội bộ cần kiểm soát truy cập; tool gọi API cần giới hạn và validate.  
- **Chi phí & latency**: Số chunk đưa vào context ảnh hưởng token và thời gian; cần cân bằng top-k, rerank, và model size.

---

## 5.6. Code mẫu (RAG đơn giản với LangChain)

```python
from langchain.vectorstores import Chroma
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.llms import Ollama  # hoặc OpenAI, ...
from langchain.chains import RetrievalQA

embeddings = HuggingFaceEmbeddings(model_name="sentence-transformers/all-MiniLM-L6-v2")
vectorstore = Chroma(persist_directory="./chroma_db", embedding_function=embeddings)
llm = Ollama(model="llama2")

qa = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=vectorstore.as_retriever(search_kwargs={"k": 3}),
    return_source_documents=True,
)
result = qa({"query": "Nội dung chính của tài liệu là gì?"})
print(result["result"])
for doc in result["source_documents"]:
    print(doc.metadata, doc.page_content[:200])
```

---

## 5.7. Tài liệu tham khảo

- Lewis et al., *Retrieval-Augmented Generation for Knowledge-Intensive NLP*
- Yao et al., *ReAct: Synergizing Reasoning and Acting in LLMs*
- [LangChain](https://python.langchain.com/), [LlamaIndex](https://www.llamaindex.ai/)
- [Chroma](https://www.trychroma.com/), [sentence-transformers](https://www.sbert.net/)
