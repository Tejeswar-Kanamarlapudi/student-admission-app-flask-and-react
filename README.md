# Student Admission System — Full Stack CRUD App

A modern full-stack web application designed for managing student admissions and demonstrating how **React (Vite)**, **Flask (Python)**, and **MySQL** communicate seamlessly in a full-stack architecture.

---

## 📌 What is this Application?

The **Student Admission System** is a full-fledged admission management portal and teaching application. It provides an intuitive interface for university/college administrative staff to manage student admissions through standard **CRUD** operations:

- **Create**: Add new student admission entries with personal and academic details.
- **Read**: Fetch and display the student directory dynamically in real-time.
- **Update**: Edit existing student records directly through the interface.
- **Delete**: Remove student entries with immediate UI and database synchronization.

### 🏗 Architecture Overview

```
React (Vite Frontend)  <--- HTTP / JSON (fetch) --->  Flask (Python REST API)  <--- mysql-connector --->  MySQL Database
   localhost:5173                                        localhost:5000                                    localhost:3306
```

---

## 📂 Project Structure

```
student-admission-app/
├── backend/
│   ├── app.py              # Flask API with all 4 CRUD endpoints
│   ├── db.py               # MySQL database connection configuration
│   └── requirements.txt    # Python backend dependencies
├── frontend/
│   ├── index.html          # Vite HTML entry point
│   ├── package.json        # Frontend dependencies & scripts
│   ├── vite.config.js      # Vite build configuration
│   └── src/
│       ├── App.jsx         # React UI + state + CRUD handlers
│       ├── App.css         # Styling for admission app & student table
│       ├── index.css       # Global baseline CSS
│       └── main.jsx        # React root mount file
└── README.md               # Project documentation
```

---

## 🔄 How a Request Flows (End-to-End)

1. **Page Load**: When the page loads, React's `useEffect` sends a `GET` request to `http://127.0.0.1:5000/students`.
2. **Backend Processing**: Flask receives the request, queries MySQL (`SELECT * FROM students ORDER BY id`) via `db.py`.
3. **Database Response**: MySQL returns tuples/rows, which Flask converts into a JSON list of dictionaries and sends back via `jsonify()`.
4. **UI Render**: React receives the JSON payload, updates state (`useState`), and dynamically displays the student cards or directory table.
5. **Write Operations (Add/Edit/Delete)**:
   - **Add**: Form submit triggers a `POST` request with JSON body → MySQL runs `INSERT`.
   - **Edit**: Clicking "Edit" populates the form; saving triggers a `PUT` request with updated values → MySQL runs `UPDATE`.
   - **Delete**: Clicking "Delete" triggers a `DELETE` request with the student ID → MySQL runs `DELETE`.
   - After each action, the student directory automatically refetches to stay synchronized.

---

## 🛠 Prerequisites

Ensure you have the following installed on your machine:

- [Node.js](https://nodejs.org/) (v18 or higher recommended) & `npm`
- [Python](https://www.python.org/) (v3.8 or higher) & `pip`
- [MySQL Server](https://dev.mysql.com/downloads/mysql/) (running locally or remotely)

---

## 🚀 Step-by-Step Setup & How to Run

### Step 1: Set Up the MySQL Database

1. Open your MySQL client (MySQL Workbench, phpMyAdmin, or MySQL CLI):
   ```bash
   mysql -u root -p
   ```
2. Create the database and the `students` table:
   ```sql
   CREATE DATABASE IF NOT EXISTS student_admission;
   USE student_admission;

   CREATE TABLE IF NOT EXISTS students (
       id INT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(100) NOT NULL,
       age INT NOT NULL,
       gender VARCHAR(20) NOT NULL,
       phone VARCHAR(20) NOT NULL,
       address VARCHAR(300),
       degree VARCHAR(100) NOT NULL,
       program VARCHAR(100) NOT NULL
   );

   -- Insert initial sample records (optional)
   INSERT INTO students (name, age, gender, phone, address, degree, program)
   VALUES 
       ('Sri Ram', 20, 'Male', '9876543210', 'Chennai, Tamil Nadu', 'B.Tech', 'Computer Science'),
       ('Anitha Kumar', 21, 'Female', '9123456780', 'Vijayawada, Andhra Pradesh', 'MCA', 'Data Science');
   ```

---

### Step 2: Configure & Start the Flask Backend

1. Navigate to the `backend` folder:
   ```bash
   cd backend
   ```

2. *(Optional but recommended)* Create and activate a Python virtual environment:
   - **Windows (PowerShell)**:
     ```powershell
     python -m venv venv
     .\venv\Scripts\Activate.ps1
     ```
   - **macOS / Linux**:
     ```bash
     python3 -m venv venv
     source venv/bin/activate
     ```

3. Install required Python packages:
   ```bash
   pip install Flask flask-cors mysql-connector-python
   ```
   *(Or install via `pip install -r requirements.txt`)*

4. Open `backend/db.py` and update your MySQL connection details:
   ```python
   DB_HOST = "localhost"
   DB_USER = "root"
   DB_PASSWORD = "your_mysql_password"   # <-- Replace with your MySQL password
   DB_NAME = "student_admission"
   ```

5. Run the Flask server:
   ```bash
   python app.py
   ```
   Backend will start at: **`http://127.0.0.1:5000`**

---

### Step 3: Start the Vite + React Frontend

1. Open a new terminal window and navigate to the `frontend` folder:
   ```bash
   cd frontend
   ```

2. Install npm dependencies:
   ```bash
   npm install
   ```

3. Start the Vite development server:
   ```bash
   npm run dev
   ```

4. Open your browser and navigate to the URL shown in your terminal (typically **`http://localhost:5173`**).

---

## 📡 API Endpoints Reference

| Method | Endpoint | Description | Request Body |
| :--- | :--- | :--- | :--- |
| **GET** | `/students` | Fetch all student admission records | _None_ |
| **POST** | `/students` | Add a new student admission | `{ name, age, gender, phone, address, degree, program }` |
| **PUT** | `/students/<id>` | Update an existing student record | `{ name, age, gender, phone, address, degree, program }` |
| **DELETE** | `/students/<id>` | Delete a student by ID | _None_ |

---

## 💡 Notes & Best Practices

- **CORS Handling**: `flask-cors` is enabled in `backend/app.py` to allow requests originating from Vite's port (`5173`) to Flask's port (`5000`).
- **Simplicity First**: State management relies on standard React hooks (`useState`, `useEffect`) and native `fetch` without overhead, keeping the codebase clean and educational.
