MCQ STUDIO — FIREBASE SPARK PLAN PACKAGE
========================================

Files
-----
index.html       Updated MCQ Studio application
firestore.rules  Firestore security rules for this app
README.txt       Setup and limitations

Firebase project configured
---------------------------
Project ID: flutter-ai-playground-253f2
Plan: Spark (free)

Services used
-------------
- Firebase Authentication (Email/Password)
- Cloud Firestore
- Firebase Hosting (optional, Spark-compatible)
No Cloud Functions, Cloud Storage, paid APIs, or Blaze upgrade are used.

IMPORTANT FREE-PLAN SCORING LIMITATION
--------------------------------------
The app calculates quiz scores in the student's browser and writes the result
to Firestore. This is the practical no-Cloud-Functions approach. Firestore
rules verify that the student submits a result for their own account and
assigned grade, and prevent editing/deleting a saved result. However, rules
cannot verify that the browser calculated the correct score from the answer
key. A technically skilled user could manipulate the browser before saving.
Do not use these points as official examination marks or for high-stakes
decisions. Trusted, tamper-resistant automatic grading requires trusted
server-side code (such as Cloud Functions or another backend), which is not
included in this Spark-only package.

SETUP
-----
1. In Firebase Console, open Authentication > Sign-in method and ensure
   Email/Password is enabled.
2. Open Firestore > Rules. Replace all existing rules with firestore.rules
   from this package and click Publish.
3. Create the first administrator in Authentication > Users > Add user.
   Copy the new user's UID.
4. Open Firestore > Data. Create collection "users". Create a document whose
   document ID is exactly that UID. Add these fields:
      name       string  (e.g. School Administrator)
      email      string  (the admin email)
      role       string  admin
      active     boolean true
   Do not add stage or grade to the admin profile.
5. Host index.html on Firebase Hosting (Spark plan) or open it from a web
   server. For Firebase Hosting, install/use Firebase CLI on a computer,
   run `firebase login`, `firebase init hosting` in a folder containing
   index.html, select the existing project flutter-ai-playground-253f2,
   choose public directory as the current folder (or a folder containing
   index.html), configure as a single-page app: No, then run
   `firebase deploy --only hosting`.
   If you do not use a computer/CLI, you can test from a local development
   server. Avoid relying on file:// for final deployment.
6. Sign in as the admin. Open Students and create student accounts.
   Use valid email addresses and temporary passwords of at least 6 chars.
7. Open Import and download the Excel template. Fill it and import questions.

QUESTION IMPORT COLUMNS
-----------------------
Stage | Grade | Domain/Subject | Question | Option A | Option B |
Option C | Option D | Correct Answer | Explanation | Difficulty

Correct Answer may be A/B/C/D, 1/2/3/4, or the exact option text.
Stage values accepted include Foundational, Preparatory, Middle.
If Stage or Grade is omitted, the selections in the import form are used.
Subject should match the stage's subject/domain list.

DATA COLLECTIONS
----------------
users       One profile document per Firebase Authentication UID
questions   MCQs including correctIndex (readable by students of that grade)
results     Immutable student quiz submissions; admin can read all

SECURITY NOTES
--------------
- Admin role is bootstrapped manually in Firestore; never let a student create
  or edit their own role/profile.
- Keep the published Firestore rules. Do not switch to test mode.
- The question document includes the correct answer index because grading is
  browser-side on Spark. Students can technically inspect it. This is another
  reason the portal should be treated as practice/learning, not a secure exam.
- The app stores quiz answers in each result for review/audit; students can
  create only their own result records and cannot update/delete them.
- No student photo upload is included; Cloud Storage is not needed.

If you encounter a permission error, first verify the admin profile document
ID is exactly the Authentication UID and role is exactly "admin", active is
boolean true, and the latest firestore.rules were published.
