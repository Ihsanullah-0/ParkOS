# ✅ ParkOS — Fixed Files Checklist

## 🐛 What was fixed:

1. **index.html** 
   - ✅ Added both Admin and Viewer role badges display
   - ✅ Better error messages
   - ✅ Console logging for debugging
   - ✅ Proper role detection from Firestore

2. **admin.html & viewer.html**
   - ✅ Fixed API key typo (`Xgv0v` → `XgvOv`)
   - ✅ All Firebase configs now match

---

## 📂 Files you now have:

| File | Purpose |
|------|---------|
| `index.html` | Login page (works for both admin & viewer) |
| `admin.html` | Full admin dashboard with all features |
| `viewer.html` | Read-only viewer dashboard |
| `VIEWER-SETUP-GUIDE.md` | Step-by-step guide to create viewer users |

---

## 🎯 Next steps:

### 1. Upload all 3 HTML files to the same folder

Make sure `index.html`, `admin.html`, and `viewer.html` are in the same directory.

### 2. Test your admin login

Open `index.html` in your browser:
- Email: `admin1@gmail.com`
- Password: (your password)
- Should redirect to **admin.html**

### 3. Create a viewer user

Follow **VIEWER-SETUP-GUIDE.md** to:
1. Add viewer user in Firebase Authentication
2. Create Firestore document with `role: "viewer"`
3. Update security rules

### 4. Test viewer login

Use the viewer email/password you created.
Should redirect to **viewer.html** (read-only dashboard).

---

## 🔍 Debugging tips:

### Check browser console (F12):

If something doesn't work, open DevTools and look for:
- `"Checking role for UID: ..."` — shows it's reading Firestore
- `"Role found: admin"` or `"Role found: viewer"` — role detection working
- `"Redirecting as: admin"` — about to redirect
- Any red error messages

### Common issues:

**"No account found with this email"**
→ User doesn't exist in Authentication. Add them first.

**"Incorrect password"**
→ Wrong password. Reset it in Firebase Console → Authentication.

**Redirects to wrong dashboard**
→ Check the Firestore `users` collection. The UID must match exactly, and `role` field must be `"admin"` or `"viewer"`.

**"Permission denied" when viewing data**
→ Update Firestore security rules (see VIEWER-SETUP-GUIDE.md).

---

## 🚀 You're ready!

All bugs are fixed. Follow VIEWER-SETUP-GUIDE.md to create your viewer user.
