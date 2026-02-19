<div align="center">

# ⚡ HabitFlow

### Full-Stack Habit & Goal Tracker

**Build habits that actually stick. Track streaks, visualize progress, and achieve your goals.**

[![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)](https://mongodb.com)
[![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com)
[![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://reactjs.org)
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org)
[![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)](https://jwt.io)

![HabitFlow Dashboard Preview](https://via.placeholder.com/900x480/0a0a0b/f59e0b?text=HabitFlow+Dashboard)

</div>

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [API Reference](#-api-reference)
- [Seed Data](#-seed-data)
- [Email Reports](#-email-reports)
- [Deployment](#-deployment)
- [Contributing](#-contributing)

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔐 **Authentication** | JWT-based register & login with bcrypt password hashing |
| ◎ **Habit Tracking** | Daily & weekly habits with custom icons, colors, and categories |
| 🔥 **Streak System** | Auto-calculated current & longest streaks per habit |
| 🎯 **Goal Management** | Goals with sub-tasks, milestones, priority levels, and status tracking |
| 📊 **Progress Charts** | Area, bar, and pie charts powered by Recharts |
| 📅 **Calendar Heatmap** | Monthly view with 5-level completion intensity |
| 📧 **Email Reports** | Beautiful HTML progress reports sent via Nodemailer |
| 📱 **Responsive Design** | Works seamlessly on desktop and mobile |

---

## 🛠 Tech Stack

### Backend
- **Node.js** + **Express** — REST API server
- **MongoDB** + **Mongoose** — Database & ODM
- **JWT** — Stateless authentication
- **bcryptjs** — Password hashing
- **Nodemailer** — Transactional email
- **express-validator** — Input validation

### Frontend
- **React 18** + **React Router v6** — SPA with client-side routing
- **Recharts** — Data visualization
- **Axios** — HTTP client with interceptors
- **Context API** — Auth state management
- **CSS Variables** — Design system theming

---

## 📁 Project Structure

```
habitflow/
├── backend/
│   ├── models/
│   │   ├── User.js          # Auth + stats schema
│   │   ├── Habit.js         # Habit with completions array & streaks
│   │   └── Goal.js          # Goal with sub-tasks & milestones
│   ├── routes/
│   │   ├── auth.js          # Register, login, profile
│   │   ├── habits.js        # CRUD + toggle complete + stats + calendar
│   │   ├── goals.js         # CRUD + sub-task management
│   │   └── email.js         # HTML progress report email
│   ├── middleware/
│   │   └── auth.js          # JWT protect middleware
│   ├── seed.js              # Demo data seeder
│   ├── server.js            # Express entry point
│   └── .env.example
│
├── frontend/
│   └── src/
│       ├── context/
│       │   └── AuthContext.js
│       ├── utils/
│       │   └── api.js           # Axios client + interceptors
│       ├── components/
│       │   ├── Layout.js        # Sidebar navigation
│       │   └── Toast.js         # Toast notification system
│       └── pages/
│           ├── AuthPage.js      # Login / Register
│           ├── Dashboard.js     # Overview + today's habits
│           ├── HabitsPage.js    # Habit management
│           ├── GoalsPage.js     # Goal management + sub-tasks
│           ├── CalendarPage.js  # Heatmap calendar
│           ├── ProgressPage.js  # Charts & analytics
│           └── EmailPage.js     # Send progress reports
│
└── package.json             # Root scripts with concurrently
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** v18+
- **MongoDB** running locally or a [MongoDB Atlas](https://cloud.mongodb.com) URI
- **npm** or **yarn**

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/habitflow.git
cd habitflow
```

### 2. Install Dependencies

```bash
# Install root dependencies (concurrently)
npm install

# Install backend dependencies
cd backend && npm install

# Install frontend dependencies
cd ../frontend && npm install
```

### 3. Configure Environment

```bash
cd backend
cp .env.example .env
```

Edit `.env` with your values — see [Environment Variables](#-environment-variables) below.

### 4. Run the App

```bash
# From the root directory — starts both servers simultaneously
npm start

# Or run separately:
npm run start:backend    # API server on http://localhost:5000
npm run start:frontend   # React app on http://localhost:3000
```

### 5. Seed Demo Data (Optional)

```bash
cd backend
node seed.js
```

This creates two demo accounts with 60 days of habit history, goals, and sub-tasks — see [Seed Data](#-seed-data).

---

## 🔑 Environment Variables

Create `backend/.env` using the template below:

```env
# Server
PORT=5000

# MongoDB
MONGO_URI=mongodb://localhost:27017/habittracker

# JWT
JWT_SECRET=your_super_secret_key_minimum_32_characters
JWT_EXPIRE=30d

# SMTP Email (Gmail example)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=yourname@gmail.com
EMAIL_PASS=your_16char_app_password

# Frontend URL (for CORS)
CLIENT_URL=http://localhost:3000
```

> **Gmail Setup:** Go to Google Account → Security → 2-Step Verification → App Passwords. Generate a password for "Mail" and use that 16-character string as `EMAIL_PASS`.

---

## 📡 API Reference

All endpoints except `/auth/register` and `/auth/login` require:
```
Authorization: Bearer <token>
```

### Authentication

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/auth/register` | Create new account |
| `POST` | `/api/auth/login` | Login and receive JWT |
| `GET` | `/api/auth/me` | Get current user |
| `PUT` | `/api/auth/profile` | Update profile |
| `PUT` | `/api/auth/change-password` | Change password |

### Habits

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/habits` | List all habits |
| `GET` | `/api/habits/today` | Today's habits with completion status |
| `GET` | `/api/habits/stats` | Stats + 30-day completion data |
| `GET` | `/api/habits/calendar?year=&month=` | Monthly calendar heatmap data |
| `POST` | `/api/habits` | Create a habit |
| `PUT` | `/api/habits/:id` | Update a habit |
| `POST` | `/api/habits/:id/complete` | Toggle completion for a date |
| `DELETE` | `/api/habits/:id` | Delete a habit |

### Goals

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/goals` | List goals (filter: `?status=in_progress`) |
| `GET` | `/api/goals/:id` | Get single goal |
| `POST` | `/api/goals` | Create a goal |
| `PUT` | `/api/goals/:id` | Update goal (auto-calculates progress) |
| `POST` | `/api/goals/:id/subtasks` | Add sub-task |
| `PUT` | `/api/goals/:id/subtasks/:taskId` | Toggle / update sub-task |
| `DELETE` | `/api/goals/:id/subtasks/:taskId` | Delete sub-task |
| `DELETE` | `/api/goals/:id` | Delete goal |

### Email

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/email/progress-report` | Send HTML progress report |

<details>
<summary><strong>Example: Toggle Habit Completion</strong></summary>

```bash
POST /api/habits/64abc123/complete
Authorization: Bearer eyJhbGc...

{
  "date": "2026-02-18",   // optional, defaults to today
  "note": "Great session",
  "value": 1
}

# Response:
{
  "success": true,
  "isCompleted": true,
  "habit": { ... }
}
```

</details>

<details>
<summary><strong>Example: Send Progress Report</strong></summary>

```bash
POST /api/email/progress-report
Authorization: Bearer eyJhbGc...

{
  "recipientEmail": "client@company.com",
  "recipientName": "Sarah",
  "clientName": "Acme Corp",
  "message": "Great progress this week! On track for Q1 goals."
}
```

</details>

---

## 🌱 Seed Data

Running `node seed.js` from the `backend/` directory populates the database with realistic demo data:

### Demo Accounts

| Email | Password | Description |
|---|---|---|
| `alex@demo.com` | `password123` | Full dataset — 12 habits, 6 goals |
| `sam@demo.com` | `password123` | Light dataset — 3 habits |

### What Gets Seeded (Alex Rivera)

**12 Habits** across all categories, each with **60 days of randomized completion history** (60–93% hit rates):

> Morning Meditation · Run/Jog · Read 30 Pages · Drink 2L Water · Strength Training · Daily Journaling · No Social Media before 10am · Learn Spanish · Eat Vegetables · Sleep by 11pm · Code Side Project · Call Family

**6 Goals** at varying completion stages:

| Goal | Status | Progress |
|---|---|---|
| Run a Half Marathon | In Progress | 45% |
| Launch Finance Dashboard | In Progress | 62% |
| Read 24 Books This Year | In Progress | 58% |
| Emergency Fund: $15,000 | In Progress | 73% |
| Reach B2 Spanish | In Progress | 35% |
| AWS Solutions Architect Cert | Not Started | 0% |

> ⚠️ Running `seed.js` **clears all existing data** before inserting. Don't run it in production.

---

## 📧 Email Reports

HabitFlow can send rich HTML progress reports to clients or stakeholders.

### What's Included
- Habit completion stats for today
- Average goal progress percentage
- Top streaks leaderboard
- Active goals with visual progress bars
- Custom personal message
- Plain-text fallback for all email clients

### Email Client Compatibility
The template uses **table-based layout with fully inline styles** and **base64 transfer encoding** — compatible with Gmail, Outlook, Apple Mail, and Yahoo Mail.

---

## 🚢 Deployment

### Backend — Railway / Render

1. Connect your GitHub repo
2. Set root directory to `backend/`
3. Set environment variables in the dashboard
4. Use a [MongoDB Atlas](https://cloud.mongodb.com) URI for `MONGO_URI`

### Frontend — Vercel / Netlify

```bash
cd frontend
npm run build
# Deploy the /build folder
```

Set environment variable:
```
REACT_APP_API_URL=https://your-backend-domain.com/api
```

---

## 🤝 Contributing

Contributions are welcome!

```bash
# 1. Fork the repo
# 2. Create your feature branch
git checkout -b feature/your-feature-name

# 3. Commit your changes
git commit -m 'feat: add your feature'

# 4. Push and open a Pull Request
git push origin feature/your-feature-name
```

### Commit Convention
This project uses [Conventional Commits](https://www.conventionalcommits.org/):
- `feat:` — new feature
- `fix:` — bug fix
- `docs:` — documentation only
- `style:` — formatting, no logic change
- `refactor:` — code restructure

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

Built with ⚡ by [Your Name](https://github.com/yourusername)

**HabitFlow** · Track. Streak. Achieve.

</div>
