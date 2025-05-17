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

## 🔗 API Endpoints

### Authentication

| Method | Endpoint               | Description                    | Access  |
| ------ | ---------------------- | ------------------------------ | ------- |
| POST   | `/api/auth/register`   | Register as a **Job Seeker**   | Public  |
| POST   | `/api/auth/login`      | Login and receive a JWT        | Public  |

### Job Seeker

| Method | Endpoint                              | Description                       | Access        |
| ------ | ------------------------------------- | --------------------------------- | ------------- |
| GET    | `/api/jobs`                           | List all open job postings        | Authenticated |
| POST   | `/api/applications/:job_id/apply`     | Upload CV & initiate application  | Job Seeker    |

### Employee (Recruiter)

| Method | Endpoint                                        | Description                          | Access    |
| ------ | ----------------------------------------------- | ------------------------------------ | --------- |
| POST   | `/api/jobs`                                     | Create a new job posting             | Employee  |
| PUT    | `/api/jobs/:job_id`                             | Update own job posting               | Employee  |
| DELETE | `/api/jobs/:job_id`                             | Delete own job posting               | Employee  |
| GET    | `/api/applications/job/:job_id`                 | View applications for a job          | Employee  |
| PUT    | `/api/applications/:application_id/status`      | Accept or reject an application      | Employee  |

### Admin

| Method | Endpoint                         | Description                       | Access |
| ------ | -------------------------------- | --------------------------------- | ------ |
| GET    | `/api/admin/users`               | List all users                    | Admin  |
| POST   | `/api/admin/users`               | Create Employee/Admin accounts    | Admin  |
| PUT    | `/api/admin/users/:id`           | Update a user’s details           | Admin  |
| DELETE | `/api/admin/users/:id`           | Delete a user                     | Admin  |
| GET    | `/api/admin/jobs`                | List all jobs                     | Admin  |
| GET    | `/api/admin/applications`        | List all applications             | Admin  |
| GET    | `/api/admin/analytics`           | View platform analytics           | Admin  |

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

