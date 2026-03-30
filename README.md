# TypeScript CRUD API with Node.js, Express & MySQL

A fully typed REST API built with TypeScript, Express, Sequelize, and MySQL. Supports full CRUD operations on user records with role-based access, bcrypt password hashing, and Joi validation.

---

## Project Structure

```
typescript-crud-api/
├── config.json                  # Database credentials (do not commit!)
├── tsconfig.json                # TypeScript compiler settings
├── package.json
└── src/
    ├── server.ts                # Entry point
    ├── _helpers/
    │   ├── db.ts                # MySQL + Sequelize setup
    │   └── role.ts              # Role enum (Admin | User)
    ├── _middleware/
    │   ├── errorHandler.ts      # Global error handler
    │   └── validateRequest.ts   # Joi validation wrapper
    └── users/
        ├── user.model.ts        # Sequelize User model (typed)
        ├── user.service.ts      # Business logic (typed methods)
        └── users.controller.ts  # Route handlers (typed)
```

---

## Setup Instructions

### 1. Clone or create the project folder

```cmd
mkdir typescript-crud-api
cd typescript-crud-api
```

### 2. Install dependencies

```cmd
npm install express mysql2 sequelize bcryptjs jsonwebtoken cors joi rootpath
npm install --save-dev typescript ts-node @types/node @types/express @types/cors @types/bcryptjs @types/jsonwebtoken nodemon
npx tsc --init
```

### 3. Configure the database

Create `config.json` in the project root:

```json
{
  "database": {
    "host": "localhost",
    "port": 3306,
    "user": "root",
    "password": "your_mysql_password",
    "database": "typescript_crud_api"
  },
  "jwtSecret": "change-this-in-production-123!"
}
```

> ⚠️ Replace `your_mysql_password` with your actual MySQL root password.  
> ⚠️ In production, use environment variables instead of hardcoding secrets.

### 4. Update `tsconfig.json`

Replace the contents with:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "lib": ["ES2020"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "baseUrl": "./src",
    "paths": {
      "_helpers/*": ["_helpers/*"],
      "_middleware/*": ["_middleware/*"],
      "users/*": ["users/*"]
    }
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

### 5. Update `package.json` scripts

```json
"scripts": {
  "build": "tsc",
  "start": "node dist/server.js",
  "start:dev": "nodemon --exec ts-node src/server.ts",
  "test": "ts-node tests/users.test.ts"
}
```

### 6. Create the folder structure (Windows CMD)

```cmd
mkdir src
mkdir src\_helpers
mkdir src\_middleware
mkdir src\users
mkdir tests
```

### 7. Start the development server

Make sure MySQL is running first, then:

```cmd
npm run start:dev
```

Expected output:
```
✅ Database initialized and models synced
✅ Server running on http://localhost:4000
```

---

## API Endpoints

| Method | Endpoint        | Description              |
|--------|-----------------|--------------------------|
| POST   | /users          | Create a new user        |
| GET    | /users          | Get all users            |
| GET    | /users/:id      | Get a user by ID         |
| PUT    | /users/:id      | Update a user by ID      |
| DELETE | /users/:id      | Delete a user by ID      |

---

## Testing the API (using Postman)

| Method | URL | Description |
|--------|-----|-------------|
| POST | /users | Create a new user |
| GET | /users | Get all users |
| GET | /users/:id | Get user by ID |
| PUT | /users/:id | Update a user |
| DELETE | /users/:id | Delete a user |

## Example: Create a User
POST http://localhost:4000/users
Body:
{
  "title": "Mr",
  "firstName": "Jane",
  "lastName": "Smith",
  "email": "jane@example.com",
  "password": "secret123",
  "confirmPassword": "secret123",
  "role": "User"
}
``` 