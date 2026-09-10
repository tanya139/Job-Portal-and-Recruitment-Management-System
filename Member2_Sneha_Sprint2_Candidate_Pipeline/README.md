# Job Portal & Recruitment Management System (P12)
**Domain:** HR Tech  
**Academic Context:** 5th Semester • Christ University • CIA-3 Project Development (L&T EduTech)

---

## 👥 Team Members & Sprint Ownership

| Member Name | Role & Responsibility | Assigned Sprint & Modules | Dedicated Git Branch |
|---|---|---|---|
| **Tanya** | **Member 1 (Foundation Lead)** | **Sprint 1:** Modules 1, 2, 3, 4 (User Registration & Auth, Company Profile, Job Postings, Job Search & Filtering) | `feature/sprint1-foundation-jobs` |
| **Sneha** | **Member 2 (Core Workflow Lead)** | **Sprint 2:** Modules 5, 6, 7, 8 (Candidate Profile & Resume Metadata, Application Submission, Pipeline Workflow, Interview Slots) | `feature/sprint2-candidate-pipeline` |
| **Tirth** | **Member 3 (Recruiter Operations & Analytics Lead)** | **Sprint 3:** Modules 9, 10, 11, 12, 13 (Recruiter Dashboard, Saved Jobs & Alerts, Offer Letters, Funnel Analytics, RBAC) | `feature/sprint3-recruiter-reports-rbac` |
| **Tiswin** | **Member 4 (Architecture, DB & QA Lead)** | **Infrastructure & Quality:** MongoDB Schemas & Indexes, Centralized Error Handling, Postman Collection, PPT & Documentation | `feature/infra-database-postman-docs` |

---

## 📌 Problem Statement
Modern recruitment requires a cohesive platform bridging candidates and hiring teams. This platform enables companies to post job openings, candidates to filter jobs and submit applications with resume metadata, recruiters to shepherd applicants through a multi-stage pipeline (`Applied` ➔ `Shortlisted` ➔ `Interview` ➔ `Offered` ➔ `Hired`/`Rejected`), and administrators to monitor hiring metrics and conversion funnels.

---

## 🎯 Implemented Functional Modules (All 13 Modules Complete)

| # | Module Name | Owner | Status | Description |
|---|---|---|---|---|
| **1** | User Registration & Authentication | Tanya | ✅ Complete | JWT-based auth and bcrypt password hashing for Candidate & Recruiter roles. |
| **2** | Company Profile Management | Tanya | ✅ Complete | Recruiter-managed company profiles with industry, location, and description. |
| **3** | Job Posting Management | Tanya | ✅ Complete | Full CRUD for job openings with salary range, required skills, and status. |
| **4** | Job Search & Filtering | Tanya | ✅ Complete | Query jobs by title keyword, required skills, location, and experience level. |
| **5** | Candidate Profile & Resume Metadata | Sneha | ✅ Complete | Candidate profile with experience, skills array, and resume metadata. |
| **6** | Job Application Submission | Sneha | ✅ Complete | Candidate applies to jobs with duplicate prevention (`400/409`). |
| **7** | Applicant Pipeline Workflow | Sneha | ✅ Complete | Controlled transitions: `Applied` ➔ `Shortlisted` ➔ `Interview` ➔ `Offered` ➔ `Hired`/`Rejected`. |
| **8** | Interview Scheduling Records | Sneha | ✅ Complete | Recruiters schedule interview slots (date, time, mode, link/venue, feedback). |
| **9** | Recruiter Applicant Dashboard | Tirth | ✅ Complete | Filter applicants per job posting and review candidate profiles. |
| **10** | Saved Jobs & Job Alerts | Tirth | ✅ Complete | Candidates bookmark jobs and set preference-based job alert records. |
| **11** | Offer Management | Tirth | ✅ Complete | Recruiters issue formal offers; candidates accept or decline. |
| **12** | Admin Reports & Funnel Analytics | Tirth | ✅ Complete | Aggregate pipeline metrics: conversion rate per stage, applicant counts, postings. |
| **13** | Role-Based Access Control (RBAC) | Tirth / Tiswin | ✅ Complete | Route authorization middleware enforcing Candidate, Recruiter, and Admin access. |

---

## 🛠 Tech Stack
- **Backend Runtime:** Node.js & Express.js (MVC Architecture)
- **Database:** MongoDB with Mongoose ODM
- **Authentication & Security:** JWT (JSON Web Tokens), `bcryptjs` password hashing
- **Validation & Errors:** `express-validator` middleware, centralized error handling
- **Frontend Demonstration UI:** HTML5, CSS3, JavaScript (ES6), Bootstrap 5.3 CDN
- **API Testing & Specification:** Postman Collection (`postman/Job-Portal-API.postman_collection.json`)

---

## 🗄 Collections & Database Design (ER Model)

```text
       +------------------+
       |      Users       | (Tanya / Tiswin)
       +--------+---------+
                |
     +----------+-----------+
     |                      |
+----+------------+   +-----+-------------+
| CandidateProfile|   |      Company      | (Tanya)
+-----------------+   +-----+-------------+
                            | 1:N
                      +-----+-------------+
                      |    JobPosting     | (Tanya)
                      +-----+-------------+
                            | 1:N
                      +-----+-------------+
                      |    Application    | (Sneha)
                      +--+-------------+--+
                         | 1:N         | 1:1
+------------------+     |       +-----+-------------+
|    Interview     +-----+       |       Offer       | (Tirth)
+------------------+             +-------------------+
```

### MongoDB Design Rationale:
- **Referencing (`ObjectId`):** Used for independently queried resources (`users`, `companies`, `jobPostings`, `applications`, `interviews`, `offers`) to prevent document bloating and unbounded growth.
- **Embedding:** Small scalar arrays such as `skills[]` in `JobPosting` and `CandidateProfile`, and `recruiterIds[]` in `Company` are embedded because they are read atomically with parent documents and rarely updated in isolation.
- **Indexes:**
  - `users.email` (unique index for fast credential lookups)
  - `jobPostings.companyId`, `candidateProfiles.userId`, `applications.jobId` (optimized foreign key lookups)

---

## 🚀 Quick Setup & Installation

### Prerequisites
- Node.js (v18 or higher recommended)
- MongoDB running locally at `mongodb://127.0.0.1:27017`

### Steps
1. **Clone the repository:**
   ```bash
   git clone https://github.com/tanya139/job_lnt_cia3.git
   cd job_lnt_cia3
   ```
2. **Install dependencies:**
   ```bash
   npm install
   ```
3. **Configure Environment Variables:**
   ```bash
   cp .env.example .env
   ```
   Ensure `.env` contains:
   ```env
   PORT=5000
   MONGODB_URI=mongodb://127.0.0.1:27017/job_portal
   JWT_SECRET=supersecretjwtkey_christ_cia3
   JWT_EXPIRES_IN=1d
   ```
4. **Seed Demo Data:**
   ```bash
   npm run seed
   ```
5. **Start the Application:**
   ```bash
   npm start
   # Or for development: npm run dev
   ```
6. **Open in Browser:** Visit `http://localhost:5000`

---

## 🔑 Demo Login Credentials (Seeded)

| Role | Email | Password | Accessible Views & Features |
|---|---|---|---|
| **Admin** | `admin@example.com` | `Admin@123` | System stats, funnel analytics, manage all records |
| **Recruiter** | `recruiter@example.com` | `Recruiter@123` | Company profile, job postings, pipeline stages, interview slots, offers |
| **Candidate** | `candidate@example.com` | `Candidate@123` | Search jobs, save jobs, apply, manage profile, accept/decline offers |

---

## 📡 Main REST API Endpoints

| Method | Endpoint | Access Role | Description |
|---|---|---|---|
| `POST` | `/api/auth/register` | Public | Register new Candidate or Recruiter |
| `POST` | `/api/auth/login` | Public | Authenticate user & return JWT token |
| `GET` | `/api/jobs/search` | Public | Filter jobs by title, skills, location, experience |
| `POST` | `/api/jobs` | Recruiter / Admin | Create a new job opening |
| `GET` | `/api/jobs/:id` | Public | Fetch job details |
| `POST` | `/api/companies` | Recruiter / Admin | Create or manage company profile |
| `GET` | `/api/candidates/profile` | Candidate | Fetch candidate profile & resume metadata |
| `PUT` | `/api/candidates/profile` | Candidate | Update profile, skills, experience |
| `POST` | `/api/applications` | Candidate | Submit application for a job opening |
| `PUT` | `/api/applications/:id/stage` | Recruiter / Admin | Advance applicant pipeline stage |
| `POST` | `/api/interviews` | Recruiter | Schedule candidate interview slot |
| `GET` | `/api/recruiter/applicants` | Recruiter / Admin | List applicants filtered by job posting |
| `POST` | `/api/saved-jobs` | Candidate | Bookmark a job posting |
| `POST` | `/api/job-alerts` | Candidate | Set automated job alert preference |
| `POST` | `/api/offers` | Recruiter | Issue formal job offer |
| `PUT` | `/api/offers/:id/status` | Candidate | Accept or reject offer |
| `GET` | `/api/admin/reports/funnel` | Admin | Aggregate recruitment funnel conversion metrics |

---

## 🧪 Postman Testing
The complete test suite is available in `postman/Job-Portal-API.postman_collection.json`.
It covers:
- **Happy Paths:** Registration, Login, Company creation, Job posting, Profile management, Application submission, Stage progression, Interview scheduling, Offer issuance, and Admin Analytics.
- **Validation Failures:** 400 Bad Request on missing fields or invalid email formats.
- **Authentication Failures:** 401 Unauthorized when Bearer token is omitted.
- **Authorization Failures:** 403 Forbidden when a Candidate attempts Recruiter/Admin operations.
- **Conflict Handling:** 409 / 400 on duplicate applications or duplicate email registration.

---

## 🌿 Git Branching Strategy & Contribution
- `main`: Stable production-ready codebase.
- `feature/sprint1-foundation-jobs`: Auth, Company, Job Posting, Search (Tanya)
- `feature/sprint2-candidate-pipeline`: Profile, Applications, Pipeline, Interviews (Sneha)
- `feature/sprint3-recruiter-reports-rbac`: Dashboard, Saved Jobs, Offers, Reports, RBAC (Tirth)
- `feature/infra-database-postman-docs`: Architecture, Schemas, Postman & PPT (Tiswin)
