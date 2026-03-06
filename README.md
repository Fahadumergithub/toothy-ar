# 🦷 ToothyAR

Real-time AR tooth detection in the mobile browser using YOLOv8 + TensorFlow.js.
Runs **100% on-device** — no server inference, zero latency.

## Stack
- **Frontend:** Vanilla HTML/CSS/JS (single file)
- **ML Runtime:** TensorFlow.js + TFLite WASM
- **Model Hosting:** GitHub Releases
- **App Hosting:** Firebase Hosting
- **CI/CD:** GitHub Actions → auto-deploy on push to `main`

---

## Repo Structure
```
toothy-ar/
├── public/
│   └── index.html          ← entire app
├── .github/
│   └── workflows/
│       └── deploy.yml      ← CI/CD pipeline
├── firebase.json
├── .firebaserc
└── README.md
```

---

## One-time Setup

### 1. Add Firebase Service Account to GitHub Secrets

1. Go to [Firebase Console](https://console.firebase.google.com) → Project Settings → Service Accounts
2. Click **"Generate new private key"** → download the JSON file
3. Go to your GitHub repo → Settings → Secrets and variables → Actions
4. Click **"New repository secret"**
5. Name: `FIREBASE_SERVICE_ACCOUNT`
6. Value: paste the **entire contents** of the downloaded JSON file
7. Click **"Add secret"**

### 2. Push this repo to GitHub

```bash
git init
git add .
git commit -m "Initial ToothyAR setup"
git branch -M main
git remote add origin https://github.com/Fahadumergithub/toothy-ar.git
git push -u origin main
```

GitHub Actions will automatically deploy to Firebase Hosting on every push to `main`.

---

## Live URL
Once deployed your app will be live at:
```
https://toothar-demo.web.app
```

---

## How It Works
1. On first load, the app downloads `best_float32.tflite` from GitHub Releases (~112MB)
2. The model is cached in the browser — subsequent loads are instant
3. Camera feed is captured frame by frame
4. Each frame is resized to 640×640, normalised, and fed to YOLOv8
5. Output tensor `[1, 11, 8400]` is parsed → NMS applied → bounding boxes drawn on canvas overlay

## Tooth Classes
| ID | Class | Colour |
|---|---|---|
| 0 | Premolar | `#00e5ff` |
| 1 | Canine | `#ff6b6b` |
| 2 | Central Incisor | `#51cf66` |
| 3 | Lateral Incisor | `#ffd43b` |
| 4 | Molar | `#cc5de8` |
| 5 | Missing | `#ff3d71` |
| 6 | Wisdom Tooth | `#74c0fc` |
