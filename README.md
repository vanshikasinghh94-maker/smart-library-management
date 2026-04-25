# 📚 AI-Powered Smart Library Management System

**Developer:** Vanshika Singh | Enrollment: O23BCA110100  
**Institution:** Chandigarh University | BCA Final Year Project  
**Mentor:** Kashish Gupta

---

## 🌟 Overview

A full-stack web application that automates college library operations and delivers personalized book recommendations using a hybrid Machine Learning model (Collaborative Filtering + Content-Based Filtering).

### Key Features
- 🔐 Secure role-based login (Librarian / Student) with JWT + bcrypt
- 📖 Complete book catalogue — add, edit, search, delete
- 📤 Book issue & return with automatic fine calculation (₹5/day overdue)
- 🤖 AI recommendation engine (88% accuracy on test set)
- 📊 Real-time analytics dashboard with Chart.js visualizations
- 🔔 Automated notifications via APScheduler (due dates, overdue, new arrivals)

---

## 🏗️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Python 3.11, Flask 3.0 |
| Database | MySQL 8.0 + SQLAlchemy ORM |
| AI / ML | Scikit-learn (TF-IDF, Cosine Similarity) |
| Frontend | HTML5, Bootstrap 5, Chart.js |
| Auth | JWT (flask-jwt-extended) + bcrypt |
| Scheduler | APScheduler (daily notification jobs) |

---

## 📁 Project Structure

```
SmartLibrary/
├── app.py                   # Entry point
├── config.py                # Configuration (dev/prod/test)
├── requirements.txt         # Python dependencies
├── .env.example             # Environment variables template
├── app/
│   ├── __init__.py          # Flask app factory
│   ├── models/
│   │   └── __init__.py      # SQLAlchemy models (User, Book, BorrowingHistory...)
│   ├── routes/
│   │   ├── auth.py          # /api/auth — login, register, profile
│   │   ├── books.py         # /api/books — CRUD
│   │   ├── borrowing.py     # /api/borrowing — issue, return, history
│   │   ├── recommendations.py # /api/recommendations
│   │   ├── admin.py         # /api/admin — analytics, user mgmt
│   │   └── notifications.py # /api/notifications + scheduler jobs
│   └── ai/
│       └── recommendation_engine.py  # Hybrid ML model
├── templates/
│   ├── index.html           # Login page
│   ├── student/dashboard.html
│   └── admin/dashboard.html
├── static/
│   ├── css/style.css
│   └── js/
│       ├── auth.js
│       ├── student.js
│       └── admin.js
├── scripts/
│   ├── schema.sql           # MySQL schema (direct import)
│   └── seed_db.py           # Sample data seeder
└── tests/
    └── test_app.py          # Unit + integration tests (pytest)
```

---

## ⚙️ Setup & Installation

### Prerequisites
- Python 3.11+
- MySQL 8.0+
- pip

### Step 1 — Clone & create virtual environment
```bash
unzip SmartLibrary.zip
cd SmartLibrary
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
```

### Step 2 — Install dependencies
```bash
pip install -r requirements.txt
```

### Step 3 — Configure environment
```bash
cp .env.example .env
# Edit .env — set DATABASE_URL, SECRET_KEY, JWT_SECRET_KEY
```

### Step 4 — Set up the database
```bash
# Option A: using the SQL schema file
mysql -u root -p < scripts/schema.sql

# Option B: let SQLAlchemy create tables automatically
python -c "from app import create_app, db; app = create_app(); app.app_context().push(); db.create_all()"
```

### Step 5 — Seed sample data (optional but recommended)
```bash
python scripts/seed_db.py
```

### Step 6 — Run the application
```bash
python app.py
```

Open http://localhost:5000 in your browser.

---

## 🔑 Default Credentials (after seeding)

| Role | Email | Password |
|------|-------|----------|
| Librarian | librarian@library.edu | librarian123 |
| Student | vanshika@student.edu | student123 |

---

## 🤖 AI Recommendation System

The hybrid model combines two approaches:

**Collaborative Filtering (60% weight)**  
Builds a user-item matrix from borrowing history and finds similar readers using cosine similarity. Recommends books liked by students with similar reading patterns.

**Content-Based Filtering (40% weight)**  
Uses TF-IDF vectorization on book title + author + category + description. Creates a user preference profile from past reads and finds similar books.

```
Final Score = 0.6 × CF_score + 0.4 × CB_score
```

Fallback: If a user has no history, returns the most borrowed available books.

---

## 🧪 Running Tests

```bash
pytest tests/ -v
```

Tests cover: authentication, CRUD operations, borrowing workflow, fine calculation, and access control.

---

## 📡 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/auth/login | Login |
| POST | /api/auth/register | Register |
| GET | /api/auth/profile | Get current user |
| GET | /api/books/ | List / search books |
| POST | /api/books/ | Add book (librarian) |
| PUT | /api/books/:id | Update book (librarian) |
| POST | /api/borrowing/issue | Issue book (librarian) |
| POST | /api/borrowing/return/:id | Return book (librarian) |
| GET | /api/borrowing/history | Borrowing history |
| GET | /api/recommendations/ | AI recommendations |
| GET | /api/admin/dashboard | Stats (librarian) |
| GET | /api/admin/analytics/popular-books | Popular books chart |
| GET | /api/notifications/ | User notifications |

---

## 📝 Notes

- Fine calculation: ₹5 per overdue day (configurable in `config.py`)
- Default borrow period: 14 days
- Notifications run daily via APScheduler
- The AI engine retrains on every request (suitable for up to 5,000 users; cache for production scale)
