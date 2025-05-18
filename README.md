# HireMe Backend API

A robust backend API for a job application platform that connects job seekers with employers, featuring role-based authentication, payment processing, and file upload capabilities.

## 🚀 Technologies Used

- **Node.js** – Runtime environment  
- **Express.js** – Web framework  
- **PostgreSQL** – Primary database (hosted on Supabase)  
- **Supabase** – Hosted PostgreSQL service  
- **JWT** – Authentication tokens  
- **Multer** – File upload handling  
- **Bcrypt** – Password hashing  
- **Stripe** – Payment processing (mock for testing)  
- **Zod** – Validation schemas  
- **dotenv** – Environment variable management  

## 📁 Project Structure

```
hireme-backend/
├── src/
│   ├── config/
│   ├── controllers/
│   ├── middlewares/
│   ├── routes/
│   ├── schemas/
│   ├── services/
│   └── app.js
├── uploads/
│   └── resumes/
├── .env
├── .gitignore
├── package.json
└── server.js

```

## 🛠️ Installation

1. **Clone** the repository:  
   ```bash
   git clone https://github.com/your-username/hireme-backend.git
   cd hireme-backend
   ```
2. **Install** dependencies:  
   ```bash
   npm install
   ```
3. **Configure** environment variables by creating a `.env` file in the project root:
   ```env
   DATABASE_URL=postgresql://<user>:<pass>@<host>:5432/<db_name>
   SUPABASE_URL=https://<project-ref>.supabase.co
   SUPABASE_SERVICE_ROLE_KEY=<service-role-key>
   JWT_SECRET=<your_jwt_secret>
   STRIPE_SECRET_KEY=<your_stripe_secret_key>
   PORT=4000
   ```

## 🚀 Running the Project

- **Development**:
  ```bash
  npm run dev
  ```
- **Production**:
  ```bash
  npm start
  ```
## 🔐 API Endpoints

### Authentication
```http
# Register new user
POST http://localhost:5000/api/users/register
Content-Type: application/json

{
    "full_name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "role": "job_seeker"  // Can be: "job_seeker", "employee", or "admin"
}

# Login
POST http://localhost:5000/api/users/login
Content-Type: application/json

{
    "email": "john@example.com",
    "password": "password123"
}
```

### Job Seeker Endpoints
```http
# Get all available jobs
GET http://localhost:5000/api/jobs

# Get specific job details
GET http://localhost:5000/api/jobs/:id

# Apply for a job (Step 1: Upload Resume)
POST http://localhost:5000/api/applications/:job_id/:user_id/initiate
Authorization: Bearer job_seeker_token
Content-Type: multipart/form-data

{
    "resume": [file]
}

# Process payment (Step 2)
POST http://localhost:5000/api/applications/:job_id/:user_id/payment
Authorization: Bearer job_seeker_token
Content-Type: application/json

{
    "payment_method": "card"
}

# Confirm payment (Step 3)
POST http://localhost:5000/api/applications/:job_id/:user_id/confirm-payment
Authorization: Bearer job_seeker_token
Content-Type: application/json

{
    "payment_intent_id": "pi_123456789"
}

# View application details
GET http://localhost:5000/api/applications/:application_id
Authorization: Bearer job_seeker_token
```

### Employee Endpoints
```http
# Create new job
POST http://localhost:5000/api/jobs
Authorization: Bearer employee_token
Content-Type: application/json

{
    "job_title": "Frontend Developer",
    "job_description": "Job description here",
    "company_name": "Tech Corp"
}

# Update job
PUT http://localhost:5000/api/jobs/:id
Authorization: Bearer employee_token
Content-Type: application/json

{
    "job_title": "Updated Title",
    "job_description": "Updated description",
    "company_name": "Updated Corp",
    "job_status": "closed"
}

# Delete job
DELETE http://localhost:5000/api/jobs/:id
Authorization: Bearer employee_token

# Get jobs posted by employee
GET http://localhost:5000/api/jobs/employee/jobs
Authorization: Bearer employee_token

# Get applications for a job
GET http://localhost:5000/api/applications/job/:job_id
Authorization: Bearer employee_token

# Update application status
PUT http://localhost:5000/api/applications/:application_id/status
Authorization: Bearer employee_token
Content-Type: application/json

{
    "status": "approved"  // Can be: "pending", "accepted", "rejected"
}

# Get all applications for posted jobs
GET http://localhost:5000/api/applications/user/applications
Authorization: Bearer employee_token
```

### Admin Endpoints

#### User Management
```http
# Get all users
GET http://localhost:5000/api/users/all
Authorization: Bearer admin_token

# Create new user
POST http://localhost:5000/api/users/create
Authorization: Bearer admin_token
Content-Type: application/json

{
    "full_name": "New User",
    "email": "e4@gmail.com",
    "password": "password123",
    "role": "employee"  // Can be: "job_seeker", "employee", or "admin"
}

# Update user
PUT http://localhost:5000/api/admin/users/:id
Authorization: Bearer admin_token
Content-Type: application/json

{
    "full_name": "Updated Name",
    "email": "updated@example.com",
    "role": "employee"  // Can be: "job_seeker", "employee", or "admin"
}

# Delete user
DELETE http://localhost:5000/api/admin/users/:id
Authorization: Bearer admin_token
```

#### Applications Management
```http
# Get all applications
GET http://localhost:5000/api/admin/applications
Authorization: Bearer admin_token

# Filter applications by status
GET http://localhost:5000/api/admin/applications?status=accepted
Authorization: Bearer admin_token

# Filter applications by company and status
GET http://localhost:5000/api/admin/applications?company=Scale up adds&status=accepted
Authorization: Bearer admin_token
```

#### Analytics
```http
# Get company analytics
GET http://localhost:5000/api/admin/analytics?company=Scale up adds
Authorization: Bearer admin_token
```

### Shared Endpoints (Accessible by Admin and Employee)
```http
# Get all jobs
GET http://localhost:5000/api/jobs

# Get job by ID
GET http://localhost:5000/api/jobs/:id

# Create new job
POST http://localhost:5000/api/jobs
Authorization: Bearer token
Content-Type: application/json

{
    "job_title": "Frontend Developer",
    "job_description": "Job description here",
    "company_name": "Tech Corp"
}

# Update job
PUT http://localhost:5000/api/jobs/:id
Authorization: Bearer token
Content-Type: application/json

{
    "job_title": "Updated Title",
    "job_description": "Updated description",
    "company_name": "Updated Corp",
    "job_status": "closed"
}

# Delete job
DELETE http://localhost:5000/api/jobs/:id
Authorization: Bearer token
```

Note: 
- Replace `:id`, `:job_id`, `:user_id`, and `:application_id` with actual IDs
- All endpoints except register and login require Authorization header
- Application fee is 100 Taka
- Resume must be uploaded as a file
- Status values: "pending", "accepted", "rejected"
- Job status values: "open", "closed"

All use ENUM types for roles and statuses to ensure data integrity.

## 💰 Payment System

- Application fee: **100 Taka**  
- Stripe mock integration for testing  
- Invoice tracking stored in the `invoices` table  

## 📄 File Upload

- Supported formats: **PDF**, **DOCX**  
- Max size: **5MB**  
- Stored under `uploads/resumes/`

## 🔒 Security Features

- **JWT-based** authentication  
- **Role-based** access control  
- Password hashing with **Bcrypt**  
- Request validation using **Zod**  
- **CORS** enabled  

