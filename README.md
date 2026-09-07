# InSync

Dual-role menstrual cycle tracker and partner-support dashboard. React + Vite
frontend, Firebase (Auth, Firestore) backend, no server code required.

## What's implemented

- Anonymous auth on first launch, no email/password required
- Role selection (Tracker / Partner) → dual rose/slate themes
- One-time 6-character pairing code, permanent link once redeemed, 24h expiry on unused codes
- Tracker: multi-month calendar with flow/fertile-window/ovulation markers, daily log
  drawer (flow, pain scale, symptoms, BBT, cervical fluid, mood, sleep, stress, private notes)
- Cycle math: phase detection, next-period/ovulation prediction, rolling cycle-length
  variance and short-luteal-phase anomaly flags (advisory only, never silently reshapes
  predictions)
- Cycle-synced fitness & macro guidance per phase, with an automatic kcal buffer in luteal phase
- Product inventory tracker with a low-stock warning window before predicted period start
- Granular privacy matrix (phase, tips, mood, pain toggleable; private notes and BBT hardcoded private)
  that gates exactly what gets published to `partner_sync`
- Partner dashboard: live `onSnapshot` sync, In-Person/Long-Distance mode switch (stored in
  `localStorage`, zero extra reads/writes for the tracker), categorized tip cards, deterministic
  daily tip rotation from a 20+ prompt-per-category dictionary
- Unlink Partner (both sides), Reset Cycle History (soft), Full App & Account Reset (hard, with
  double confirmation)
- `firestore.rules` enforcing the schema's privacy boundaries

## What you'll need to do

1. **Create a Firebase project** at https://console.firebase.google.com
2. **Enable Anonymous Authentication**: Build → Authentication → Sign-in method → Anonymous → Enable
3. **Create a Firestore database**: Build → Firestore Database → Create database (production mode)
4. **Get your web app config**: Project settings → General → Your apps → Add app (Web) →
   copy the config values into a `.env` file at the project root (copy `.env.example` first)
5. **Deploy security rules**:
   ```bash
   npm install -g firebase-tools
   firebase login
   firebase init   # select this project's Firestore + Hosting, use existing files when prompted
   firebase deploy --only firestore:rules
   ```

## Local development

```bash
npm install
npm run dev
```

Open the printed localhost URL in two different browser profiles (or one normal + one
incognito window) to simulate a Tracker and a Partner pairing with each other.

## Deploying

```bash
npm run build
firebase deploy
```

## Logo asset

`src/assets/logo.png` is the sunburst/crescent-moon mark you provided. It's referenced as a
raster image rather than inline SVG — if you have the original vector file, drop it in as
`src/assets/logo.svg` and swap the `<img>` imports in `App.jsx`, `RoleSelect.jsx`,
`Pairing.jsx`, `TrackerApp.jsx`, and `PartnerApp.jsx`.

## Notes on scope / what to harden before real users

- Anomaly detection and daily tip publishing run client-side on the tracker's device. For a
  production app you'd likely move `publishPartnerSync` and anomaly checks into a scheduled
  Cloud Function so they run even when the tracker's app isn't open.
- The `pairCodes` collection rules are permissive (any signed-in user can read/write) to keep
  setup simple; tighten this with a Cloud Function-issued custom claim if you want stricter
  guarantees.
- Journal notes and BBT are stored in Firestore in plaintext within `private_logs`, which only
  the owner can read per the security rules. The spec mentions "encrypted" journal notes — true
  client-side encryption (e.g. via the Web Crypto API with a user-held key) isn't implemented
  yet and would be a good next addition if that's a hard requirement.
