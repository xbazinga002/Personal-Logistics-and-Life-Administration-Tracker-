# System Design
**Project:** Personal Logistics and Life Administration Tracker  
**Team:** AdminOPs  
**Members:**
**Date:** 02/06/2026

---

## 1. Design Overview
This document translates the project requirements into a technical design. The system is a full-stack web application that provides secure user accounts, life-admin item tracking, dashboard summaries, recurrence logic, and in-app notifications with persistent storage in PostgreSQL.

---

## 2. Architecture
### Architecture Style
Client–server application using a layered design:
- **Presentation Layer:** React + TypeScript UI
- **API Layer:** Express REST API (controllers/routes)
- **Service Layer:** business logic (recurrence, urgency/status, notifications)
- **Data Access Layer:** DB queries/repositories
- **Database Layer:** PostgreSQL

### High-Level Architecture Diagram
```mermaid
flowchart LR
  FE[Frontend - React and TypeScript] --> BE[Backend - Express REST API]
  BE --> AUTH[Auth - JWT middleware]
  BE --> SVC[Service layer - business logic]
  SVC --> DAL[Data access layer]
  DAL --> DB[PostgreSQL database]
  SVC --> NOTIF[Notification generator]
  BE --> FILES[Optional file storage]


---



## 3. Technology Choices
### Front End
- React + TypeScript
- React Router for routing
- React Hook Form + Zod for validation (recommended)

### Back End
- Node.js + Express
- JWT authentication
- bcrypt for password hashing

### Database
- PostgreSQL
- Migrations tool (recommended: Prisma Migrate, Knex migrations, or Sequelize migrations)

### Testing
- Jest (unit tests)
- Supertest (API integration tests)

---

## 4. Data Model Design
### Tables (Proposed)
- `users` (id, email, password_hash, created_at)
- `items` (id, user_id, title, notes, due_date, status, recurrence_type, recurrence_interval, archived_at, created_at)
- `categories` (id, user_id, name)
- `tags` (id, user_id, name)
- `item_tags` (item_id, tag_id)
- `notifications` (id, user_id, item_id, type, message, is_read, created_at)
- optional: `attachments` (id, item_id, filename, mime_type, size_bytes, storage_path, uploaded_at)

### ER Diagram
```mermaid
erDiagram
  USERS ||--o{ ITEMS : owns
  USERS ||--o{ CATEGORIES : defines
  USERS ||--o{ TAGS : defines
  ITEMS ||--o{ NOTIFICATIONS : generates
  ITEMS ||--o{ ITEM_TAGS : has
  TAGS ||--o{ ITEM_TAGS : links
