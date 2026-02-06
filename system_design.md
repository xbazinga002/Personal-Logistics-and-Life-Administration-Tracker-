# System Design
**Project:** Personal Logistics and Life Administration Tracker  
**Team:** AdminOPs  
**Members:**James Rhodes,Shane Stroud
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
