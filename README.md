# SBAS — School Management System

Bilingual (Bangla + English) school management starter using HTML, CSS, Vanilla JavaScript and Firebase.

## Included
- Firebase email/password admin login
- Firestore-backed student add, edit, delete, search and class filter
- Completely redesigned responsive admin workspace (new layout, navigation, cards, forms, tables and login screen)
- GitHub Pages deployment notes
- Blogger integration instructions and embed snippet

## Run locally
Open this folder in VS Code and use Live Server on `index.html`.

## Firebase
Check `assets/firebase-config.js` and configure Firestore Security Rules before production use. Do not use permissive public rules.

## Deployment
See `BLOGGER-INTEGRATION.md` for the GitHub Pages + Blogger setup.


## Student & Teacher Records (Updated)
- Student IDs are generated automatically as ST0001, ST0002, ... using Firestore transactions.
- Teacher IDs are generated automatically as TC001, TC002, ... using Firestore transactions.
- Student and teacher profiles include bilingual names, family, identity, contact, and address fields.
- Firestore collections: `students`, `teachers`, and `systemCounters`.

## Theme refresh v7
- Rebuilt the visual system around a clean school ERP workspace layout.
- Reworked sidebar navigation hierarchy, page header, dashboard hero, metric cards, quick actions, data tables, form sections, controls and login screen.
- Increased typography and input sizing for readability.
- Preserved Firebase Authentication, Firestore collections, student/teacher forms and ID generation.

## Classes & Sections module (v8)
- Firestore collection: `classes`
- Class names supported: Play, Nursery, KG, ১–১০
- Section is optional (for example A/B/Morning/Day).
- Grade ৯ and ১০ require exactly one group: বিজ্ঞান, মানবিক, or ব্যবসায় শিক্ষা.
- Optional class teacher is selected from the `teachers` collection; subjects are comma-separated.
- Includes create, list, search, edit, delete, duplicate prevention, and dashboard class count refresh.
