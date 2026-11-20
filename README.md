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
