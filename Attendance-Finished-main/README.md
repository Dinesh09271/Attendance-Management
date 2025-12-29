Attendance Management System

A modern, full-stack attendance management system built with **Spring Boot**, **React**, **TypeScript**, and **MySQL**.

Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Development Setup](#development-setup)
- [Docker Deployment](#docker-deployment)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [Environment Variables](#environment-variables)
- [Troubleshooting](#troubleshooting)
- [License](#license)


Features

JWT-based Authentication with role-based access control (Admin, Staff, Student)
Student Management - Add, edit, delete, and view student records
Attendance Tracking - Mark and monitor attendance by session
Reports & Analytics - Generate attendance reports with filters
Timetable Management - Create and manage class schedules
System Settings - Configure attendance thresholds and academic year
Token Refresh - Automatic token refresh for seamless sessions
Modern UI - Responsive React interface with TypeScript
Docker Support - Containerized deployment ready

Tech Stack

Backend
- **Java 17**
- **Spring Boot 4.0.1**
- **Spring Security** with JWT
- **Spring Data JPA / Hibernate**
- **MySQL 8.0**
- **Maven**

Frontend
- **React 19**
- **TypeScript 5.8**
- **Vite 6.2**
- **Axios** for API calls
- **React Router** for navigation
- **Recharts** for data visualization
- **Lucide React** for icons

---

Prerequisites

Before you begin, ensure you have the following installed:

- **Java 17+** ([Download](https://adoptium.net/))
- **Node.js 20+** and **npm** ([Download](https://nodejs.org/))
- **MySQL 8.0+** ([Download](https://dev.mysql.com/downloads/))
- **Maven 3.9+** (or use included wrapper)
- **Docker & Docker Compose** (optional, for containerized deployment)

---

Quick Start

1. Clone the Repository

```bash
git clone https://github.com/yourusername/attendance-management-system.git
cd attendance-management-system
```

2. Setup Database

```bash
# Login to MySQL
mysql -u root -p

# Create database
CREATE DATABASE attendance_db;

# Import schema
mysql -u root -p attendance_db < database/schema.sql
```

3. Configure Backend

Edit `attendance-backend/src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/attendance_db
spring.datasource.username=root
spring.datasource.password=YOUR_PASSWORD
jwt.secret=YOUR_SECRET_KEY_256_BITS_MINIMUM
```

4. Run Backend

```bash
cd attendance-backend
./mvnw spring-boot:run

# Or on Windows
mvnw.cmd spring-boot:run
```

Backend will start on **http://localhost:8080**

5. Configure Frontend

Create `Frontend/attendx---advanced-student-attendance-system/.env`:

```env
VITE_API_BASE_URL=http://localhost:8080/api
```

6. Install Frontend Dependencies

```bash
cd Frontend/attendx---advanced-student-attendance-system
npm install
```

7. Run Frontend

```bash
npm run dev
```

Frontend will start on **http://localhost:3000**

8. Login

Open http://localhost:3000 and login with default credentials:

| Role | Username | Password |
|------|----------|----------|
| Admin | admin | admin123 |
| Staff | staff | staff123 |
| Student | student | student123 |

---

## Development Setup

### Backend Development

```bash
cd attendance-backend

# Build
./mvnw clean package

# Run tests
./mvnw test

# Run with specific profile
./mvnw spring-boot:run -Dspring-boot.run.profiles=dev
```

### Frontend Development

```bash
cd Frontend/attendx---advanced-student-attendance-system

# Development server with hot reload
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Type checking
npx tsc --noEmit
```

---

## Docker Deployment

### Using Docker Compose (Recommended)

```bash
# Build and start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Remove volumes (caution: deletes database data)
docker-compose down -v
```

Services will be available at:
- Frontend: http://localhost:3000
- Backend: http://localhost:8080
- MySQL: localhost:3306

### Manual Docker Build

**Backend:**
```bash
cd attendance-backend
docker build -t attendance-backend .
docker run -p 8080:8080 attendance-backend
```

**Frontend:**
```bash
cd Frontend/attendx---advanced-student-attendance-system
docker build -t attendance-frontend .
docker run -p 3000:3000 attendance-frontend
```

---

## API Documentation

### Authentication Endpoints

#### POST /api/auth/login
Login and get JWT token

**Request:**
```json
{
  "username": "admin",
  "password": "admin123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "username": "admin",
    "role": "ROLE_ADMIN",
    "userId": 1,
    "name": "admin",
    "email": "admin@attendx.edu"
  }
}
```

#### POST /api/auth/refresh
Refresh access token

**Request:**
```
POST /api/auth/refresh?refreshToken=YOUR_REFRESH_TOKEN
```

### Student Endpoints

#### GET /api/students
Get all students

**Headers:**
```
Authorization: Bearer YOUR_JWT_TOKEN
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "rollNo": "CS2024001",
      "name": "John Doe",
      "department": "Computer Science",
      "semester": 3,
      "email": "john.doe@attendx.edu"
    }
  ]
}
```

#### POST /api/students
Create new student

**Request:**
```json
{
  "rollNo": "CS2024004",
  "name": "Alice Brown",
  "department": "Computer Science",
  "semester": 2,
  "email": "alice@attendx.edu",
  "phone": "1234567890",
  "section": "A"
}
```

#### PUT /api/students/{id}
Update student

#### DELETE /api/students/{id}
Delete student

### Attendance Endpoints

#### POST /api/attendance/session
Mark attendance for a session

**Request:**
```json
{
  "sessionId": 1,
  "studentId": 1,
  "date": "2024-12-24",
  "status": "PRESENT"
}
```

---

##  Database Schema

### Core Tables

1. **users** - Authentication credentials
2. **student** - Student information
3. **staff** - Faculty information
4. **subject** - Course/subject details
5. **timetable_session** - Class schedules
6. **session_attendance** - Attendance records
7. **attendance_status** - Status types (Present, Absent, Late)
8. **system_settings** - Application configuration
9. **refresh_tokens** - JWT refresh tokens

### Relationships

```
users (1) -----> (1) staff
users (1) -----> (0..1) student
staff (M) <-----> (M) subject
staff (1) -----> (M) timetable_session
subject (1) -----> (M) timetable_session
student (1) -----> (M) session_attendance
timetable_session (1) -----> (M) session_attendance
```

### Views

- **v_student_attendance_summary** - Attendance statistics per student
- **v_daily_attendance** - Daily attendance overview

### Stored Procedures

- **sp_calculate_student_attendance** - Calculate attendance percentage
- **sp_get_low_attendance_students** - Get students below threshold

---

## Environment Variables

### Backend (.env or application.properties)

```properties
# Database
SPRING_DATASOURCE_URL=jdbc:mysql://localhost:3306/attendance_db
SPRING_DATASOURCE_USERNAME=root
SPRING_DATASOURCE_PASSWORD=your_password

# JWT
JWT_SECRET=your_256_bit_secret_key
JWT_EXPIRATION=3600000
JWT_REFRESH_EXPIRATION=86400000

# Server
SERVER_PORT=8080

# File Upload
FILE_UPLOAD_DIR=./uploads
SPRING_SERVLET_MULTIPART_MAX_FILE_SIZE=5MB
```

### Frontend (.env)

```env
VITE_API_BASE_URL=http://localhost:8080/api
```

---

## Troubleshooting

### Backend Issues

**Problem:** `Cannot connect to database`
```bash
# Check MySQL is running
sudo systemctl status mysql

# Check connection details in application.properties
# Verify database exists
mysql -u root -p -e "SHOW DATABASES;"
```

**Problem:** `Port 8080 already in use`
```bash
# Find process using port 8080
netstat -ano | findstr :8080

# Change port in application.properties
server.port=8081
```

**Problem:** `JWT token validation failed`
```bash
# Ensure JWT secret is at least 256 bits (32 characters)
# Check jwt.secret in application.properties
```

### Frontend Issues

**Problem:** `CORS errors`
```bash
# Verify WebConfig.java includes your frontend URL
# Check backend console for CORS configuration logs
```

**Problem:** `API calls failing`
```bash
# Check .env file exists with correct API_BASE_URL
# Verify backend is running on correct port
# Check browser console for detailed error messages
```

**Problem:** `npm install fails`
```bash
# Clear npm cache
npm cache clean --force

# Delete node_modules and package-lock.json
rm -rf node_modules package-lock.json

# Reinstall
npm install
```

### Docker Issues

**Problem:** `Container exits immediately`
```bash
# Check logs
docker-compose logs backend
docker-compose logs frontend

# Rebuild without cache
docker-compose build --no-cache
```

**Problem:** `Database connection refused`
```bash
# Wait for MySQL to be fully ready
# Check healthcheck status
docker-compose ps
```

---

## Project Structure

```
attendance-management-system/
├── attendance-backend/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/attendance/
│   │   │   │   ├── config/          # Configuration classes
│   │   │   │   ├── controller/      # REST controllers
│   │   │   │   ├── dto/             # Data Transfer Objects
│   │   │   │   ├── exception/       # Custom exceptions
│   │   │   │   ├── model/           # JPA entities
│   │   │   │   ├── repository/      # Data repositories
│   │   │   │   ├── security/        # Security & JWT
│   │   │   │   ├── service/         # Business logic
│   │   │   │   └── util/            # Utilities
│   │   │   └── resources/
│   │   │       └── application.properties
│   │   └── test/
│   ├── Dockerfile
│   ├── pom.xml
│   └── mvnw
├── Frontend/
│   └── attendx---advanced-student-attendance-system/
│       ├── components/              # React components
│       ├── pages/                   # Page components
│       ├── services/                # API services
│       ├── Dockerfile
│       ├── nginx.conf
│       ├── package.json
│       ├── tsconfig.json
│       └── vite.config.ts
├── database/
│   └── schema.sql                   # Database schema
├── docker-compose.yml
└── README.md
```

---

## Security Considerations

1. **Change default passwords** in production
2. **Use strong JWT secrets** (minimum 256 bits)
3. **Enable HTTPS** for production deployment
4. **Implement rate limiting** for API endpoints
5. **Regular security updates** for dependencies
6. **Database backups** scheduled regularly
7. **Input validation** on all endpoints
8. **SQL injection protection** via JPA/Hibernate

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License.

---

## Default Users

| Username | Password | Role | Description |
|----------|----------|------|-------------|
| admin | admin123 | ROLE_ADMIN | Full system access |
| staff | staff123 | ROLE_STAFF | Mark attendance, view reports |
| student | student123 | ROLE_STUDENT | View own attendance |

** IMPORTANT: Change these passwords before deploying to production!**

---

##  Support

For issues, questions, or contributions, please open an issue on GitHub.

---

**Built with using Spring Boot & React**
