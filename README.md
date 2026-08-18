# Student Registration System

A Flask-based web application for managing student registrations with MongoDB as the database. This project includes CRUD (Create, Read, Update, Delete) operations, comprehensive testing, Docker containerization, and CI/CD pipeline integration.

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Requirements](#requirements)
- [Folder Structure](#folder-structure)
- [Installation](#installation)
- [Environment Variables](#environment-variables)
- [Usage](#usage)
- [API Routes](#api-routes)
- [Testing](#testing)
- [Docker Deployment](#docker-deployment)
- [License](#license)

## 🎯 Project Overview

The Student Registration System is a web application that allows users to manage student information. It provides a user-friendly interface to add, view, update, and delete student records stored in a MongoDB database. The project is built with Flask, uses PyMongo for database operations, and is containerized with Docker for easy deployment.

## ✨ Features

- **Student Management**: Add, view, update, and delete student records
- **Web Interface**: Clean HTML templates with responsive design
- **MongoDB Integration**: Persistent data storage with MongoDB
- **Health Check**: API endpoint for system health monitoring
- **Testing Suite**: Comprehensive pytest tests for all CRUD operations
- **Docker Support**: Containerized application for consistent deployment
- **Environment Configuration**: Secure configuration management with `.env` files
- **Code Quality**: Integration with pylint, black, and bandit for code standards

## 📦 Requirements

### Python Packages

- **Flask** (3.x) - Web framework
- **Flask-PyMongo** - MongoDB integration for Flask
- **PyMongo** - Python MongoDB driver
- **python-dotenv** - Environment variable management
- **certifi** - SSL/TLS certificate bundle (cross-platform compatibility)
- **pytest** - Testing framework
- **pylint** - Code analysis
- **black** - Code formatter
- **bandit** - Security issue scanner

### System Requirements

- Python 3.11 or higher
- MongoDB (local or cloud instance via MongoDB Atlas)
- Docker (optional, for containerized deployment)
- Docker Compose (optional)

## 📁 Folder Structure

```
flask_Practice/
├── app.py                      # Main Flask application
├── test_app.py                 # Pytest test suite
├── requirements.txt            # Project dependencies
├── Dockerfile                  # Docker container configuration
├── README.md                   # Project documentation
├── LICENSE                     # License file
├── .env                        # Environment variables (not in repo)
├── .venv/                      # Virtual environment (local only)
│
└── templates/                  # HTML templates directory
    ├── base.html               # Base template (shared layout)
    ├── index.html              # Home page (student list)
    ├── add_student.html        # Form to add new student
    └── update_student.html     # Form to update student info
```

### Key Files Description

- **app.py**: Core Flask application with routes for CRUD operations
- **test_app.py**: Unit tests using pytest framework
- **requirements.txt**: List of Python package dependencies
- **Dockerfile**: Instructions for building Docker image
- **templates/**: HTML templates for web interface

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd flask_Practice
```

### 2. Create Virtual Environment

```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# macOS/Linux
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Setup Environment Variables

Create a `.env` file in the project root directory:

```bash
MONGO_URI=mongodb+srv://<username>:<password>@<cluster>.mongodb.net/student_db
SECRET_KEY=your-secret-key-here
```

**Example for Local MongoDB:**
```
MONGO_URI=mongodb://localhost:27017/student_db
SECRET_KEY=dev-secret-key
```

## 🔐 Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `MONGO_URI` | MongoDB connection string | `mongodb+srv://user:pass@cluster.mongodb.net/student_db` |
| `SECRET_KEY` | Flask secret key for sessions | `your-random-secret-key` |

> **Note**: The `.env` file should never be committed to version control. Add it to `.gitignore`.

## 📖 Usage

### Running Locally

```bash
# Activate virtual environment (if not already activated)
.venv\Scripts\activate  # Windows
# or
source .venv/bin/activate  # macOS/Linux

# Run the Flask application
python app.py
```

The application will start at `http://localhost:5000`

### Web Interface

- **Home Page** (`/`): View all registered students
- **Add Student** (`/add`): Form to add a new student
- **Update Student** (`/update/<student_id>`): Form to update student information
- **Delete Student** (`/delete/<student_id>`): Remove a student record

## 🔌 API Routes

| Method | Route | Description |
|--------|-------|-------------|
| GET | `/` | Display all students |
| GET | `/add` | Show add student form |
| POST | `/add` | Submit new student data |
| GET | `/update/<student_id>` | Show update form for student |
| POST | `/update/<student_id>` | Submit updated student data |
| GET | `/delete/<student_id>` | Delete a student record |
| GET/POST | `/health` | Health check endpoint |

## 🧪 Testing

Run the test suite using pytest:

```bash
# Run all tests
pytest test_app.py

# Run with verbose output
pytest test_app.py -v

# Run specific test
pytest test_app.py::test_home_page -v
```

### Test Coverage

- **test_home_page**: Verify home page loads and displays students
- **test_add_student**: Test adding new student functionality
- **test_update_student**: Test updating existing student data
- **test_delete_student**: Test deleting student records
- **test_health_route**: Verify health check endpoint

> **Note**: Tests use a separate test MongoDB database (`test_student_db`)

## 🐳 Docker Deployment

### Build Docker Image

```bash
docker build -t studentregsystem:latest .
```

### Run Docker Container

```bash
# Basic run
docker run -p 5000:5000 \
  -e MONGO_URI="mongodb+srv://user:pass@cluster.mongodb.net/student_db" \
  -e SECRET_KEY="your-secret-key" \
  studentregsystem:latest

# Run in background
docker run -d -p 5000:5000 \
  -e MONGO_URI="<your-mongo-uri>" \
  -e SECRET_KEY="<your-secret>" \
  --name student-app \
  studentregsystem:latest
```

### Docker Image Details

- **Base Image**: Python 3.11-slim
- **Working Directory**: `/app`
- **Exposed Port**: 5000
- **Entry Point**: `python app.py`

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Create a feature branch
2. Make your changes
3. Run tests to ensure everything works
4. Submit a pull request

## 📞 Support

For issues or questions, please open an issue in the repository or contact the project maintainer.

