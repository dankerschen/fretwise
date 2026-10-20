# Fretwise — guitar chord, progression & scale trainer

A single-page guitar learning tool: chord charts, progressions, scales and modes, practice games and a random drill.
Open `index.html` in a browser to use it. Progress is saved in that browser.

To let friends use it from anywhere, each with their own saved progress, host it on GitHub Pages and connect a free Firebase project. One-time setup, about 15–20 minutes.

## 1. Put it on GitHub Pages

No git needed, everything happens on the GitHub website.

1. Sign in at <https://github.com>, click **+ → New repository**, name it e.g. `guitar-tool`, set it to **Public**, tick **Add a README file**, and click **Create repository**.
2. In the new repository click **Add file → Upload files**. Drag in `index.html`, `firebase-config.js`, `firestore.rules` and `README.md` from this folder. Click **Commit changes**; it's fine to replace the README GitHub created.
3. Go to **Settings → Pages → Build and deployment**. Choose **Deploy from a branch**, branch `main`, folder `/ (root)`, then **Save**.
4. After a minute or two the site is live at `https://<your-username>.github.io/guitar-tool/`. The Pages settings page shows the link once it's ready.

**Updating later:** after changing a file here, upload it the same way (**Add file → Upload files**, then **Commit changes**). It replaces the old version, and the site updates within a couple of minutes. To edit a small thing like `firebase-config.js` directly on GitHub, open the file, click the pencil icon, edit, and commit.

## 2. Create the Firebase project (accounts + storage)

1. Go to <https://console.firebase.google.com>, click **Create a project**. Google Analytics isn't needed.
2. **Add a web app**: on the project overview click the `</>` (Web) icon, give it a name. Hosting isn't needed. Firebase shows a `firebaseConfig = { ... }` object.
3. Copy that object into `firebase-config.js`, replacing `null`:
   ```js
   window.FIREBASE_CONFIG = { apiKey: "...", authDomain: "...", projectId: "...", storageBucket: "...", messagingSenderId: "...", appId: "..." };
   ```
   These values are safe to commit. They only identify the project; the database rules below decide who can read what.
4. **Turn on sign-in**: *Build → Authentication → Get started → Sign-in method*, enable **Google** and **Email/Password**.
5. **Allow your site**: *Authentication → Settings → Authorized domains → Add domain*, enter `<your-username>.github.io`.
6. **Create the database**: *Build → Firestore Database → Create database*. Pick a location near you and start in **production mode**.
7. **Set the rules**: in Firestore, open the **Rules** tab, replace everything with the contents of `firestore.rules`, and click **Publish**. This makes each person's progress private to them.
8. Upload the updated `firebase-config.js` to the repository, or edit it on GitHub with the pencil icon. The **Sign in** button appears on the live site.

The free Spark plan covers this comfortably: up to 50k document reads and 20k writes a day.

## How saving works

- **Not signed in:** progress is saved in that browser only.
- **Signed in:** drill ratings and best chord-change scores are saved to the person's account and follow them to any device. Settings such as theme and note names stay per device.
- On the first sign-in in a browser, the tool offers to add any progress already made there to the account.
- Signing out on a shared computer leaves no account data behind in that browser.

## Testing sign-in

Sign-in doesn't work when `index.html` is opened directly as a file; it then saves to that browser only, with no Sign in button. Test accounts on the live GitHub Pages site instead. Opening the local file is still fine for checking everything else before you upload.
