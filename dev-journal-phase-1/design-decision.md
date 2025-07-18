# 🧠 Design Decisions – Smart Photo Storage

This document summarize technical considerations and architectural decisions for the Smart Photo Storage project.

---

##  Image Storage

- **Current**: Images are stored locally under the `/uploads` folder and served as static files
- **Planned**: Will migrate to MinIO or S3-compatible object storage in a later phase
- **Reason**: Enables decoupling between backend app and storage layer, improves scalability and portability

---

##  Authentication & JWT

- **Current**: JWT-based authentication with tokens stored in frontend localStorage
- **Planned**: Migrate to HTTP-only cookies for improved XSS protection and security best practices
- **Status**: JWT middleware is already implemented in the backend

---

##  Image Access Control

- **Current**: `/uploads` folder is publicly accessible
- **Planned**: Access control via backend endpoint (restricted to photo owners only)
- **Future**: Protection will be integrated with the migration to MinIO or similar

---

##  Search & Pagination

- **Search**: Case-insensitive partial match on photo `name` field
- **Pagination**: Implemented using MongoDB `skip` and `limit` strategy
- **Future**: Will be extended to support semantic search using ML-generated image embeddings
