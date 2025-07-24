# 📐 Design Decisions – Phase 1: Core Functionality

This document summarizes technical considerations and architectural decisions made in **Phase 1** of the *Smart Photo Storage* project, focusing on **foundational features** including storage, authentication, upload mechanisms, and containerization.

---

## 🎯 Goal

To build a **minimum viable product (MVP)** that allows users to:

- Register and log in securely
- Upload and view photos
- Navigate with a modern frontend interface
- Prepare the system for future enhancements such as semantic search and scalable storage

---

## 🖼️ Image Storage

| Feature | Status |
|--------|--------|
| Storage Location |  Local `/uploads` folder |
| Serving Method |  Static file serving via backend |
| Planned |  Migrate to **MinIO** or S3-compatible storage |
| Reason | Enables backend–storage decoupling, improves scalability & portability |

---

## 🔐 Authentication & JWT

| Feature | Status |
|--------|--------|
| Auth Method |  JWT-based login |
| Token Storage |  `localStorage` on frontend |
| Planned |  Move to **HTTP-only cookies** |
| Reason | More secure against XSS attacks, follows best practices |

> JWT middleware is already implemented and enforced on protected backend routes.

---

## 🛂 Image Access Control

| Feature | Status |
|--------|--------|
| Access |  All images under `/uploads` are publicly accessible |
| Planned |  Restrict access via backend logic (photo ownership) |
| Future | Integrated with MinIO/S3 to support signed URLs or ACL-based control |

---

## 🔍 Search & Pagination

| Feature | Status |
|--------|--------|
| Search |  Partial match on `name` field (case-insensitive) |
| Pagination |  Implemented via `skip` and `limit` in MongoDB |
| Future | ⏭ Extend to **semantic search** using vector embeddings in Phase 2 |

---

## 📤 Upload Mechanism

| Feature | Status |
|--------|--------|
| Single Upload |  Supported |
| Batch Upload |  Supported via `multipart/form-data` |
| DB Insert |  Uses `insertMany` for batch |
| Planned |  Add goroutine-based parallel processing for scalability |
| Next |  Integrate with MinIO in next phase with metadata and ownership checks |

---

## 📦 Containerization

| Feature | Status |
|--------|--------|
| Dockerfiles |  Added for both **frontend** and **backend** |
| Docker Compose |  Planned in next phase |
| Reason | Enables consistent, reproducible environments and simplifies deployment |

---

## 🔧 Summary of Phase 1 Decisions

| Area | Decision |
|------|----------|
| 🗂️ Storage | Local file system (`/uploads`) |
| 🔐 Auth | JWT with `localStorage` |
| 🚫 Access Control | Public now, protected later |
| 🔍 Search | Filename-based |
| ⚙️ Upload | Single + batch (with `insertMany`) |
| 🐳 Containerization | Dockerized backend & frontend |
