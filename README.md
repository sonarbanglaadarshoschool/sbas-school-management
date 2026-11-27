# SBAS School Management System — v10

Bilingual (Bangla + English) Firebase-backed school management dashboard for Sonar Bangla Adarsho School.

## Included
- Admin login through Firebase Authentication; admin role is checked in `users/{uid}`.
- Dashboard summary, Students, Teachers & Staff, Classes with class-specific subjects, Admission Applications, Attendance, Exams/marks entry, Fees/payments, Notices, Routine, Reports and Settings.
- Public online admission form at `admission.html`; applicants can submit a pending application without an account. Admin reviews it and can approve (creates a student record and ST ID) or reject it.
- Automatic Student IDs (`ST0001`) and Teacher IDs (`TC001`).
- Attendance statuses: present, absent, late and leave; daily records can be reviewed by date/class.
- Bangladesh board percentage-to-grade scale: 80–100 A+ (5.00), 70–79 A (4.00), 60–69 A- (3.50), 50–59 B (3.00), 40–49 C (2.00), 33–39 D (1.00), below 33 F (0.00).
- A4 print layout for the currently selected student's saved exam result. The current result-entry model stores marks per exam/subject; final multi-subject GPA, optional/4th-subject treatment, board-specific class rules and merit ranking require school-level confirmation before being used as official board-equivalent results.
- Firestore persistence. No payment gateway is included; Fees records payments entered by school staff.

## Existing project configuration
The `assets/firebase-config.js` file is prefilled with the Firebase Web App config supplied for project `sbas-2bfe2`. If Firebase Console shows a different Web App config, copy its values exactly into this file.

## Deploy to existing GitHub repository
1. Download and extract the ZIP.
2. Open the extracted `SBAS-Complete-Dashboard-v10` folder.
3. Upload the contents of that folder (not the ZIP itself) to the root of the existing repository: `https://github.com/sonarbanglaadarshoschool/sbas-school-management/upload/main`.
4. Replace the old `index.html`, `assets` files and add `admission.html`. Commit the changes.
5. Open the GitHub Pages site and hard-refresh with Ctrl+Shift+R.

## Important Firebase security setup
The public admission form requires a Firestore rule that permits only creation of pending admission applications. The updated `firestore.rules` file in this package includes this rule and keeps other collections admin-only.

To publish it: Firebase Console → Firestore Database → Rules → replace the rules with the contents of `firestore.rules` → Publish. Do not skip this step or the public admission form will be denied. Keep the admin user document at `users/{ADMIN_UID}` with `role: "admin"` and `status: "active"`.

The public form stores an optional compressed photo as a data URL limited to 350KB before encoding; rules cap the stored string. This avoids requiring Firebase Storage for the starter deployment. For a public production admission portal, enable Firebase App Check and add CAPTCHA/rate limiting to reduce spam.

## Verification / limitations
- JavaScript syntax and ZIP integrity were checked during packaging.
- This package has not been end-to-end tested against your live Firebase project from a browser. Please test with a sample admission, approve/reject, and verify all modules before real school records are entered.
- The current A4 result printout is a subject/result print layout, not a certified complete board marksheet until multi-subject GPA rules and optional-subject handling are configured.
- Fees are manual payment records, not online payment processing.
- The language toggle exists in the dashboard; some dynamically generated form labels are still Bangla-first and may not translate completely.

## Blogger embed (optional)
Use an iframe to embed the public-facing pages only. Do not expose the admin dashboard as a public Blogger page without access control.


## Classes ও Subjects (আলাদা মডিউল)
- **Classes:** শ্রেণি, রুম নম্বর এবং বিভাগ। বিভাগ কেবল নবম ও দশম শ্রেণির জন্য (মানবিক/বিজ্ঞান/কমার্স)।
- **Subjects:** সাবজেক্টের নাম, সাবজেক্ট কোড, সংশ্লিষ্ট ক্লাস এবং সাবজেক্ট টিচার।
- পরীক্ষা তৈরির সময় নির্বাচিত ক্লাস অনুযায়ী Subjects তালিকা দেখানো হয়।
- ক্লাস ও সাবজেক্ট Firestore-এ আলাদা `classes` এবং `subjects` collections-এ সংরক্ষিত হয়।
- পুরোনো `classes` ডকুমেন্টে আগে থেকে থাকা `subjects` array স্বয়ংক্রিয়ভাবে মুছে ফেলা হয় না; নতুন Subjects মডিউলে আনতে আলাদা migration প্রয়োজন হতে পারে।

## v12 usability update
- Added dropdowns for gender, blood group, religion, nationality, guardian relation, marital status, academic year, section, class group, fee type and payment method where structured choices are appropriate.
- Added keyword search and class/status filters to student and subject lists; added search/day filtering to routines and keyword filters to exam, fee and notice tables. Admission applications retain search and status filters; attendance uses date and class selectors.
- Student and teacher photos are stored as image URLs only. Paste a direct `https://` image link; no file upload/base64 image storage is used.
- Added English/Bangla language toggle support for the admin interface and public admission page. Dynamic interface labels are translated using the built-in label dictionary.
- Refined form controls, table readability, spacing, focus states and responsive layouts.

Note: English translation coverage is best-effort for interface labels and common status/option text. User-entered names, addresses, notice content and other records are intentionally not machine-translated.


## v15 Audit & Repair Notes

- Student and teacher IDs are generated only when a new record is saved. They are hidden from the entry forms and displayed in the saved record list and success message.
- Attendance has been rebuilt with date, class and section selection, class/group-aware student matching, saved attendance prefill, status totals, and batch saving.
- Attendance date values now use the browser's local calendar date. Present and Late are both counted as attended in the dashboard/report percentage.
- Public admission now sends a direct `photoURL` string (optional) and normalized gender values. The supplied `firestore.rules` file has been aligned with this payload; update the rules in Firebase Console before using the public admission form.
- Form controls and tables have a refined compact UI, and the attendance screen has a dedicated responsive layout.
- The shared table filter has only an active search control; a non-functional placeholder dropdown was removed.

### Important deployment note
The `firestore.rules` file is included for review. If the live Firebase project currently uses different rules, back them up and publish the included rules in Firebase Console only after confirming that they match your deployment and access policy. JavaScript syntax and package integrity checks do not replace a live Firebase end-to-end test.

## v17 hotfix
- Fixed the Classes form crash: the grade dropdown referenced `gradeList` without declaring it, preventing the class form from rendering.
- Added the complete Play, Nursery, KG and Grades 1–10 options in Bengali and English.
- Verified `assets/app.js` with Node.js syntax check and verified the ZIP archive integrity.


## v22 updates (November 2026)
- Student directory now displays student photo thumbnails and sorts by class/roll.
- Teacher directory now displays teacher photo thumbnails.
- Class directory is sorted in school-grade sequence.
- Removed the duplicate exam-create action; exam creation supports multiple classes and automatically includes subjects assigned to those classes.
- Marks entry is student-wise and includes all applicable subjects with per-subject full marks.
- Results include a View Marksheet action with A4 portrait print layout.
- ID card front/back print on one A4 portrait sheet with a 5 mm gap.
- Payment list includes a printable/downloadable quarter-A4 receipt.
- Added responsive styles for image thumbnails, exam setup and subject-wise marks entry.

Note: Run end-to-end tests with the live Firebase project and confirm Firestore rules allow the `exams` and `results` writes before production use.


## v23 — Exam & Board-style results update
- Exams and Results are now separate sidebar pages.
- Exam class selection uses individual checkboxes. Selecting classes loads every linked subject.
- Each subject can be marked as compulsory or optional/fourth subject.
- Bangladesh education-board letter grades are applied by percentage: A+ 80–100 (5.00), A 70–79 (4.00), A− 60–69 (3.50), B 50–59 (3.00), C 40–49 (2.00), D 33–39 (1.00), F 0–32 (0.00).
- Any F in a compulsory subject means fail. Optional-subject bonus is max(GP−2, 0), added to compulsory grade-point total and divided by the number of compulsory subjects; GPA is capped at 5.00.
- Result search and A4 academic transcript are available from the Results page. Transcript clearly identifies itself as a school-issued internal marksheet, not an official Board transcript.
- Existing results remain visible; legacy exams without an optional-subject designation treat all subjects as compulsory.

## v24 Result workflow update
- Results page now follows the requested flow: select an exam, select a class, then load the students.
- Each student row shows Student ID, name, class, roll, total marks, GPA, Edit Marks, and View Marksheet.
- Edit Marks opens a student-specific dialog listing every subject for that exam/class, the full marks (default 100 or exam-defined value), and the obtained-mark input.
- Saving writes the student's result to `results/{examId}_{studentDocumentId}` and refreshes the student table. View Marksheet uses the saved subject marks and the existing A4 marksheet printer.
- The legacy marks-entry tab inside the Exams page is retained; the new student-first workflow is on the Results page.


## v25 — Pro UI & Public Portal Configuration
- Refreshed the dashboard and public admission page with a consistent premium, colorful responsive UI theme.
- Added a dedicated Public Website Settings section under Settings for portal visibility, school name, title/subtitle, announcement, contact details, website/Facebook links, logo URL and banner image URL.
- Public portal configuration is stored separately in `publicPortal/config`; the public admission page reads only this published configuration.
- Added a Firestore rule allowing unauthenticated read access only to `publicPortal/config`; create/update/delete remain admin-only. Publish the updated `firestore.rules` in Firebase Console before using public portal settings.
- Added controls to pause the public portal or temporarily stop new admission submissions.
- Student, teacher, admission photo, school logo and banner images use direct HTTPS image URLs; no image file upload or base64 image storage is used.
- Existing student, teacher, admission, fee, exam, result and other data field names and collections are retained.

### Public portal image URL guidance
Use a direct HTTPS URL that opens the image file itself (JPG, PNG or WEBP). A webpage/share link (for example a generic Google Drive sharing page) may not render as an image.


## v26 form layout refinement
- Reworked all `.form-grid` data-entry forms to use compact, two-column, label-left / input-right rows based on the supplied Student Application screenshot.
- Public admission form uses the same aligned layout.
- Kept existing form fields, names, option values, Firestore collection names and JavaScript workflows unchanged in this CSS-only refinement.
- Dashboard and navigation retain their existing theme; form surfaces are intentionally restrained white/green/gray to match the supplied reference.
