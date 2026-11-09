# Enable cross-device invitation responses

The invitation now uses one fixed Firestore document and waits for Firebase Anonymous Authentication before saving. The current project has no real Firebase credentials, so live sync will remain unavailable until Firebase is configured.

## Firebase project setup

1. Create a Firebase project at https://console.firebase.google.com/ and register a Web app.
2. In **Project settings → Your apps**, copy the web app configuration into `firebase-config.js`, replacing `YOUR_API_KEY`, `YOUR_PROJECT_ID`, and `YOUR_APP_ID`. These web configuration values are intended to be visible in a static website; Firestore Security Rules, not API-key secrecy, protect the database.
3. In **Authentication → Sign-in method**, enable **Anonymous**.
4. Create a **Firestore Database**.
5. In **Firestore Database → Rules**, use the following rule for this invitation's one response document:

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /artifacts/date-invitation-app/public/data/invitation_responses/current_response {
      allow read, write: if request.auth != null;
    }
  }
}
```

6. Publish the rules and deploy `date_invitation.html`, `firebase-config.js`, and `index.html` together. GitHub Pages will then build the updated repository automatically.

## Access note

This rule allows any visitor who can anonymously sign in to read or overwrite the invitation response. The Host View is also a presentation mode, not a protected admin login. Use this setup only for non-sensitive invitation responses. Do not put private information in the document; a truly private host dashboard requires a trusted backend and separate host authentication.
