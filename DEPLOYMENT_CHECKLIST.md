# OEMS Deployment Checklist

## Before deploying
- [ ] Confirm `backend/.env` is not committed.
- [ ] Confirm the Render database plan has suitable retention for your exam records.
- [ ] Push the complete project to GitHub.

## Render
- [ ] Create a Blueprint from the repository (root `render.yaml`).
- [ ] Set `FRONTEND_ORIGIN` to `https://YOUR-USERNAME.github.io` (origin only).
- [ ] Set a strong `ADMIN_PASSWORD`.
- [ ] Wait for the API and database deployment.
- [ ] Check `https://YOUR-API.onrender.com/api/health` and confirm `status: ok`.

## GitHub Pages
- [ ] In repository Settings > Pages, select **GitHub Actions** as the source.
- [ ] Set `frontend/js/config.js` `API_URL` to `https://YOUR-API.onrender.com/api`.
- [ ] Push to `main` or `master` and wait for the Pages workflow to complete.
- [ ] Open the GitHub Pages URL and sign in as `admin`.

## Acceptance test before real exams
- [ ] Create a school and at least one subject.
- [ ] Create a teacher and assign the subject.
- [ ] Create a student account (or use Add many students).
- [ ] Create a short test exam and import a text-based PDF.
- [ ] Confirm student answers autosave and the server finalizes an expired exam.
- [ ] Confirm teachers can only see exams/results for assigned subjects.
- [ ] Confirm results and CSV export are correct.
- [ ] Test retake workflow.
- [ ] Take a database backup and test restoring it before using real records.
