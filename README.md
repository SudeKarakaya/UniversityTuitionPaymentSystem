# 🎓 University Tuition Payment System  
A RESTful, versionable, JWT-secured tuition payment system built for a fictitious university.  
Students, banking systems, and university admins interact with the system through separate endpoints.

# 📌 Features

### ✔ Multiple Clients Supported
- **University Mobile App** → Tuition query
- **Banking App** → Tuition query + payment
- **Admin Web Portal** → Tuition add, CSV batch insert, unpaid tuition report (with paging)

### ✔ Core Requirements Implemented
- Versioned REST APIs (`/api/v1/...`)
- JWT Authentication for Bank + Admin
- Paging support (Admin unpaid report)
- API Gateway (Reverse Proxy) with:
  - Rate limiting (3 mobile queries per student/day)
  - Request/response logging
- Cloud deployment (API + Gateway hosted)
- PostgreSQL cloud database (Railway)
- Swagger documentation pointing to API Gateway

# 🏛 Architecture Overview
Client Apps → API Gateway (YARP) → University API → PostgreSQL (Railway)

### Gateway Responsibilities
- Central entry point  
- Rate limiting  
- JWT validation pass-through  
- Logging & request tracing  

### API Responsibilities
- Tuition CRUD  
- Payment recording  
- CSV batch upload  
- Paging  
- DB migrations + seed data  

# 📁 Project Structure
/UniversityTuitionPaymentSystem
Controllers/
Data/
Models/
Program.cs
appsettings.json

/UniversityTuitionPaymentGateway
Program.cs
appsettings.json

# 🔐 Authentication

### ✔ JWT Authentication implemented  
Roles:
- **Admin**
- **Bank**
- Mobile app endpoints are **anonymous**.

### Example Token Request
POST /api/v1/authentication/token
{
"username": "bank",
"password": "bankpass"
}

# Assumptions
No actual credit card system required
Payments are simulated
Student list seeded for demo
Admin/Bank credentials are static (lab/demo purpose)

# Issues Encountered
Azure App Service default domain sometimes invalid
Swagger "AddServer" caused deployment crash
Railway SSL mode adjustments
EF Core migrations not discovering PostgreSQL at first
YARP config change required (Routes → new syntax)

# Conclusion
This project fulfills all requirements:
Versioned APIs
JWT authentication
Paging
CSV batch processing
API Gateway with rate limiting + logging
Cloud-hosted REST backend

# 📊 Data Model (ER Diagram)
    
    STUDENTS {
        int StudentId PK
        string StudentNo (unique)
        string FirstName
        string LastName
        string Email
    }

    TUITIONS {
        int TuitionId PK
        int StudentId FK
        string Term
        decimal TotalAmount
        decimal Balance
        bool IsPaid
    }

    PAYMENTS {
        int PaymentId PK
        int TuitionId FK
        decimal AmountPaid
        datetime PaymentTime
        string Source
        string Status
    }
