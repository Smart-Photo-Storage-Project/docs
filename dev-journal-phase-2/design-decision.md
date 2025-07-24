# 📐 Design Decisions – Phase 2: ML Integration

This document outlines the major technical considerations and design choices made in **Phase 2** of the *Smart Photo Storage* project, focusing on **semantic search**, **image embedding**, and **machine learning integration**.

---

## 🎯 Goal

To enable users to search for photos not only by filename or metadata, but also through **natural language descriptions**, such as:

- "liburan bersama keluarga di pantai"
- "sunset with friends"
- "foto saat perpisahan SMA"

This is made possible by generating **semantic embeddings** for both images and user queries, then comparing them using **cosine similarity**. By doing so, users can find relevant photos even if they don't remember the filename or the exact keywords.

---

## 🧠 Chosen Strategy

### 1. Use Pretrained Embedding Models

We use **off-the-shelf pretrained models** instead of training from scratch:

- **CLIP (OpenAI)** → for converting images into vector embeddings  
- **M-CLIP (Multilingual CLIP)** → for converting natural language queries into embeddings (supports **Bahasa Indonesia** and **English**)

> ✅ **Following official recommendation**: We use **two separate models**, one for image and one for text embedding.  
> - Image model: `clip-ViT-B-32`  
> - Text model: `sentence-transformers/clip-ViT-B-32-multilingual-v1`  
> This approach is based on the [M-CLIP documentation](https://huggingface.co/sentence-transformers/clip-ViT-B-32-multilingual-v1), which aligns the multilingual text model to the original CLIP image space.

---

## 🧱 System Architecture Additions (Phase 2)

| Component                  | Role                                                                 |
|---------------------------|----------------------------------------------------------------------|
| `photo-storage-inference` | FastAPI service for `/embed/image` and `/embed/text` endpoints       |
| `photo-storage-backend`   | Sends image and query text to inference service, stores to vector DB |
| `Vector DB (Qdrant)`      | Stores image vectors and enables fast semantic search                |
| `MongoDB`                 | Still used for photo metadata                                        |
| `Frontend (Vue)`          | Allows user to input natural language search                         |

---

## 🗂️ Embedding Storage Strategy

### Why Move to a Vector DB (Qdrant)

- MongoDB isn't optimized for vector similarity search
- Qdrant supports **fast and scalable** vector indexing (HNSW)
- Native support for **cosine similarity**, filtering, and payload (metadata)
- Well-documented REST and gRPC APIs with Docker support

### Search Process (Updated):

1. User submits a query in English or Indonesian
2. Backend sends the query to inference service → returns a text embedding
3. Backend sends this embedding to Qdrant → gets top-N similar image vectors
4. Image IDs are matched with metadata from MongoDB
5. Results returned to frontend

---

## 🌐 Language Support

We chose **M-CLIP** because:

- Supports **Bahasa Indonesia** and **English**
- Allows local and international users to search naturally
- Avoids needing to fine-tune a new model
- Robust and production-tested

> ✅ Example: Users can search using phrases like _"foto ulang tahun"_ or _"birthday at cafe"_ interchangeably.

---

## 🧪 Embedding Pipeline Summary

| Step              | Model Used                                        |
|-------------------|---------------------------------------------------|
| Image → Vector    | `clip-ViT-B-32` (original OpenAI CLIP)            |
| Text → Vector     | `sentence-transformers/clip-ViT-B-32-multilingual-v1` |
| Vector Storage    | Qdrant (via HTTP API or client SDK)              |
| Similarity Metric | Cosine similarity                                 |

---

## 📌 Summary of Phase 2 Decisions

| Area              | Decision                                                                 |
|-------------------|--------------------------------------------------------------------------|
| 🔤 Text Embedding | Use **M-CLIP** (Bahasa Indonesia + English)                              |
| 🖼️ Image Embedding | Use **original CLIP** (clip-ViT-B-32)                                     |
| 🔍 Semantic Search | Compare embeddings using **cosine similarity**                          |
| 💾 Vector Storage  | Use **Qdrant vector DB**                                                 |
| 🔁 Inference Flow  | Implemented as separate **FastAPI service** with two models             |
| ✅ Recommendation  | Follows official best practices for CLIP + M-CLIP dual-model architecture |

