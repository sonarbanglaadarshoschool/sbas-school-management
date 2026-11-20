# SBAS Complete School Management Dashboard v9

A bilingual-ready (Bangla-first) school administration dashboard using HTML, CSS, vanilla JavaScript, Firebase Authentication and Cloud Firestore.

## Included modules
- Admin login and role/status verification (`users/{uid}` must have `role: admin` and `status: active`)
- Dashboard live counts and latest notices
- Students: add, edit, delete, search, auto IDs (`ST0001` onward)
- Teachers: add, edit, delete, search, auto IDs (`TC001` onward)
- Classes & Sections: optional section, class teacher, class-specific subjects, optional group; Grades 9 and 10 support Science, Humanities and Business Studies
- Attendance: date/class/section filtered present, absent, late and leave records
- Exams: create/edit/delete exam records, class-subject selection, enter/update marks and calculate percentage/letter grade
- Fees & Payments: record/edit/delete payment transactions and receipt numbers
- Notices: create/edit/delete, publish or draft
- Reports: live counts and CSV export
- Settings: school profile, contact details, academic year and grading note

## Deploy/update using GitHub web upload
1. Download and extract the ZIP.
2. Open the extracted `school-management-starter` folder (the project root in this ZIP).
3. Open your GitHub repository's **Add file → Upload files** page.
4. Select all files and folders inside this project folder, including `index.html`, `assets`, `firestore.rules`, and docs. Do not upload the ZIP itself and do not upload an extra parent folder.
5. Drop the files into GitHub. Commit with message `Deploy SBAS complete dashboard v9`.
6. If GitHub asks to replace files, accept. Keep the existing repository's `main` branch.
7. Wait for GitHub Pages to deploy, then hard-refresh the website (Ctrl+F5).

## Firebase requirements
- Firebase project config is in `assets/firebase-config.js`.
- Authentication: Email/Password provider enabled.
- Firestore: create database in production mode.
- Admin document: `users/{AUTH_UID}` with fields `role: "admin"`, `status: "active"`, and optional `name`.
- Add the GitHub Pages hostname to Authentication → Settings → Authorized domains.
- Publish the included `firestore.rules` in Firestore → Rules. These rules intentionally restrict all app data to active admins.

## Collections used
`users`, `students`, `teachers`, `classes`, `attendance`, `exams`, `results`, `fees`, `notices`, `settings`, `systemCounters`.

## Important notes
- This is an operational starter dashboard, not a payment gateway. Payment entries are manual records; it does not process money.
- The attendance module stores one document per date/class/student. If a student belongs to a section, keep the section value consistent between the student and class records.
- Class-specific subjects are stored as an array on each `classes` document. Exams can select subjects from that class list.
- Test with dummy records before entering live student data. Keep regular Firestore backups. Never share admin passwords.
- Firestore rules are admin-only. Do not make the database public to fix permission errors.
