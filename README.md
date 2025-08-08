# Simple Node.js Login App

A very simple Node.js application demonstrating basic authentication with MongoDB.

## Features

- User registration and login
- JWT token-based authentication
- MongoDB integration
- Simple and clean API endpoints

## Prerequisites

- Node.js (v14 or higher)
- MongoDB running on localhost:27017

## Installation

1. Install dependencies:

```bash
npm install
```

2. Make sure MongoDB is running on your machine:

```bash
# If you have MongoDB installed locally
mongod
```

3. Start the server:

```bash
npm start
```

The server will start on `http://localhost:3000`

## API Endpoints

### Authentication

#### 1. Register a new user

```
POST /api/register
Content-Type: application/json

{
  "username": "your_username",
  "password": "your_password"
}
```

#### 2. Login

```
POST /api/login
Content-Type: application/json

{
  "username": "your_username",
  "password": "your_password",
  "twoFactorToken": "123456" // Optional, required if 2FA is enabled
}
```

#### 3. Logout

```
POST /api/logout
Authorization: Bearer YOUR_JWT_TOKEN
```

### Two-Factor Authentication

#### 4. Setup 2FA (Get QR Code)

```
POST /api/2fa/setup
Authorization: Bearer YOUR_JWT_TOKEN
```

#### 5. Verify and Enable 2FA

```
POST /api/2fa/verify
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json

{
  "token": "123456"
}
```

#### 6. Disable 2FA

```
POST /api/2fa/disable
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json

{
  "password": "your_password"
}
```

### User Profile

#### 7. Get Profile

```
GET /api/profile
Authorization: Bearer YOUR_JWT_TOKEN
```

#### 8. Health Check

```
GET /api/health
```

## Database

The app connects to MongoDB at: `mongodb://localhost:27017/loginform`

### User Schema

```javascript
{
  username: String (required, unique),
  password: String (required, hashed),
  twoFactorSecret: String (optional),
  twoFactorEnabled: Boolean (default: false),
  createdAt: Date (default: now)
}
```

## How to Test

1. **Register a user:**

```bash
curl -X POST http://localhost:3000/api/register \
  -H "Content-Type: application/json" \
  -d '{"username": "testuser", "password": "testpass123"}'
```

2. **Login:**

```bash
curl -X POST http://localhost:3000/api/login \
  -H "Content-Type: application/json" \
  -d '{"username": "testuser", "password": "testpass123"}'
```

3. **Setup 2FA:**

```bash
curl -X POST http://localhost:3000/api/2fa/setup \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

4. **Enable 2FA (after scanning QR code):**

```bash
curl -X POST http://localhost:3000/api/2fa/verify \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"token": "123456"}'
```

## Security Features

- Passwords are hashed using bcryptjs
- JWT tokens for secure authentication
- Time-based One-Time Password (TOTP) for 2FA
- Input validation and error handling
- Protected routes with middleware

## Environment Variables

Create a `.env` file with:

```
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
PORT=3000
```

## Dependencies

- **express**: Web framework
- **mongoose**: MongoDB ODM
- **bcryptjs**: Password hashing
- **jsonwebtoken**: JWT token handling
- **speakeasy**: 2FA TOTP generation
- **qrcode**: QR code generation for 2FA setup
- **cors**: Cross-origin resource sharing
- **dotenv**: Environment variable management

## Notes

- This is a learning project designed to be simple and understandable
- In production, use stronger JWT secrets and implement additional security measures
- The app stores data in MongoDB as requested
- All successful operations return success messages with ✅ emoji for clarity
