# Implementation Notes  
### Deployment Guidance for NoticeHub on Google Cloud

This document provides practical implementation notes for deploying and operating NoticeHub on Google Cloud. It covers recommended configurations, integration patterns, and operational considerations.

---

## 1. Deployment Overview

NoticeHub runs as a collection of serverless and cloud-native components:

- **Cloud Run** for compute  
- **Cloud Storage** for file ingest  
- **Document AI** for extraction  
- **Firestore / Cloud SQL** for structured storage  
- **Pub/Sub + Cloud Functions** for automation  

This design allows for minimal DevOps burden while maintaining robust performance.

---

## 2. Storage & Ingestion Setup

### **Cloud Storage Bucket Structure**

```
gs://noticehub-prod/
  uploads/
  processed/
  failed/
  logs/
```

**Recommended settings:**
- Uniform bucket-level access  
- Lifecycle rules for cleanup (e.g., move old processed files to “archive”)  
- Event triggers:
  - New object → Pub/Sub topic → Cloud Run processor  

---

## 3. Cloud Run Processing Service

### Responsibilities:
- Accepts file ingestion events  
- Prepares PDFs for Document AI  
- Handles multi-page splitting  
- Normalizes images for extraction  
- Sends payloads to extraction pipeline  

### Best Practices:
- Use minimal container images  
- Set concurrency based on memory footprint (4–20 recommended)  
- Use Cloud Run Jobs for batch backfills  

---

## 4. Document AI Integration

### Extraction Pipeline:
1. OCR  
2. Classification  
3. Page splitting (if multiple notices exist)  
4. Key-value extraction  
5. Error handling and fallback rules  

### Performance Tips:
- Enable **processor versioning** for stability  
- Use parallel API calls for multi-page documents  
- Cache processor IDs in environment variables  
- Implement retry logic for quota rate limits  

---

## 5. Database Notes

### **Firestore Recommended For:**

- Flexible fields (tax authorities differ globally)  
- Fast UI queries  
- Real-time updates with listeners  

### **Cloud SQL Recommended For:**

- BI reporting  
- Relational data models  
- Complex joins and aggregates  

**Indexes:**  
Pre-create indexes for fields like:
- `tax_authority`  
- `notice_type`  
- `due_date`  
- `status`  

---

## 6. Workflow Automation

### Pub/Sub Topics:
```
new_notice
data_extracted
due_soon
task_created
task_completed
```

### Cloud Functions:
- Sends email notifications  
- Checks for duplicates  
- Updates audit logs  
- Generates task routing events  

---

## 7. Collaboration Layer

The application layer manages:

- Comments  
- Annotations  
- Attachments  
- Task assignments  
- Status transitions  

All actions are recorded with timestamps for audit readiness.

---

## 8. Security Implementation Notes

- Use service accounts scoped to least privilege  
- Separate dev/staging/prod projects  
- Enable Cloud Audit Logs  
- Enforce IAM principals for:
  - Storage  
  - Pub/Sub  
  - Document AI  
  - SQL / Firestore  
- Encrypt sensitive fields where appropriate  
- Consider VPC-SC for enterprise customers  

---

## 9. Monitoring & Logging

### Recommended Tools:
- Cloud Logging (processor logs, extraction logs)  
- Cloud Monitoring dashboards  
- Log-based metrics for:
  - Extraction failure rates  
  - Processing duration  
  - Notice volume over time  

Alerts can be configured for:
- Repeated extraction failures  
- High Cloud Function invocation errors  
- Storage ingestion spikes  

---

## 10. Extensibility Notes

Future enhancements can be added without major architecture changes:

- ERP integrations (SAP, NetSuite) via Cloud Run APIs  
- BI dashboards via BigQuery  
- Additional Document AI processors  
- SSO (Okta, Google Workspace, Azure AD)  

---

## 11. Summary

These implementation notes reflect a production-grade, Google Cloud–aligned approach to running NoticeHub. The design emphasizes scalability, reliability, and maintainability, ensuring that tax teams can process notices efficiently without worrying about infrastructure complexity.
