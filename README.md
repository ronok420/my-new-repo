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
# Register new user [Public]
POST http://localhost:5000/api/users/register
Content-Type: application/json

{
    "full_name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "role": "job_seeker"  // Can be: "job_seeker", "employee", or "admin"
}

# Login [Public]
POST http://localhost:5000/api/users/login
Content-Type: application/json

{
    "email": "john@example.com",
    "password": "password123"
}
```

### Job Seeker Endpoints
```http
# Get all available jobs [Public]
GET http://localhost:5000/api/jobs

# Get specific job details [Public]
GET http://localhost:5000/api/jobs/:id

# Apply for a job (Step 1: Upload Resume) [Job Seeker]
POST http://localhost:5000/api/applications/:job_id/:user_id/initiate
Authorization: Bearer job_seeker_token
Content-Type: multipart/form-data

{
    "resume": [file]
}

# Process payment (Step 2) [Job Seeker]
POST http://localhost:5000/api/applications/:job_id/:user_id/payment
Authorization: Bearer job_seeker_token
Content-Type: application/json

{
    "payment_method": "card"
}

# Confirm payment (Step 3) [Job Seeker]
POST http://localhost:5000/api/applications/:job_id/:user_id/confirm-payment
Authorization: Bearer job_seeker_token
Content-Type: application/json

{
    "payment_intent_id": "pi_123456789"
}

# View application details [Job Seeker]
GET http://localhost:5000/api/applications/:application_id
Authorization: Bearer job_seeker_token
```

[Rest of the endpoints follow the same format...]

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

