# Authentication API

API endpoints for user authentication.

## Overview

ReFlow uses wallet-based authentication with JWT tokens.

## Endpoints

### Login

Authenticate with wallet signature.

```
POST /auth/login
```

#### Request Body

```json
{
  "address": "0x1234567890abcdef...",
  "signature": "0xabcdef...",
  "message": "Sign this message to login: <nonce>"
}
```

#### Response

```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
    "user": {
      "id": "uuid",
      "address": "0x...",
      "kycStatus": "VERIFIED"
    }
  }
}
```

### Register

Register a new user.

```
POST /auth/register
```

#### Request Body

```json
{
  "address": "0x1234567890abcdef...",
  "email": "user@example.com",
  "signature": "0xabcdef..."
}
```

#### Response

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "address": "0x...",
    "email": "user@example.com",
    "kycStatus": "PENDING"
  }
}
```

### Refresh Token

Refresh access token.

```
POST /auth/refresh
```

#### Request Body

```json
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

#### Response

```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
  }
}
```

### Get Profile

Get current user profile.

```
GET /auth/profile
```

**Auth Required:** Yes

#### Response

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "address": "0x...",
    "email": "user@example.com",
    "kycStatus": "VERIFIED",
    "createdAt": "2024-01-01T00:00:00Z"
  }
}
```

## Authentication Flow

```
1. Frontend requests nonce for address
2. User signs message with wallet
3. Frontend sends signature to /auth/login
4. Backend verifies signature
5. Backend returns JWT tokens
6. Frontend stores tokens
7. Frontend includes token in subsequent requests
```

## Token Usage

### Access Token

- Short-lived (15 minutes)
- Include in Authorization header
- Used for API requests

### Refresh Token

- Longer-lived (7 days)
- Used to get new access token
- Should be stored securely

## Errors

| Code | Message | Description |
|------|---------|-------------|
| 401 | Invalid signature | Signature verification failed |
| 401 | Token expired | Access token expired |
| 401 | Invalid token | Token malformed or invalid |
| 404 | User not found | No user with this address |
