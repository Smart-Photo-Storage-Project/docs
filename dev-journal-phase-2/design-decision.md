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

-  **CLIP (OpenAI)** for converting images into vector embeddings
-  **M-CLIP (Multilingual CLIP)** for converting natural language queries into embeddings (supports **Bahasa Indonesia** and **English**)

>  Rationale: Fine-tuning is unnecessary because our search use case is general. Pretrained models already work well for broad, diverse datasets.

---

## 🗂️ Embedding Storage Strategy

| Option | Chosen |
|--------|--------|
| Vector DB (Qdrant, Weaviate) | ❌ Not yet |
| Store in MongoDB | ✅ Yes (for now) |

###  Why MongoDB for Now:
- Simplifies implementation for MVP
- Suitable for small-medium scale vector search
- No need for additional infrastructure
- Cosine similarity can be computed in Go backend

###  Search Process:
1. User submits a query in English or Indonesian
2. Backend sends query to inference service → returns a vector
3. All image vectors are retrieved from MongoDB
4. Cosine similarity is calculated in Go
5. Top-N results are returned to the frontend

---

## 🌐 Language Support

We chose **M-CLIP** because:
- Supports **Bahasa Indonesia** and **English**
- Allows local and international users to use search naturally
- Future-proof and robust for multilingual input

>  Example: Users can search using phrases like _"foto ulang tahun"_ or _"birthday at cafe"_ interchangeably.

---

## 🧱 System Architecture Additions (Phase 2)

| Component | Role |
|----------|------|
| `photo-storage-inference` | FastAPI service for `/embed/image` and `/embed/text` endpoints |
| `photo-storage-backend` | Calls inference service to get embeddings for images and queries |
| `MongoDB` | Stores photo metadata and corresponding image embeddings |
| `Frontend (Vue)` | Allows user to input natural language search |

---

## 📌 Summary of Phase 2 Decisions

| Area | Decision |
|------|----------|
| 🔤 Text Embedding | Use **M-CLIP** (supports Bahasa Indonesia + English) |
| 🖼️ Image Embedding | Use standard **CLIP** |
| 🔍 Semantic Search | Compare image–query embeddings via cosine similarity |
| 💾 Vector Storage | Store in **MongoDB** (for now) |
| ⚡ Vector DB | Postponed until necessary for scaling |
| 🔁 Inference Strategy | Run as separate FastAPI service (Dockerized) |
