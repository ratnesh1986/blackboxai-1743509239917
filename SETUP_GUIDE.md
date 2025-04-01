# Step-by-Step Setup Guide

## 1. Install Prerequisites
```bash
# Install Node.js (v18+ recommended)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install PostgreSQL
sudo apt-get install postgresql postgresql-contrib
```

## 2. Database Setup
```bash
# Create database user
sudo -u postgres createuser --interactive
# Enter name: insurance_admin
# Make superuser? (y/n): y

# Create database
sudo -u postgres createdb insurance_portal

# Set password
sudo -u postgres psql -c "ALTER USER insurance_admin WITH PASSWORD 'securepassword';"
```

## 3. Project Setup
```bash
# Install nodemon globally
npm install -g nodemon

# Install project dependencies
npm install

# Initialize database schema
psql -U insurance_admin -d insurance_portal -f db/schema.sql
```

## 4. Configuration
```bash
# Edit .env file
nano .env
```
Set these values:
```
DATABASE_URL=postgres://insurance_admin:securepassword@localhost:5432/insurance_portal
JWT_SECRET=your_strong_secret_here
PORT=3000
```

## 5. Running the Application
```bash
# Development mode (with auto-restart)
npm run dev

# Production mode
npm start
```

## 6. Testing the API
```bash
# Test insurance admin login
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@insurance.com","password":"securepassword123","role":"insurance"}'

# Test client admin login
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@client.com","password":"clientpass123","role":"client"}'
```

## 7. Creating Initial Data
```sql
-- Run in psql:
INSERT INTO insurance_companies (name, contact_email) 
VALUES ('Xtra Trust Insurance', 'admin@insurance.com');

INSERT INTO admins (email, password_hash, role_id, company_id, is_active)
VALUES ('admin@insurance.com', '$2b$10$...', 1, 1, true);
-- (Use bcrypt to generate password hash)