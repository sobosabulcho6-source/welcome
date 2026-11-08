# OEMS — Full-stack Online Examination Management System

OEMS includes a responsive browser frontend, a Node.js/Express API, and a PostgreSQL database. In production the Express service serves both the frontend and `/api`, so the browser and API share one domain. This avoids manually editing an API URL or configuring CORS after deployment.

## Included features

- Role-based sign-in: Administrator, Subject Teacher, and Student
- Admin management of schools, subjects, users, and teacher-subject assignments
- Scheduled examinations with multiple-choice and short-answer questions
- Server-side exam opening/closing times and duration enforcement
- Automatic answer saving and submission
- Automatic marking and results reporting
- Student account deactivation after submission and admin-controlled retakes
- Password hashing, JWT authentication, login rate limiting, and security headers
- PostgreSQL schema creation and first-admin creation on first startup
- Responsive layout for desktop, tablet, and mobile

## Deployment (GitHub + Render)

**Important:** GitHub is the code repository; GitHub Pages cannot run the Express backend or PostgreSQL. This project is configured so Render serves the frontend and backend together and provisions PostgreSQL. You only need to push this folder to GitHub and create the Render Blueprint once.

1. Create a GitHub repository and upload/push the complete contents of this folder. Keep `backend/`, `frontend/`, and `render.yaml` in the repository root exactly as shown.
2. Sign in to Render and choose **New → Blueprint**. Connect the GitHub repository containing this project and select `render.yaml`.
3. Render will create the PostgreSQL database and web service. When prompted, enter `ADMIN_PASSWORD` (at least 8 characters; use a strong, unique password). `JWT_SECRET` is generated automatically. `ADMIN_USERNAME` is `admin`.
4. Wait for deployment to finish. Open the Render service URL. Check `/api/health`; it should return `{"status":"ok"}`.
5. Open the service URL to use OEMS. Sign in with username `admin` and the `ADMIN_PASSWORD` you supplied. Change the password after the first login.

Subsequent GitHub pushes trigger automatic redeployment when auto-deploy is enabled on Render. No frontend API URL editing, database table creation, or manual SQL setup is required.

## Local development

Requirements: Node.js 20.19+ and a PostgreSQL database.

1. Create a local PostgreSQL database, for example `oems`.
2. Copy `backend/.env.example` to `backend/.env` and fill in the database URL, a random JWT secret of at least 32 characters, and an initial admin password of at least 8 characters.
3. In a terminal:

   ```bash
   cd backend
   npm install
   npm start
   ```

4. Open `http://localhost:3000`. The same server serves the frontend and API.

## Exam question format

Create an exam and enter one question per line:

```text
What is 2 + 2? | short | 4 | 1
Capital of Ethiopia? | mcq | Addis Ababa | 1 | Addis Ababa;Dire Dawa;Hawassa
```

Fields are `question | kind | correct answer | marks | options`. Use `short` or `mcq`. The options field is only needed for multiple-choice questions. Answer keys are not included in the question payload sent to students.

## Operational notes

- The initial admin account is created only when the database has no user with the configured admin username.
- Students are deactivated after submitting, as implemented in this edition. Admins can use **Allow retake** in Results to remove the result and reactivate the student.
- Teacher access to exams and results is limited by assigned subjects.
- Automatic marking compares normalized text answers and is not AI marking. Review answer keys before publishing exams.
- Render free services may sleep when idle; the first request after sleep may take time. Free database plans have limits and may expire. For real examinations, use a persistent paid database plan and maintain backups.
- Never commit `.env` or real passwords. Use HTTPS and a strong unique administrator password.
