# Cross-device invitation responses

Live sync is configured for Firebase project `date-invitation-sync`. The website uses Anonymous Authentication and a Standard Firestore database in `europe-west3` with real-time updates enabled. `firebase-config.js` contains the public web-app configuration needed by the static GitHub Pages site.

The deployed Firestore rules are scoped to this invitation's single response document:

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /artifacts/date-invitation-app/public/data/invitation_responses/current_response {
      allow read: if request.auth != null;
      allow create, update: if request.auth != null;
      allow delete: if false;
    }
  }
}
```

The rules are published in Firebase. Future code changes to `date_invitation.html` or `firebase-config.js` should be committed to the GitHub repository; GitHub Pages deploys them automatically.

Car Date responses store the non-empty song titles in the `songs` array alongside `dateType`, `location`, `date`, `time`, and `status`. The Host View renders the saved array as a numbered list.

## Access note

Anyone with the public invitation URL can anonymously sign in and read or change the response, and the Host View is not a protected admin login. This is suitable only for non-sensitive invitation responses. Do not put private information in the response document; a private host dashboard requires separate host authentication and a trusted backend. Firestore is free-tier eligible, but usage beyond the free quota can incur charges.
