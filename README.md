# Elevare

# Elevare ⚡ — Engineering Placement Suite

> *Compile your future. Debug your career path. Deploy to production.*

Elevare is a single-page web app for managing college placement drives. It gives students a dashboard to track their CGPA-and-skill-based job matches, manage their profile/resume, and get an AI-generated "core fundamentals" study plan — while giving placement admins a control panel to configure recruiter requirements and browse eligible candidates.

> 🎓 Built as a 5th Semester Mini Project.

---

## 📸 Screenshots

**Login — Student / Admin access selection**

![Login screen](./screenshots/login.png)

**Student Dashboard — AI coach, resume center, and compatible companies**

![Student dashboard](./screenshots/student_dashboard.png)

**Admin Dashboard — placement stats and recruiter configuration**

![Admin dashboard](./screenshots/admin_dashboard.png)

**Admin — User Registry**

![Admin user registry](./screenshots/admin_registry.png)

---

## ✨ Features

### 🎓 Student Portal
- **Login by USN** — no separate password flow, session-based access
- **Profile config** — GitHub, LinkedIn, and a short career bio (used to personalize AI feedback)
- **Resume upload** — stores resume metadata + file data (PDF/DOC/DOCX/TXT, up to 400KB)
- **Compatible Nodes** — a live-scored list of companies the student qualifies for, based on CGPA threshold + skill overlap
- **One-click apply** — tracked in a "Deployment Pipeline" application tracker
- **AI Career Coach** — runs a Gemini-powered analysis of the student's profile and returns:
  1. **Kernel Panic** — gap analysis of missing CS fundamentals (DSA, OS, DBMS, Networks)
  2. **Patching Protocol** — an ordered study plan to close those gaps
  3. **Optimization Task** — a challenge project to apply the missing concepts
- **Knowledge Base** — auto-suggested books/courses based on the student's tech stack

### 🛠️ Admin Portal
- **Dashboard stats** — total students, active recruiters, and a CGPA-based "system health" metric
- **Recruiter config** — add companies with a minimum CGPA and required skill stack
- **Query matches** — pull a ranked list of eligible students for any company, with one-click JSON export
- **User registry** — view, inspect, or remove student records
- **Database controls** — wipe all student records (with confirmation)

---

## 🧱 Tech Stack

| Layer       | Technology |
|-------------|------------|
| UI          | HTML5, Tailwind CSS (CDN), Phosphor Icons |
| Fonts       | Outfit, Plus Jakarta Sans, JetBrains Mono (Google Fonts) |
| State/Logic | Vanilla JavaScript (ES Modules), custom render-loop pattern |
| Backend     | Firebase (Firestore + Anonymous Auth) |
| AI          | Google Gemini 1.5 Flash API |

The entire app lives in a single `index.html` file — no build step required.

---

## 🚀 Getting Started

1. **Clone the repo / download `index.html`.**

2. **Set up Firebase**
   - Create a Firebase project and enable **Firestore** and **Anonymous Authentication**.
   - Replace the `firebaseConfig` object near the top of the `<script>` block with your project's credentials.

3. **Set up Gemini API**
   - Generate an API key from [Google AI Studio](https://aistudio.google.com/).
   - Replace the `API_KEY` constant with your own key.

4. **Run it**
   - Open `index.html` directly in a browser, or serve it with any static file server:
     ```bash
     npx serve .
     ```

> ⚠️ **Security note:** This project currently has Firebase and Gemini credentials hardcoded directly in client-side JS for demo purposes. Before deploying publicly, move these to environment variables / a backend proxy, restrict your Firebase API key by domain, and add proper Firestore security rules — anyone can read your config from the page source otherwise.

---

## 🗂️ Data Model

**`students` collection** (`artifacts/{appId}/public/data/students`)

| Field          | Type     | Description |
|----------------|----------|-------------|
| `usn`          | string   | Unique student ID (used as document ID & login key) |
| `name`         | string   | Student name |
| `cgpa`         | number   | Cumulative GPA |
| `skills`       | string[] | Tech stack / skills |
| `bio`          | string   | Career objective (feeds the AI coach) |
| `github`       | string   | GitHub profile URL |
| `linkedin`     | string   | LinkedIn profile URL |
| `resume`       | object   | `{ fileName, fileData, uploadedAt }` |
| `applications` | array    | `{ companyId, companyName, status, timestamp }[]` |

**`companies` collection** (`artifacts/{appId}/public/data/companies`)

| Field             | Type     | Description |
|-------------------|----------|-------------|
| `name`            | string   | Company name |
| `min_cgpa`        | number   | Minimum CGPA cutoff |
| `required_skills` | string[] | Required tech stack |

---

## 🔑 Access Roles

- **Student** — logs in with their USN, sees only their own dashboard.
- **Admin** — logs in with a whitelisted email (`aradhya3124@gmail.com` or any `@admin.edu` address). From the admin panel, an admin can also "inspect" any student's dashboard view.

---

## 🧮 Matching Algorithm

A student is **eligible** for a company if:
1. Their CGPA ≥ the company's `min_cgpa`, **and**
2. They match **at least one** required skill (or the company has no skill requirements)

The **match score** shown to students combines:
- 50 points for meeting the CGPA bar
- Up to 50 points for the percentage of required skills matched

Companies are ranked highest-score-first for each student, and students are ranked by skill-match ratio for each company.

---

## 🤖 AI Coach Prompting

The AI coach sends the student's profile (name, USN, department, CGPA, skills, and bio) to Gemini with a system prompt that frames it as a "Senior Principal Engineer" focused on core CS fundamentals, returning a structured 3-part markdown response rendered in a retro terminal UI.

---

## 🛣️ Possible Improvements

- Replace anonymous auth with proper student/admin authentication
- Move resume storage to Firebase Storage instead of base64-in-Firestore
- Add Firestore security rules to restrict read/write access
- Move API keys server-side
- Add pagination for large student/company lists

---

## 📄 License

Add your preferred license here (e.g., MIT).
