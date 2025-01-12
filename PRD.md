# Product Requirements Document (PRD)

## Table of Contents

1. [Overview](#1-overview)
2. [Monorepo Structure](#2-monorepo-structure)
3. [Project Goal & Objectives](#3-project-goal--objectives)
4. [Key Features](#4-key-features)
5. [Technical Requirements](#5-technical-requirements)
6. [Success Criteria](#6-success-criteria)
7. [User Stories](#7-user-stories)
8. [Timeline & Milestones](#8-timeline--milestones)
9. [Potential Risks & Mitigations](#9-potential-risks--mitigations)
10. [Open Questions](#10-open-questions)
11. [Versioning & Future Enhancements](#11-versioning--future-enhancements)

---

## 1. Overview

**Project Name:** Revi  
**Description:** Revi is a platform to **add**, **track**, and **manage** revisions under different "titles" or categories, including the ability to **upload images**, **mark revisions as complete**, and **add notes** in a chat-like view.

**Why “Revi”?**

- Many teams struggle with scattered feedback and countless revision requests.
- Revi aims to streamline this process by providing a centralized, intuitive interface to manage all revision-related tasks.

---

## 2. Monorepo Structure

This project is organized as a **monorepo** to keep frontend, backend, and database scripts/configurations in one place, ensuring easier cross-team collaboration and version control.

revi/
├── apps/
│ ├── nuxt-frontend/
│ │ ├── pages/
│ │ ├── components/
│ │ └── …
│ └── backend/
│ ├── src/
│ ├── controllers/
│ ├── routes/
│ └── …
├── database/
│ ├── migrations/
│ ├── seeders/
│ └── …
├── .gitignore
├── package.json
├── README.md
└── PRD.md

- **`apps/nuxt-frontend`**:  
  Contains the Nuxt 3 application for the frontend (pages, components, layouts, etc.).
- **`apps/backend`**:  
  Contains the Node.js application (Express or NestJS) that handles the business logic and exposes CRUD endpoints.
- **`database`**:  
  Contains PostgreSQL scripts (schema definitions, migrations, seed data, etc.).

---

## 3. Project Goal & Objectives

- **Goal**: Provide a simple, intuitive platform for managing multiple revision requests in a single interface, reducing confusion and context-switching.
- **Objectives**:
  1. Centralize all revisions under distinct titles/categories.
  2. Allow image uploads to give visual context to revision requests.
  3. Streamline communication and status updates (complete / in progress).
  4. Maintain a minimalistic UI that can be easily expanded or integrated with other tools in the future.

---

## 4. Key Features

1. **Sidebar with Titles/Categories**

   - A list of revision "titles" on the left for easy navigation.
   - Ability to add or remove titles dynamically.

2. **Dedicated Page per Title**

   - Each title’s page displays a chat-like list of revisions.
   - Shows each revision’s description, image thumbnail, timestamp, etc.

3. **Add/Delete Titles and Revisions**

   - “+ Add” button to create new revisions with an optional image.
   - Delete button to remove outdated or irrelevant revisions.

4. **Mark Revisions as Complete / Additional Notes**

   - Checkboxes or toggles to mark a revision as complete.
   - Inline notes or comments for further clarifications.

5. **Minimalistic UI & Header**

   - A top header with the application logo (left) and a placeholder user icon (right) for future authentication features.

6. **Image Upload**
   - Simple endpoint or local storage mechanism for uploading images.
   - Database references (`image_url`) to track files.

---

## 5. Technical Requirements

1. **Frontend: Nuxt (Nuxt 3)**

   - Utilize Nuxt’s file-based routing for a clean architecture.
   - Potential usage of **Pinia** (state management) or Vuex (if necessary).
   - SSR (Server-Side Rendering) can be considered or disabled, depending on deployment needs.

2. **Backend: Node.js**

   - Recommended frameworks: **Express** or **NestJS**.
   - Provide CRUD endpoints:
     - Titles: `/api/titles`
     - Revisions: `/api/revisions`
   - Optionally include a simple file upload endpoint (`/api/uploads`).

3. **Database: PostgreSQL**

   - Store data for `Titles` and `Revisions`.
   - Proposed schema:
     - **Titles Table**
       - `id` (Primary Key)
       - `name` (string)
       - `created_at` (timestamp)
     - **Revisions Table**
       - `id` (Primary Key)
       - `title_id` (Foreign Key to Titles)
       - `description` (text)
       - `image_url` (string, optional)
       - `is_marked` (boolean, default false)
       - `extra_notes` (text, optional)
       - `created_at` (timestamp)

4. **Image Handling**

   - **Local**: Store images in a public/uploads folder in the backend.
   - **Cloud** (Future Consideration): Could integrate with S3 or similar for production scalability.

5. **Auth Placeholder**
   - No complex authentication; just a placeholder user icon to illustrate potential future login flows.

---

## 6. Success Criteria

- **Ease of Use**: Users can quickly add titles and revisions without confusion.
- **Reliability**: The system handles basic concurrency (multiple users adding revisions simultaneously) without data conflicts or corruption.
- **Performance**: Page loads and revision updates feel snappy for typical usage (small to medium teams).
- **Scalability**: The codebase can be expanded to handle more complex features (e.g., real auth, file storage solutions, etc.).

---

## 7. User Stories

1. **Create a New Title**

   - “As a user, I want to create a new title so I can categorize my revisions.”
   - **Acceptance Criteria**: A button to add a title, appears instantly in the sidebar.

2. **Add a Revision with an Image**

   - “As a user, I want to attach an image to my revision so I can provide visual reference.”
   - **Acceptance Criteria**: A form that accepts an image upload and text; upon save, the revision appears in the title’s feed.

3. **Mark Revision as Complete**

   - “As a user, I want to mark a revision as complete so I can keep track of my progress.”
   - **Acceptance Criteria**: A checkbox or button that updates the revision’s status visually.

4. **Delete a Revision**

   - “As a user, I want to delete a revision that’s no longer relevant.”
   - **Acceptance Criteria**: A delete button that removes the revision from both the UI and the database.

5. **View Revisions in a Chat-like Feed**
   - “As a user, I want to see all revisions for a selected title in a single, scrollable feed.”
   - **Acceptance Criteria**: A list of revisions with timestamps, images, and descriptions arranged in chronological order.

---

## 8. Timeline & Milestones

| Milestone                         | Estimated Duration | Notes                                                        |
| --------------------------------- | ------------------ | ------------------------------------------------------------ |
| **Project Setup (Monorepo)**      | 1–2 days           | Initialize repo structure, configure package.json, etc.      |
| **Database & Backend Scaffold**   | 2–4 days           | Setup PostgreSQL scripts, CRUD endpoints (Titles, Revisions) |
| **Frontend (Nuxt) + Basic UI**    | 3–5 days           | Header, Sidebar, Routing, Basic Pages                        |
| **Image Upload & Chat-like Feed** | 2–4 days           | Implement file upload endpoint, dynamic feed layout          |
| **Refinement & QA**               | 1–2 days           | Bug fixes, UI polish, small improvements                     |
| **Total**                         | ~2–3 weeks         | Depending on team size and parallelization                   |

---

## 9. Potential Risks & Mitigations

- **Image Storage Complexity**

  - _Risk_: Large images could cause performance or storage issues.
  - _Mitigation_: Implement file size limits, consider compression, or eventually integrate cloud storage.

- **Scalability**

  - _Risk_: Increasing number of revisions might slow down query responses.
  - _Mitigation_: Use indexing in PostgreSQL on relevant columns (e.g., title_id), apply pagination as needed.

- **Future Authentication Needs**

  - _Risk_: If a full auth system is required later, it might require significant refactoring.
  - _Mitigation_: Keep routes and data structures flexible, with potential user-ownership fields.

- **UI/UX Overcomplication**
  - _Risk_: Feature creep could complicate the UI.
  - _Mitigation_: Stick to the minimalistic approach in MVP, gather user feedback before expanding.

---

## 10. Open Questions

1. **Hosting & Deployment**
   - Where will the monorepo be deployed (e.g., Docker, a PaaS, etc.)?
2. **File Storage Location**
   - Local or cloud (e.g., AWS S3) for production?
3. **Design Guidelines**
   - Are there brand colors or specific design guidelines to follow?
4. **Future Collaboration**
   - Will multiple users need to view/edit the same revisions simultaneously (real-time updates)?

---

## 11. Versioning & Future Enhancements

- **v1.0 (MVP)**

  - Basic CRUD for titles and revisions
  - Image upload (local)
  - Chat-like feed
  - Minimal UI and placeholder auth

- **v1.1+ (Potential Features)**
  - Real-time collaboration (WebSockets or similar)
  - Advanced user authentication (JWT, OAuth)
  - Role-based access control
  - Notification system for new revisions or changes
  - Cloud storage integration for images

---

### Final Notes

This PRD serves as a **living document**. As the project evolves, you can update sections to reflect any new requirements, architectural changes, or timeline adjustments. For clarity and collaboration, each significant PRD update can be reflected in GitHub commits and pull requests, ensuring transparency and traceability over the lifecycle of **Revi**.
