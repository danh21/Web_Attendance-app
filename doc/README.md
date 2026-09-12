# Create Your Own Firebase Project

1. Open the [Firebase Console](https://console.firebase.google.com/) and select
	 **Add project**.
2. Enter a project name, such as `attendance-app-your-name`, and complete the
	 setup steps.
3. From the project overview, click the Web icon `</>` to add a Web app.
4. Enter an app name, such as `attendance-web`, and select **Register app**.
5. Firebase will display a `firebaseConfig` object. Keep these values for the
	 next step.

## 1. Replace `firebaseConfig` in the source

Open both files:

- `src/login.html`
- `src/index.html`

In each file, find the block that starts with `const firebaseConfig` or
`var firebaseConfig` and replace the entire object with the configuration from
your new Firebase project:

```javascript
const firebaseConfig = {
	apiKey: "YOUR_API_KEY",
	authDomain: "YOUR_PROJECT.firebaseapp.com",
	databaseURL: "https://YOUR_PROJECT-default-rtdb.firebaseio.com",
	projectId: "YOUR_PROJECT",
	storageBucket: "YOUR_PROJECT.firebasestorage.app",
	messagingSenderId: "YOUR_SENDER_ID",
	appId: "YOUR_APP_ID"
};
```

Keep the property names and values provided by Firebase. If the Firebase
Console shows a `storageBucket` ending in `.appspot.com` instead of
`.firebasestorage.app`, use the value shown in the Console. The configuration
must be replaced in **both files**. If it is changed only on the login page,
login may succeed while the dashboard still connects to the old project.

The `apiKey` in a Firebase Web config is not a password or a secret. However,
data access must be protected with Authentication and Firestore Security Rules,
not by hiding the `apiKey`.

## 2. Enable email/password sign-in

1. Open the Firebase Console for your new project.
2. Select **Build > Authentication**.
3. Select **Get started** if Authentication has not been initialized yet.
4. Open the **Sign-in method** tab.
5. Select **Email/Password**.
6. Enable **Email/Password**. Leave **Email link (passwordless sign-in)**
	 disabled unless you need it, then select **Save**.

## 3. Create an email and password for login

1. In **Authentication**, open the **Users** tab.
2. Select **Add user**.
3. Enter an email and a strong password, for example
	 `admin@example.com` and a strong password.
4. Select **Add user**.
5. Use this email and password on `src/login.html`.

Do not write passwords in the source code or commit them to Git. Create new
users directly in the Firebase Console or use a separate backend
administration process.

## 4. Create Cloud Firestore and set the Rules

1. Go to **Build > Firestore Database**.
2. Select **Create database**.
3. Choose a suitable region and complete the database setup.
4. Open the **Rules** tab and replace its contents with these minimum Rules:

```text
rules_version = '2';

service cloud.firestore {
	match /databases/{database}/documents {
		match /{document=**} {
			allow read, write: if request.auth != null;
		}
	}
}
```

5. Select **Publish**.

These Rules allow every signed-in user to read and write data. They match the
current code because the dashboard reads the `Employee` collection and its
`Employee/{employeeId}/Record` subcollections. Do not use these broad Rules in
production when users need different permissions.

### Stricter Rules for production

If the dashboard only needs to read data and must not write from the browser,
use:

```text
rules_version = '2';

service cloud.firestore {
	match /databases/{database}/documents {
		match /{document=**} {
			allow read: if request.auth != null;
			allow write: if false;
		}
	}
}
```

For user-specific permissions, design role data or custom claims and write
Rules for each collection. Never use `allow read, write: if true;` because it
opens the entire database to unauthenticated users.

## 5. Verify the configuration

1. Start an HTTP server from the project directory.
2. Open `http://localhost:8000/src/login.html`.
3. Sign in with the user you created.
4. Confirm that the browser redirects to `index.html`.
5. In Firestore, create sample data with this structure:

	 ```text
	 Employee/{employeeId}
	 Employee/{employeeId}/Record/{recordId}
	 ```

6. Reload the dashboard and confirm that the attendance list is displayed.

If login succeeds but the list is empty, check that the collection name uses
the correct capitalization: `Employee`. Also check that each record is in the
`Record` subcollection. For a `Missing or insufficient permissions` error,
verify that the user is signed in, the Rules have been published, and both
HTML files use the same `projectId`.
