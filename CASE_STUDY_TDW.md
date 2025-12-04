# Case Study: TDW (US) Inc. x NoticeHub  
### Centralizing Global Tax Notice Management on Google Cloud

---

## 1. Customer Profile

**Company:** TDW (US) Inc.  
**Industry:** Industrial / Energy (Global Operations)  
**Team:** Indirect tax and finance team managing multi-jurisdiction compliance  

TDW (US) Inc. operates across multiple states and countries, receiving a high volume of tax notices from different authorities. Before NoticeHub, managing these notices depended heavily on email, shared folders, and manual tracking in spreadsheets.

---

## 2. Challenge

TDW’s tax team faced three core challenges:

1. **Fragmented visibility**  
   Notices arrived via mail and email, were scanned by different people, and stored in various shared folders. It was difficult to answer simple questions like “How many notices are open for this jurisdiction?” or “Did we already respond to this letter?”.

2. **Manual data entry & tracking**  
   Key data—such as tax authority, period, due date, and amounts—was entered manually into spreadsheets. This was time-consuming and prone to errors, especially when dealing with complex, multi-page notices.

3. **Risk of missed deadlines**  
   With notices scattered across inboxes and files, reminders depended on individual calendars. The team wanted a system that would track due dates reliably and reduce the risk of late responses or penalties.

---

## 3. Solution Overview

TDW implemented **NoticeHub**, an AI-powered tax notice management platform built on Google Cloud. The solution consolidates all notices into a single workspace and automates the most time-consuming steps of the process:

- Central upload of scanned PDF notices  
- AI-based extraction of key fields  
- Smart deduplication and linking of related notices  
- Collaborative review, annotation, and task assignment  
- Automated email reminders for upcoming deadlines  

By running on Google Cloud, NoticeHub provides the scalability, security, and reliability required for handling sensitive financial information across jurisdictions.

---

## 4. How NoticeHub Works for TDW

### 4.1 Upload & Ingestion

- The tax team uploads scanned notices directly into NoticeHub.  
- PDF files are stored in **Google Cloud Storage**.  
- A processing service running on **Cloud Run** picks up new files and prepares them for extraction.

### 4.2 AI-Powered Data Extraction

- **Document AI** separates multi-notice PDFs into individual notices.  
- The service reads and extracts key information such as:
  - Tax authority and jurisdiction  
  - Taxpayer entity  
  - Tax period and notice date  
  - Amounts and payment references  
  - Notice type and due dates
- Extracted data is written into **Firestore / Cloud SQL**, giving the team structured records for search and reporting.

### 4.3 Workflow & Collaboration

- **Pub/Sub** and **Cloud Functions** trigger workflows for new notices, duplicates, and status changes.  
- Team members review and validate extracted data, annotate notices, and assign tasks.  
- All actions are logged, giving TDW a clear audit trail for internal and external review.

### 4.4 Alerts & Reporting

- Cloud Functions send **email notifications** for new tasks and approaching due dates.  
- Optional exports to **BigQuery** allow the team to analyze trends and build dashboards in **Looker Studio** (e.g., notice volume by jurisdiction, aging analysis, and resolution time).

---

## 5. Results

After adopting NoticeHub on Google Cloud, TDW (US) Inc. achieved:

- **Faster processing:** The time to capture and organize new notices was reduced from hours to minutes.  
- **Improved accuracy:** Automated extraction significantly reduced manual data entry errors.  
- **Centralized oversight:** The tax team now works from a single source of truth for all notices, with clear status and ownership.  
- **Lower risk:** Automated reminders and structured workflows reduced the likelihood of missing due dates.

---

## 6. Customer Quote

> “Noticehub has transformed the way we handle tax notices by providing a centralized platform for our team. It streamlines the upload, categorization, and review process, allowing us to act faster and with greater accuracy across jurisdictions.”  
> **— Amelia Staton, Indirect Tax Analyst at TDW (US) Inc.**

---

## 7. Why Google Cloud

NoticeHub chose Google Cloud as the foundation for this solution because of:

- **Advanced document understanding** with Document AI  
- **Scalable, serverless compute** using Cloud Run and Cloud Functions  
- **Reliable storage** with Cloud Storage and Firestore / Cloud SQL  
- **Integrated data analytics** via BigQuery and Looker Studio  
- **Strong security and compliance** features suitable for financial and tax data

For TDW, this means a modern solution that can grow with their operations while maintaining the performance and reliability their tax team requires.

---

## 8. Next Steps

TDW is continuing to expand its use of NoticeHub by:

- Onboarding additional entities and jurisdictions  
- Enhancing analytics through Looker dashboards  
- Integrating NoticeHub with internal ticketing and ERP systems for end-to-end workflow visibility

NoticeHub will keep building on Google Cloud services to support these initiatives while maintaining a clean, collaborative experience for tax professionals.
