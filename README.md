# MedOS - Pharmacy Management System

MedOS is a full-stack pharmacy management platform built for daily pharmacy operations such as inventory control, billing, patient management, supplier tracking, analytics, and admin operations.

The application includes:
- A React + Vite frontend dashboard
- A Node.js + Express backend API
- MongoDB Atlas integration through Mongoose
- PDF invoice generation and CSV exports
- Prescription upload support
- Automated stock alert emails

## Core Features

### Authentication and Access Control
- Username/password login with role-aware access
- Role-based UI gating for admin-only areas
- User account status (Active/Inactive)
- Last login tracking

### Dashboard and Analytics
- KPI cards for medicines, low stock, revenue, and patients
- Revenue trend charts and sales volume charts
- Category distribution charts
- Recent activity feed
- Low-stock and expiry visibility

### Inventory Management
- Add, edit, delete medicines
- Category filtering and search
- Stock status indicators (in stock, low stock, out of stock, expired)
- Manual restock operation with stock transaction logging
- Inventory CSV export

### Billing and Sales (POS)
- Medicine search with quick-add flow
- Cart management with quantity controls
- Patient selection or walk-in billing
- Discount and GST support
- Multiple payment methods (Cash, Card, UPI, Credit)
- Prescription upload (image/PDF)
- Invoice persistence to database
- PDF invoice generation and download

### Patient Management
- Add, edit, delete patient records
- Search by name/phone
- Purchase history per patient
- Invoice PDF download from history
- Prescription file access from patient history
- Patients CSV export

### Supplier Management
- Add, edit, delete suppliers
- Active/inactive supplier states
- Supplier-wise medicine count view

### Reports
- Revenue and category analytics
- Sales history with date-range filtering
- Top-selling medicines report
- Inventory health table
- Sales CSV export

### Admin Operations
- User management (create, edit, delete)
- Protection against deleting the last admin
- Pharmacy and system settings management
- In-app database schema viewer page

### Alerts and Notifications
- Out-of-stock and low-stock detection
- Email alerts via Nodemailer/Gmail SMTP
- Manual restock alert trigger endpoint

## Tech Stack

### Frontend
- React 18
- Vite 6
- Recharts (charts/analytics)
- jsPDF + jspdf-autotable (invoice generation)
- Plain CSS-in-JS style objects (no external UI framework)

### Backend
- Node.js (ES Modules)
- Express 4
- Mongoose 8
- Multer (file uploads)
- Nodemailer (email alerts)
- CORS + dotenv

### Database
- MongoDB Atlas
- Mongoose schemas for users, medicines, sales, patients, suppliers, settings, stock transactions, activity logs, and restock requests
- Automatic seed on first run

## Project Structure

```text
.
|-- src/
|   |-- components/
|   |-- pages/
|   |-- utils/
|   |-- App.jsx
|   |-- config.js
|   `-- theme.js
|-- backend/
|   |-- database/
|   |   |-- index.js
|   |   |-- schema.sql
|   |   `-- seed.sql
|   |-- services/
|   |   `-- emailService.js
|   |-- uploads/prescriptions/
|   `-- server.js
|-- .env.example
|-- package.json
`-- vite.config.js
```

## Environment Variables

### Frontend (.env at project root)

Use `.env.example` as baseline:

```env
VITE_API_URL=http://localhost:3001/api
```

### Backend (backend/.env)

Create `backend/.env` with:

```env
PORT=3001
NODE_ENV=development
MONGODB_URI=your_mongodb_atlas_connection_string
FRONTEND_URL=http://localhost:5173

EMAIL_USER=your_gmail_address
EMAIL_PASS=your_gmail_app_password
ADMIN_EMAIL=admin@yourdomain.com
```

## Local Development Setup

## 1) Install dependencies

From project root:

```bash
npm install
```

From backend folder:

```bash
cd backend
npm install
```

## 2) Configure environment files
- Create root `.env` (or keep defaults)
- Create `backend/.env` with MongoDB and email credentials

## 3) Run backend

```bash
cd backend
npm run dev
```

Backend starts on `http://localhost:3001` by default.

## 4) Run frontend

In a separate terminal:

```bash
npm run dev
```

Frontend starts on `http://localhost:5173` by default.

## 5) Open the app
- Navigate to `http://localhost:5173`
- Log in with one of the seeded users

## Default Seeded Accounts

- Admin: `admin` / `admin123`
- Staff: `staff1` / `staff123`
- Staff: `staff2` / `staff123`

## Available Scripts

### Frontend (root)
- `npm run dev` - Start Vite dev server
- `npm run build` - Build production bundle
- `npm run preview` - Preview production build

### Backend (backend)
- `npm run dev` - Start API server in watch mode
- `npm start` - Start API server

## API Overview

Base URL: `http://localhost:3001/api`

### Auth
- `POST /auth/login`

### Uploads
- `POST /upload-prescription`

### Users
- `GET /users`
- `POST /users`
- `PUT /users/:id`
- `DELETE /users/:id`

### Medicines
- `GET /medicines`
- `GET /medicines/:id`
- `POST /medicines`
- `PUT /medicines/:id`
- `PATCH /medicines/:id/stock`
- `DELETE /medicines/:id`

### Patients
- `GET /patients`
- `GET /patients/:id`
- `GET /patients/:id/history`
- `POST /patients`
- `PUT /patients/:id`
- `DELETE /patients/:id`

### Suppliers
- `GET /suppliers`
- `POST /suppliers`
- `PUT /suppliers/:id`
- `DELETE /suppliers/:id`

### Sales and Billing
- `GET /sales`
- `GET /sales/:id`
- `POST /sales`

### Dashboard
- `GET /dashboard/stats`
- `GET /dashboard/sales-chart`
- `GET /dashboard/category-chart`
- `GET /dashboard/recent-activity`
- `GET /dashboard/low-stock`
- `GET /dashboard/expiring`

### Reports
- `GET /reports/sales`
- `GET /reports/inventory`
- `GET /reports/top-medicines`

### Settings
- `GET /settings`
- `PUT /settings`

### Alerts
- `GET /alerts/out-of-stock`
- `POST /alerts/send-restock/:id`

### Health
- `GET /health`

## Database Notes

- Primary runtime data layer is MongoDB Atlas via Mongoose.
- Backend auto-seeds sample data at startup if no users exist.
- `backend/database/schema.sql` and `backend/database/seed.sql` are SQL schema/seed references and are not used by the active backend runtime.

## Current Security and Production Notes

Current implementation is suitable for demo/internal environments and should be hardened before production:
- Passwords are stored and compared in plain text (add hashing such as bcrypt).
- No token/session-based auth (add JWT or secure session management).
- Email credentials should come only from secure environment variables.
- Add request validation/rate limiting and centralized auth middleware.

## Troubleshooting

### Backend fails to connect to database
- Verify `MONGODB_URI` in `backend/.env`
- Ensure Atlas IP allowlist includes your current IP
- Verify Atlas DB user credentials
- URL-encode special characters in DB password

### Frontend cannot reach backend
- Confirm backend is running on port `3001`
- Verify `VITE_API_URL` points to correct backend API URL
- Check CORS `FRONTEND_URL` in backend env

### Prescription uploads not accessible
- Confirm backend serves `/uploads` static directory
- Verify `backend/uploads/prescriptions` exists and is writable

## Roadmap Suggestions

- Add JWT-based auth and route middleware
- Add password hashing and role checks server-side per endpoint
- Add automated tests (unit + API + UI)
- Add Docker setup for repeatable local/dev/prod deployment
- Add CI for lint/build/test

---

MedOS is designed to provide a practical, modern pharmacy operations dashboard with clear workflows for stock, billing, and analytics.