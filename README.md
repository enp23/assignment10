# Module 10

This module builds on the FastAPI Calculator by adding a PostgreSQL database and pgAdmin for data persistence and management, alongside a secure user model, Pydantic validation, and an end-to-end CI/CD pipeline that tests and deploys the application via GitHub Actions and Docker Hub.

## Key Components

1. **Dockerfile** — Sets up a Docker container for a FastAPI Calculator Application by using the `mcr.microsoft.com/playwright/python:v1.47.0-noble` image as the base, installing necessary system packages and Python dependencies, creating a non-root user with sudo access, and configuring the environment to run the application.
2. **docker-compose.yml** — Defines a multi-service application, including a FastAPI web service, a PostgreSQL database (`postgres:16`), and pgAdmin for database management, specifying their configurations, health checks, dependencies, and networking to facilitate seamless integration and deployment.
3. **main.py** — Defines the FastAPI application, including API endpoints for basic arithmetic operations (add, subtract, multiply, divide) and serving the HTML page, utilizing Pydantic models for input validation and Jinja2 templates for rendering responses.
4. **app/models/user.py** — Defines the `User` model, including password hashing (via `bcrypt`), JWT-based token creation/verification, and user registration/authentication logic.
5. **app/schemas/user.py** — Pydantic schemas that validate user data (e.g., email format, password requirements) before it ever reaches the database.
6. **app/operations.py** — Contains arithmetic functions (`add`, `subtract`, `multiply`, `divide`).
7. **templates/index.html** — The HTML frontend that interacts with the API.

## Requirements

- Python 3.12
- Docker and Docker Compose

## Security & Validation

- **Passwords** are never stored in plain text — they're hashed using `bcrypt` (via `passlib`'s `CryptContext`) before being saved to the database.
- **Authentication** uses JWT access tokens, created and verified with `python-jose`.
- **Input validation** is handled by Pydantic schemas (`app/schemas/user.py`), which enforce field requirements (e.g., valid email format, minimum password length) and reject malformed data before it reaches the database or the `User` model.

## Database

This project uses **PostgreSQL** to persist user and calculation data, with **pgAdmin** as a web-based interface for managing the database directly.

- **Database:** `fastapi_db`
- **Tables:** `users`, `calculations` (linked by a foreign key on `user_id`)
- **pgAdmin access:** `http://localhost:5050`
  - Email: `admin@example.com`
  - Password: `admin`
  - Server host: `db` (Docker service name), Port: `5432`

## Setting Up the Development Environment

1. **Clone the repository:**
git clone https://github.com/enp23/assignment10.git

2. **Navigate to the project directory:**
cd assignment10

3. **Create a virtual environment:**
python -m venv venv
source venv/bin/activate  # On Windows use venv\Scripts\activate

4. **Install dependencies:**
pip install -r requirements.txt

5. **Run the application:**
uvicorn main:app --reload

6. **Access the application:**
   Open a web browser and navigate to `http://localhost:8000`.

7. **Stop the application:**
   Press `CTRL + C`.

### Running with Docker

8. **Build and start the application, database, and pgAdmin in Docker:**
docker compose up --build

## Docker Hub

[https://hub.docker.com/repository/docker/en23/assignment10/](https://hub.docker.com/repository/docker/en23/assigment10/)