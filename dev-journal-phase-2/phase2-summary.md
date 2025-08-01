# 🚀 Phase 2 Summary – Smart Photo Storage

## ✅ What’s Been Achieved (Inference & Embeddings)

- **Inference Service (Python + FastAPI)**:
  - Implemented standalone FastAPI app to handle CLIP-based image and text embedding
  - Two endpoints: `/embed/image` and `/embed/text`, both protected with API key auth (`x-api-key`)
  - Used two SentenceTransformer models:
    - `clip-ViT-B-32` for image embeddings
    - `clip-ViT-B-32-multilingual-v1` for multilingual text support
  - Dockerized the inference service
  - Configurable via `.env` (e.g., model names, API key, Qdrant host)

- **Backend Integration**:
  - Connected backend upload flow with inference service (via REST for now)
  - Upon photo upload:
    - Image is saved to disk
    - Metadata saved to MongoDB
    - Inference service is called with the image to get embeddings

- **Qdrant Vector DB**:
  - Set up Qdrant container via Docker
  - Defined `photo_embeddings` collection with appropriate schema (vector size, cosine distance)
  - Stored image embeddings along with metadata (photo ID, user ID)

- **Security**:
  - Implemented API key validation for all inference requests using FastAPI’s dependency injection
  - API key managed via environment variable

- **Containerization & Deployment**:
  - Final Docker image size for inference service: ~1.7GB (due to PyTorch)
  - Decision made to retain `torch` dependency for model compatibility

## 🧠 Design Highlights

- Inference is fully **decoupled** from backend, enabling async and scalable workflows
- Chose API key over OAuth or JWT to keep it lightweight and integration-friendly
- Made embedding service modular and ready for CI/CD and future expansion
- Accepted Docker size tradeoff to retain flexibility and model performance

## 🛤️ What’s Next (Phase 3+)

- 📨 **Asynchronous Embedding**:
  - Integrate RabbitMQ to offload embedding jobs from backend
  - Backend will send events; inference service will consume and insert into Qdrant

- ☁️ **Object Storage**:
  - Transition from local disk to MinIO or S3-compatible storage for images

- 📦 **Performance Optimization**:
  - Reduce Docker image size if possible (via slim base or Torch prebuilds)
  - Introduce model caching and lazy loading where relevant
