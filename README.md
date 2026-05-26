# D&H SAT Practice Online

This is a static GitHub Pages website that uses Firebase as the backend.

## What Firebase Stores

- Firebase Authentication: student and admin accounts.
- Cloud Firestore: users, uploaded tests, questions, grading tables, and scores.
- Firebase Storage: uploaded SAT PDFs.

The website files are hosted on GitHub Pages. Firebase stores the live app data.

## Important Fixes in This Package

This package has been corrected so it is easier to launch:

- Uses script tags and Firebase CDN imports. No npm build is required.
- Login and password reset now use email only.
- Account creation now creates the Firebase Auth user first, then creates the Firestore profile. This fixes the common "Missing or insufficient permissions" sign-up error.
- Includes the missing GitHub Pages workflow at `.github/workflows/pages.yml`.
- Includes the required local vendor files for icons, PDF extraction, and optional OCR.
- Includes `.nojekyll` so GitHub Pages serves all folders normally.
- Keeps `firebase-config.js` as the only file you need to edit before launch.

## Folder Structure

```text
index.html
app.js
styles.css
firebase-config.js
firestore.rules
storage.rules
README.md
.nojekyll
.github/workflows/pages.yml
templates/
vendor/
```

## Step 1: Edit Firebase Config

Open `firebase-config.js`.

Replace the placeholder values with your Firebase Web App config from:

```text
Firebase Console > Project settings > General > Your apps > Web app
```

Your file should look like this:

```js
window.DH_FIREBASE_CONFIG = {
  apiKey: "YOUR_REAL_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_REAL_STORAGE_BUCKET",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

Important: copy the exact `storageBucket` value Firebase gives you. Some Firebase projects use `.appspot.com`, while newer ones may use `.firebasestorage.app`.

You do not need `measurementId` unless you are using Google Analytics.

## Step 2: Enable Firebase Authentication

Go to:

```text
Firebase Console > Build > Authentication > Sign-in method
```

Enable:

```text
Email/Password
```

Students and admins will log in with email and password.

## Step 3: Create Cloud Firestore

Go to:

```text
Firebase Console > Build > Firestore Database
```

Choose:

```text
Standard edition
Production mode / Locked mode
```

Then open:

```text
Firestore Database > Rules
```

Delete the default rules, paste everything from `firestore.rules`, and click **Publish**.

Important: do not paste Firestore rules into Realtime Database. If the rules editor shows JSON with `.read` and `.write`, you are in the wrong place.

## Step 4: Create Firebase Storage

Go to:

```text
Firebase Console > Build > Storage
```

Choose:

```text
Production mode / Locked mode
```

Then open:

```text
Storage > Rules
```

Delete the default rules, paste everything from `storage.rules`, and click **Publish**.

Important: do not paste Storage rules into Realtime Database. Storage rules should start with:

```js
rules_version = '2';

service firebase.storage {
```

## Step 5: Add Authorized Domains

After GitHub Pages gives you the final site URL, go to:

```text
Firebase Console > Authentication > Settings > Authorized domains
```

Add:

```text
YOUR-GITHUB-USERNAME.github.io
```

Do not include `https://` and do not include the repository path.

For local testing, also make sure `localhost` is authorized.

## Step 6: Upload to GitHub

Create a new GitHub repository.

Upload everything inside this folder, including hidden files and folders:

```text
.github/
.nojekyll
```

Then commit to the `main` branch.

## Step 7: Turn On GitHub Pages

In your GitHub repository, go to:

```text
Settings > Pages
```

Set the source to:

```text
GitHub Actions
```

Then go to the **Actions** tab and wait for the deployment to finish.

Your site will be available at:

```text
https://YOUR-GITHUB-USERNAME.github.io/YOUR-REPOSITORY-NAME/
```

## Step 8: Create the Admin Account First

Open your live site and create the first account.

The first account becomes the admin.

Do this before sharing the link with students.

If you previously tried to create an account and got a permissions error, delete that failed account from:

```text
Firebase Console > Authentication > Users
```

Then sign up again.

## Uploading a SAT

1. Log in as admin.
2. Open **Upload SAT**.
3. Upload the SAT question PDF.
4. Upload or paste the answer sheet.
5. Optional: upload or paste the grading table.
6. Use OCR only if the PDF is scanned or image-only.
7. Review every extracted question while comparing it with the PDF on the right.
8. Make sure every question has choices A, B, C, and D.
9. Make sure every correct answer is A, B, C, or D.
10. Click **Upload online**.

## Best Answer Sheet Format

```csv
module,question,answer,notes
RW1,1,A,
RW1,2,C,
RW2,1,B,
MATH1,1,D,
MATH2,1,A,
```

## Best Grading Table Format

```csv
section,raw,score
rw,0,200
rw,54,800
math,0,200
math,44,800
```

If no grading table is uploaded, the app estimates section scores from 200 to 800 based on raw correct answers.

## Common Errors

### Missing or insufficient permissions

Check these:

1. Firestore rules were pasted into **Firestore Database > Rules**.
2. Storage rules were pasted into **Storage > Rules**.
3. You did not paste either set into **Realtime Database > Rules**.
4. You deleted any failed Firebase Auth users from earlier tests.
5. You uploaded the corrected `app.js`.

### Website shows Firebase setup required

Your `firebase-config.js` still has placeholder values. Replace every `PASTE_...` value.

### PDF upload does not work

Check these:

1. Firebase Storage was created.
2. `storage.rules` were published in **Storage > Rules**.
3. Your `storageBucket` in `firebase-config.js` exactly matches Firebase.
4. You are logged in as the admin account.

### GitHub Pages 404

Check these:

1. `index.html` is in the repository root.
2. `.github/workflows/pages.yml` exists.
3. GitHub Pages source is set to **GitHub Actions**.
4. The Actions deployment finished successfully.

## Notes

- Do not publish private SAT PDFs unless you have permission to share them.
- Friend/student accounts need real emails if they want password reset links.
- GitHub Pages hosts the website files only.
- Firebase stores tests, PDFs, accounts, and scores online.
