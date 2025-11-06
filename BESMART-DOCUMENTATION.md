BE-SMART Hackathon Documentation

DAY 1 (learning + taking in):

Cahceing:

- Caching is a programming technique that stores copies of frequently accessed data in a high-speed temporary storage area (a cache) to speed up future requests by avoiding slower data retrieval from the original source. It improves performance, reduces load on primary systems like databases, and enhances user experience by decreasing latency and load times.

* Benefits of Cacheing:
  Initial request: When a program needs data, it first checks the cache. If the data is not in the cache, it fetches it from the slower, primary storage location (like a database or disk).
  Storing the data: The program then stores a copy of this data in the cache.
  Subsequent requests: When the program needs the same data again, it can retrieve it directly from the fast cache instead of going back to the original source.

<<switch>> and <<checkout>> (Briefly):

    - git checkout <branch> — older command used to switch branches or restore files.

    - git switch <branch> — newer, clearer command made only for switching branches (safer and easier to use).

    - 👉 Use git switch for branches, and git restore for files — both replace the older git checkout.

What is RDS (Amazon Relational Database Service):

    - Amazon Relational Database Service (Amazon RDS) is a web service that simplifies the setup, operation, and scaling of a relational database in the cloud. It provides cost-efficient and resizable capacity while automating time-consuming administration tasks such as hardware provisioning, database setup, patching, and backups. This allows users to focus on their applications and business needs rather than database management.

# 🧠 Day 2 — Planning & Learning the Architecture

## 🚀 Overview: How the Project Works End-to-End

### Backend (`server_side/`)

**Tech:** Express 5, Node.js, AWS (RDS + Bedrock)

#### Core Responsibilities

1. **Serves frontend**

   - Builds and serves the React app from `be-smart/dist`
   - Handles all routes (`/` + catch-all)

2. **Exposes API endpoints**
   - Route modules live in: `server_side/route_modules/`

#### Utilities

- `utils/error.js` — standardizes API responses
- `utils/endpoint_helpers.js` — handles authorization patterns
- `wrappers/aws_bedrock.js` — handles AI model calls (Llama/Titan via AWS Bedrock)
- `wrappers/database.js` — manages MySQL (AWS RDS) with:
  - Pooling
  - IAM authentication
  - Transactions

#### Configuration & Deployment

- **CORS:** preconfigured for `localhost` + EC2 domain
- **Deployment:** uses PM2 on EC2
- **Server:** listens on `PORT`, serves static SPA + API

---

### Frontend (`be-smart/`)

**Tech:** React 19 + Vite + TailwindCSS

#### Structure

- **Router:** in `src/App.jsx`
  - Currently one placeholder route → `DeleteME.jsx`
- **API Wrapper:** `src/utils/api.js`
  - Exposes `get`, `post`, `put`, `delete`
  - Uses `VITE_BASE_URL` as backend base URL
- **Build:** `vite build` → outputs to `dist/` (served by backend)

---

### Build & Deploy

#### Local Development

```bash
./run_app.sh

  Builds frontend
  Installs backend dependencies
  Starts the server




#### Production (EC2)

./deploy_on_ec2.sh

Builds frontend
Installs backend dependencie
Copies build artifacts
Runs with PM2

```

# 🎨 Frontend Developer Focus (Web + Mobile)

---

## 1. 🧩 App Skeleton & Navigation

**Tasks**

- Replace `DeleteME.jsx` with real pages
- Set up routes in `App.jsx`:
  - Public routes
  - Auth routes
  - 404 fallback
- Implement consistent layout:
  - Header / Nav / Footer
  - Responsive drawer for mobile

---

## 2. 💅 UI/UX & Styling

**Goals**

- Use **TailwindCSS** for responsive, mobile-first design

**Add:**

- Loading states
- Empty states
- Error states

**Define a consistent visual system:**

- Typography
- Color palette
- Spacing
- CTAs

**Build reusable components:**

- `Button`
- `Card`
- `Modal`
- `Input`
- `FormField`

**Extras**

- Use `react-loading-skeleton` for skeleton loaders

---

## 3. 🔄 State Management & Data Fetching

**Tasks**

- Centralize all API calls in `src/utils/api.js`
- Add state management via:
  - Local state + **React Query** OR simple custom hooks

**Handle:**

- Retries
- Timeouts
- User messaging for network errors

---

## 4. 🔐 Authentication (Optional / Hackathon Demo)

**Pre-installed Libraries**

- `google-one-tap`
- `react-google-button`
- `jwt-decode`

**Implement:**

- Google sign-in flow
- Store JWT client-side
- Attach token to requests via `api.js`

**Support:**

- Login / Logout UI
- Protected routes
- Role-based UI (if required)

---

## 5. 🌐 Backend Integration (HTTP Requests)

### Base URLs

- **Development:** `http://localhost:3000`
- **Production:** `http://ec2-52-15-61-144.us-east-2.compute.amazonaws.com:3000` (confirm)

### Example Setup

```js
// src/services/users.js
import api from "../utils/api";

// GET
export async function fetchUsers() {
  return api.get("/api/users");
}

// POST
export async function createUser(payload) {
  return api.post("/api/users", payload);
}
```

```
Auth Header Example
api.get('/api/secure', {
headers: { Authorization: `Bearer ${token}` },
});
```

Error Handling

Display messages based on RC response codes:

400 / 401 / 403 / 404 / 500

## 6. 🧭 Feature Pages (Hackathon Goals)

Identify Main Workflows

Example: user onboarding, data capture, AI interaction, dashboards

Add:

Forms with validation

Inline feedback

For AI features:

Backend handles Bedrock calls

Frontend sends inputs & displays responses

## 7. 📱 Mobile Strategy

Fastest Path

Responsive web app + PWA

Add manifest + icon

Test on real devices

If native needed:

Use Expo / React Native

Hit same APIs

Keep UI minimal

## 8. 🧩 Observability & Quality

Implement:

Client-side logging of API failures (dev-only)

Lightweight analytics (if allowed)

Accessibility basics:

Focus states

Alt text

ARIA attributes

Performance:

Code-splitting

Lazy-loading

Optimized images

## \9. ⚙️ Build & Environment Setup

.env.development
VITE_BASE_URL=http://localhost:3000

.env.production
VITE_BASE_URL=http://ec2-52-15-61-144.us-east-2.compute.amazonaws.com:3000

Checklist

Run vite build → confirm dist/ generated

Verify Express serves built app

Confirm CORS is functional

🧰 Making HTTP Requests from the Frontend

Use helper: src/utils/api.js

Pattern

api.get('/api/endpoint');
api.post('/api/endpoint', payload);
api.put('/api/endpoint', payload);
api.delete('/api/endpoint');

Error Handling

Display user-friendly messages using RC codes

🏆 What to Deliver for the Hackathon
Focus Description
Clear Story Judges follow the full flow in 3–5 min
Real Value Screens Onboarding → Input → AI Action → Output
Resilience Every async action: loading, success, error
Performance & Polish Fast, smooth, mobile-responsive
Zero-Setup Demo Prod URL “just works”, quick login if needed
Short Checklist

Configure .env (dev & prod)

Replace DeleteME.jsx with real pages

Set up routes (public/auth/404)

Build UI components + Tailwind responsiveness

Integrate APIs via api.js

Implement auth (Google/JWT if needed)

Build main flows (onboarding, AI, dashboard)

Add loading / empty / error states

Optimize for mobile (responsive/PWA)

Test locally + against EC2 backend

Prepare demo walkthrough script
