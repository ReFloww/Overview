# API Reference

Complete reference for the ReFlow REST API.

## Base URL

```
Development: http://localhost:30000/api/v1
Production: https://api.reflow.io/v1
```

## Authentication

Most endpoints require JWT authentication.

### Header Format

```
Authorization: Bearer <access_token>
```

### Get Token

```bash
POST /auth/login
```

## Response Format

### Success

```json
{
  "success": true,
  "data": { ... }
}
```

### Error

```json
{
  "success": false,
  "error": {
    "code": 400,
    "message": "Error description"
  }
}
```

## Endpoints Overview

| Module | Endpoint | Description |
|--------|----------|-------------|
| Auth | `/auth/*` | Authentication |
| Market | `/market/*` | Loan marketplace |
| Portfolio | `/portfolio/*` | User holdings |
| Dashboard | `/dashboard/*` | Analytics |
| Funds | `/investment-funds/*` | Fund managers |
| History | `/history/*` | Transactions |

## Quick Reference

### Authentication

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/auth/login` | No | Login with wallet |
| POST | `/auth/register` | No | Register new user |
| POST | `/auth/refresh` | No | Refresh token |
| GET | `/auth/profile` | Yes | Get user profile |

### Market

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/market/list` | No | List all loans |
| GET | `/market/:id` | No | Get loan details |

### Portfolio

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/portfolio` | Yes | Get user holdings |
| GET | `/portfolio/summary` | Yes | Portfolio summary |

### Dashboard

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/dashboard` | Yes | User dashboard |
| GET | `/dashboard/stats` | Yes | Platform stats |

### Investment Funds

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/investment-funds/list` | No | List fund managers |
| GET | `/investment-funds/:id` | No | Fund details |

### History

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/history/transactions` | Yes | Transaction history |

## Interactive Docs

Swagger UI available at:
```
http://localhost:30000/api/v1/docs
```

## Detailed Documentation

- [Authentication](authentication.md)
- [Market Endpoints](market.md)
- [Portfolio Endpoints](portfolio.md)
- [Dashboard Endpoints](dashboard.md)
- [Investment Funds](investment-funds.md)
- [Transaction History](transactions.md)
