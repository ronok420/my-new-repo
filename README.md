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
POST /api/users/register
Body: {
    "full_name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "role": "job_seeker"
}

POST /api/users/login
Body: {
    "email": "john@example.com",
    "password": "password123"
}
```

### Admin Endpoints
```http
to view all applicants details
GET /api/admin/applications
GET /api/admin/jobs
GET /api/admin/analytics
PUT /api/admin/users/:id
Body: {
    "full_name": "Updated Name",
    "email": "updated@example.com",
    "role": "employee"
}
DELETE /api/admin/users/:id
POST http://localhost:5000/api/users/create
Authorization: Bearer admin_token
Content-Type: application/json

{
    "full_name": "New User",
    "email": "e4@gmail.com",
    "password": "password123",
    "role": "employee"  // Can be: "job_seeker", "employee", or "admin"
}
PUT http://localhost:5000/api/admin/users/1
Authorization: Bearer admin_token
Content-Type: application/json

{
    "full_name": "Updated Name",
    "email": "updated@example.com",
    "role": "employee"  // Can be: "job_seeker", "employee", or "admin"
}
Note: Replace 1 in the URL with the actual user_id,
DELETE http://localhost:5000/api/admin/users/1
Authorization: Bearer admin_token,
admin filter company by analytics
http://localhost:5000/api/admin/applications?status=accepted
admin filter applicant  by status
 http://localhost:5000/api/admin/applications?status=accepted   // or pending or reject
admin filter company  by name
http://localhost:5000/api/admin/analytics?company=Scale up adds

```

### Job Endpoints
```http
GET /api/jobs
GET /api/jobs/:id
POST /api/jobs
Body: {
    "job_title": "Frontend Developer",
    "job_description": "Job description here",
    "company_name": "Tech Corp"
}
PUT /api/jobs/:id
Body: {
    "job_title": "Updated Title",
    "job_description": "Updated description",
    "company_name": "Updated Corp",
    "job_status": "closed"
}
DELETE /api/jobs/:id
GET /api/jobs/employee/jobs
```

### Application Endpoints
```http
POST /api/applications/:job_id/:user_id/initiate
Body: {
    "resume": [file]
}

POST /api/applications/:job_id/:user_id/payment
Body: {
    "payment_method": "card"
}

POST /api/applications/:job_id/:user_id/confirm-payment
Body: {
    "payment_intent_id": "pi_123456789"
}

GET /api/applications/job/:job_id
PUT /api/applications/:application_id/status
Body: {
    "status": "approved"
}
GET /api/applications/user/applications
GET /api/applications/:application_id
```

### User Management
```http
POST /api/users/create
Body: {
    "full_name": "New User",
    "email": "new@example.com",
    "password": "password123",
    "role": "job_seeker"
}
GET /api/users/all
```

Note: All endpoints except login and register require Authorization header:

## 💾 Database Schema

Main tables (as per ERD):

- `users`
- `jobs`
- `applications`
- `invoices`

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

