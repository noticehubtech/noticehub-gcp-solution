# NoticeHub Architecture Overview  
### Google Cloud–Powered Tax Notice Management Platform

This document provides a technical overview of how NoticeHub is architected on Google Cloud to support AI-driven tax notice ingestion, extraction, workflow automation, and team collaboration. The design prioritizes reliability, security, scalability, and operational simplicity.

---

## 1. High-Level Architecture Diagram

```
                         ┌──────────────────────────┐
                         │      User Interface       │
                         │  Web App / Dashboard      │
                         └────────────┬──────────────┘
                                      │
                                      ▼
                      ┌────────────────────────────────────┐
                      │       File Upload & Ingestion      │
                      │       (Cloud Storage Bucket)        │
                      └────────────┬────────────────────────┘
                                   │ File Event
                                   ▼
                     ┌───────────────────────────────────────┐
                     │     Processing Service (Cloud Run)     │
                     │  Preprocessing, validation, batching   │
                     └────────────┬────────────────────────────┘
                                  │ Calls
                                  ▼
                ┌────────────────────────────────────────────────────┐
                │                Document AI Pipeline                │
                │  OCR → Classification → Split PDFs → Field Extraction │
                └─────────────────────────┬────────────────────────────┘
                                          │ Output Payload
                                          ▼
                         ┌────────────────────────────────┐
                         │   Firestore / Cloud SQL        │
                         │ Structured notice records       │
                         └─────────────────────────────────┘
                                          │
                             ┌────────────┴─────────────┐
                             ▼                          ▼
        ┌──────────────────────────┐       ┌───────────────────────────┐
        │ Pub/Sub Event Triggers  │       │ Cloud Functions (Actions) │
        │ new_notice, deadlines    │       │ email alerts, dedup check │
        └─────────────┬────────────┘       └───────────────────────────┘
                      │
                      ▼
         ┌──────────────────────────────────┐
         │       Collaboration Layer        │
         │ Comments, assignments, audit log │
         └──────────────────────────────────┘
```

---

## 2. Key Google Cloud Components

### **Cloud Storage**
- Entry point for uploaded PDF notices  
- Stores original files securely  
- Triggers downstream processing via event notifications  

---

### **Cloud Run**
- Stateless processing service  
- Splits batch PDFs, prepares images for Document AI  
- Handles retries and long-running tasks safely  
- Scales automatically based on load  

---

### **Document AI**
- Identifies multiple notices inside a PDF  
- Extracts structured fields:
  - Tax authority  
  - Taxpayer details  
  - Amounts  
  - Notice type  
  - Tax period  
  - Due dates  
- Supports multilingual documents  

---

### **Firestore / Cloud SQL**
Used depending on data requirements:

#### Firestore  
- Ideal for flexible document structures  
- Provides fast querying and simple indexing  

#### Cloud SQL  
- Used when relational models or analytics queries are required  
- Strong consistency for reporting and joins  

---

### **Pub/Sub**
- Event bus for workflow automation  
- Triggers include:
  - new_notice  
  - approaching_due_date  
  - task_assignment  

---

### **Cloud Functions**
- Sends email notifications  
- Performs duplicate checks  
- Handles task creation and workflow routing  

---

### **BigQuery / Looker Studio (optional)**
Enables reporting dashboards such as:
- Notice volume by jurisdiction  
- Processing time  
- Accuracy and review metrics  
- Aging and compliance KPIs  

---

## 3. Security & Compliance Considerations

- All files encrypted at rest (Google-managed keys by default)  
- TLS enforced for all data in transit  
- Service accounts applied with principle of least privilege  
- Firestore/SQL access restricted via IAM  
- Audit logging enabled for critical operations  
- Optionally integrates with VPC-SC for perimeter security  

---

## 4. Scalability & Performance

- Cloud Run scales to zero when idle and to large workloads during peak notice season  
- Document AI supports parallel extraction  
- Pub/Sub ensures durable, decoupled workflows  
- Architecture supports large-scale multi-jurisdiction notice processing  

---

## 5. Why This Architecture Works

- Serverless components minimize operational overhead  
- Document AI provides industry-leading extraction accuracy  
- Event-driven design ensures responsiveness and reliability  
- Modular system allows easy future expansion (OCR templates, MDM integration, APIs)  

---

This architecture enables NoticeHub to deliver a high-performance, intuitive platform for tax notice teams while maintaining operational simplicity on Google Cloud.
