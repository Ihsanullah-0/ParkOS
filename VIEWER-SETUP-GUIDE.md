# Quick Setup Guide — Adding a Viewer User

## ✅ You've already completed:
1. Created Firebase project "ParkOS-Lot"
2. Enabled Email/Password authentication
3. Created admin user (admin1@gmail.com)
4. Created Firestore database
5. Added role: "admin" to user document

---

## 📋 Now add a VIEWER user (read-only access):

### Step 1: Create the viewer user in Authentication

1. Go to Firebase Console → **Authentication** → **Users** tab
2. Click **Add user** button
3. Enter:
   - **Email**: `viewer1@gmail.com` (or any email you want)
   - **Password**: Create a password (save it!)
4. Click **Add user**
5. **Copy the UID** that appears (looks like `prhtMkxsVCTRwYCs224Id6qUwRq2`)

---

### Step 2: Create the role document in Firestore

1. Go to Firebase Console → **Firestore Database** → **Data** tab
2. You should see the **users** collection (you created it for admin)
3. Click on **users** collection
4. Click **Add document** (the + button)
5. In the **Document ID** field: paste the viewer user's **UID** you copied in Step 1
6. Click **Add field**:
   - **Field**: `role`
   - **Type**: `string`
   - **Value**: `viewer`
7. Click **Save**

---

### Step 3: Update Firestore Security Rules

Go to Firestore → **Rules** tab and paste this:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Users collection — read own profile only
    match /users/{uid} {
      allow read: if request.auth.uid == uid;
      allow write: if false; // Only writable via console
    }

    // Helper function to check if user is admin
    function isAdmin() {
      return request.auth != null && 
             get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "admin";
    }

    // Parked vehicles — admin write, anyone authenticated read
    match /parked/{plate} {
      allow read: if request.auth != null;
      allow write: if isAdmin();
    }

    // Entry log — admin write, anyone read
    match /entryLog/{id} {
      allow read: if request.auth != null;
      allow write: if isAdmin();
    }

    // Exit log — admin write, anyone read
    match /exitLog/{id} {
      allow read: if request.auth != null;
      allow write: if isAdmin();
    }

    // Denied log — admin write, anyone read
    match /deniedLog/{id} {
      allow read: if request.auth != null;
      allow write: if isAdmin();
    }
  }
}
```

Click **Publish**

---

## 🧪 Test your users:

### Login as Admin:
- Email: `admin1@gmail.com`
- Password: (your admin password)
- Should redirect to → **admin.html** (full dashboard with all controls)

### Login as Viewer:
- Email: `viewer1@gmail.com`
- Password: (your viewer password)
- Should redirect to → **viewer.html** (read-only dashboard)

---

## 🔍 Troubleshooting:

### "Permission denied" errors:
- Make sure you published the Firestore Rules above
- Check that the user's UID in Firestore exactly matches the UID in Authentication

### User redirects to wrong dashboard:
- Open browser DevTools (F12) → Console tab
- Look for console.log messages showing the role
- Verify the Firestore document has `role: "admin"` or `role: "viewer"`

### Login doesn't work:
- Check browser console for errors
- Verify Firebase config is correct in all 3 HTML files
- Make sure all 3 files (index.html, admin.html, viewer.html) are in the same folder

---

## 📁 Your Firestore structure should look like:

```
parkos-lot (database)
├── users/
│   ├── prhtMkxsVCTRwYCs224Id6qUwRq2/
│   │   └── role: "admin"
│   └── [viewer-uid-here]/
│       └── role: "viewer"
├── parked/ (created when first vehicle enters)
├── entryLog/ (created on first entry)
├── exitLog/ (created on first exit)
└── deniedLog/ (created when first vehicle turned away)
```

---

## 🎯 What each user can do:

| Feature | Admin | Viewer |
|---------|-------|--------|
| View dashboard | ✅ | ✅ |
| View parked vehicles | ✅ | ✅ |
| View reports & logs | ✅ | ✅ |
| Vehicle entry | ✅ | ❌ |
| Vehicle exit | ✅ | ❌ |
| Download PDF tickets | ✅ | ❌ |
| Export reports | ✅ | ❌ |

---

## 🚀 You're all set!

Open `index.html` in your browser and test both accounts.
