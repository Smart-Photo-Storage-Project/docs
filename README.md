# 📸 Smart Photo Storage

**Smart Photo Storage** is a personal flagship project aimed at building a modern, intelligent photo management application. Unlike basic cloud storage apps, this system not only stores and organizes your photos but also uses machine learning to make them searchable through natural language queries like `"family beach"` or `"birthday at night"`.

This project is designed to showcase real-world engineering skills across backend development, machine learning integration, and scalable architecture — built to reflect production-quality complexity while still being achievable as a solo developer.

---

## ✨ Key Features (Planned)

- Secure photo upload and local disk storage
- Web-based gallery for browsing photos
- Natural language photo search using image embeddings
- External ML inference service for semantic understanding
- Asynchronous photo processing using message brokers
- Containerized architecture with Docker
- Designed to be self-hostable and cloud-agnostic

---

## 📌 Project Goals

- Build a full-stack product from scratch
- Apply modern software engineering practices
- Integrate machine learning into a real-world app
- Design a modular, scalable architecture
- Demonstrate practical system design and implementation skills

---

## 🗺️ Roadmap

### ✅ Phase 1 – MVP
- ✅ Backend API in Go for photo upload and metadata handling  
- ✅ Frontend in Vue.js for image browsing and upload  
- ✅ Store metadata (e.g., upload time, tags) in MongoDB  
- ✅ Save image files to local disk (`/uploads`)  
- ✅ Dockerized backend and frontend services  

### ✅ Phase 2 – ML & Semantic Search Integration
- ✅ Create external FastAPI service for image embedding (CLIP/m-CLIP)  
- ✅ Call inference service from backend after upload  
- ✅ Store image embeddings in **Qdrant** (vector DB)  
- ✅ Implement basic semantic search using cosine similarity  

### 🔄 Phase 3 – Asynchronous Architecture & Object Storage *(In Progress)*
- [ ] Add message broker (RabbitMQ)  
- [ ] Queue image embedding jobs on upload  
- [ ] Migrate image storage from local disk to **MinIO (S3-compatible)**  
- [ ] Move toward a loosely coupled, production-ready pipeline  

### 🔜 Phase 4 – Polish and Deploy
- [ ] CI/CD pipeline (GitHub Actions or similar)  
- [ ] Local + cloud deployment with Docker Compose or Kubernetes  
- [ ] Record demo video, write technical blog post  
- [ ] Finalize documentation and system architecture diagrams  
---

## 🧱 Architecture Overview

The system consists of modular services:

| Service                 | Description                                            | Repository |
|-------------------------|--------------------------------------------------------|------------|
| `photo-storage-backend` | Go-based API for upload, metadata, and search          | [GitHub](https://github.com/Smart-Photo-Storage-Project/backend) |
| `photo-storage-frontend` | Vue.js UI for browsing and uploading photos           | [GitHub](https://github.com/Smart-Photo-Storage-Project/frontend) |
| `photo-storage-inference` | FastAPI service for generating image embeddings      | [GitHub](https://github.com/Smart-Photo-Storage-Project/inference) |
| `photo-storage-infra`   | Docker Compose, environment configs, orchestration     | [TBA]() |
| `photo-storage-docs`    | Planning, architecture, dev journal                    | [TBA]() |


---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|------------|
| **Backend** | Go (Gin, Echo, or Fiber) |
| **Frontend** | Vue.js |
| **Database** | MongoDB (stores metadata and embeddings in MVP) |
| **ML Service** | Python + FastAPI (uses CLIP/BLIP for embeddings) |
| **Queue (Phase 3+)** | RabbitMQ |
| **Image Storage** | Local Disk → MinIO (Phase 5) |
| **Vector DB (Phase 5)** | Qdrant or Weaviate |
| **Deployment** | Docker, Docker Compose |
| **CI/CD** | GitHub Actions (planned) |

---

