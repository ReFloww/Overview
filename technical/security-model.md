# Security Model

Comprehensive documentation of ReFlow's security architecture, measures, and best practices.

## Security Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                       SECURITY LAYERS                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────────┐│
│  │ APPLICATION SECURITY                                             ││
│  │ • Input validation • Output encoding • Session management       ││
│  └─────────────────────────────────────────────────────────────────┘│
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────────┐│
│  │ AUTHENTICATION & AUTHORIZATION                                   ││
│  │ • Wallet signatures • JWT tokens • Role-based access            ││
│  └─────────────────────────────────────────────────────────────────┘│
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────────┐│
│  │ SMART CONTRACT SECURITY                                          ││
│  │ • Audits • Access control • Reentrancy protection               ││
│  └─────────────────────────────────────────────────────────────────┘│
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────────┐│
│  │ INFRASTRUCTURE SECURITY                                          ││
│  │ • Encryption • Network security • Monitoring                    ││
│  └─────────────────────────────────────────────────────────────────┘│
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## Authentication Security

### Wallet-Based Authentication

```
Authentication Flow:
┌─────────┐                    ┌─────────┐
│  User   │                    │ Backend │
└────┬────┘                    └────┬────┘
     │                              │
     │ 1. Request nonce             │
     │─────────────────────────────▶│
     │                              │
     │◀─────────────────────────────│
     │ 2. Return unique nonce       │
     │                              │
     │ 3. Sign nonce with wallet    │
     │    (client-side)             │
     │                              │
     │ 4. Submit signature          │
     │─────────────────────────────▶│
     │                              │
     │    5. Verify signature       │
     │    6. Issue JWT              │
     │◀─────────────────────────────│
     │ 7. Return tokens             │
     │                              │
```

### JWT Token Security

| Measure | Implementation |
|---------|---------------|
| **Short Expiry** | Access tokens expire in 15 minutes |
| **Refresh Tokens** | Separate, longer-lived tokens |
| **Secure Storage** | HttpOnly cookies where possible |
| **Token Rotation** | Refresh tokens rotated on use |

### Session Management

```typescript
// Token configuration
const tokenConfig = {
  access: {
    expiresIn: '15m',
    algorithm: 'HS256'
  },
  refresh: {
    expiresIn: '7d',
    algorithm: 'HS256'
  }
};
```

## Authorization Model

### Role-Based Access Control

| Role | Permissions |
|------|-------------|
| **Guest** | View public data |
| **User** | View + KYC-gated actions |
| **Verified User** | Full investment access |
| **Admin** | Administrative functions |

### Permission Guards

```typescript
// KYC verification guard
@Injectable()
export class KycGuard implements CanActivate {
  async canActivate(context: ExecutionContext): Promise<boolean> {
    const user = context.switchToHttp().getRequest().user;
    return user.kycStatus === 'VERIFIED';
  }
}

// Usage
@UseGuards(JwtAuthGuard, KycGuard)
@Post('invest')
async invest() { /* ... */ }
```

## Input Validation

### Backend Validation

```typescript
// DTO validation with class-validator
export class InvestDto {
  @IsString()
  @IsNotEmpty()
  loanId: string;

  @IsNumber()
  @Min(1)
  @Max(1000000)
  amount: number;

  @IsEthereumAddress()
  walletAddress: string;
}
```

### Validation Pipeline

```
Input → Validation Pipe → Sanitization → Business Logic
          ↓
     Reject Invalid
```

## Smart Contract Security

### Security Measures

| Measure | Purpose |
|---------|---------|
| **Access Control** | Restrict function access |
| **Reentrancy Guard** | Prevent reentrancy attacks |
| **Integer Safety** | Solidity 0.8+ overflow checks |
| **Input Validation** | Validate all parameters |

### Contract Patterns

```solidity
// Access control
modifier onlyOwner() {
    require(msg.sender == owner, "Not owner");
    _;
}

// Reentrancy protection
bool private locked;
modifier noReentrant() {
    require(!locked, "Reentrant call");
    locked = true;
    _;
    locked = false;
}

// Safe math (built into Solidity 0.8+)
function add(uint256 a, uint256 b) internal pure returns (uint256) {
    return a + b; // Automatically reverts on overflow
}
```

### Audit Process

```
1. Development Complete
        ↓
2. Internal Security Review
        ↓
3. External Audit
        ↓
4. Fix Identified Issues
        ↓
5. Verification Audit
        ↓
6. Deployment
```

## Data Security

### Encryption

| Data Type | Encryption |
|-----------|-----------|
| **Data in Transit** | TLS 1.3 |
| **Data at Rest** | AES-256 |
| **Sensitive Fields** | Field-level encryption |
| **Backups** | Encrypted |

### Data Classification

| Classification | Examples | Handling |
|---------------|----------|----------|
| **Public** | Loan listings | Open access |
| **Internal** | Analytics | Auth required |
| **Confidential** | KYC data | Encrypted + restricted |
| **Restricted** | Private keys | Never stored |

## Network Security

### API Security

| Measure | Implementation |
|---------|---------------|
| **HTTPS** | All traffic encrypted |
| **CORS** | Restricted origins |
| **Rate Limiting** | Request throttling |
| **Headers** | Security headers via Helmet |

### Security Headers

```typescript
// Helmet configuration
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", "data:", "https:"],
    }
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true
  }
}));
```

## Threat Mitigation

### Common Threats

| Threat | Mitigation |
|--------|------------|
| **SQL Injection** | Parameterized queries (Prisma) |
| **XSS** | Output encoding, CSP |
| **CSRF** | Token validation |
| **DDoS** | Rate limiting, CDN |
| **Replay Attack** | Nonce-based auth |

### Blockchain-Specific

| Threat | Mitigation |
|--------|------------|
| **Front-running** | Commit-reveal if needed |
| **Reentrancy** | Checks-effects-interactions |
| **Oracle Manipulation** | N/A (no oracles currently) |
| **Flash Loan Attack** | N/A (no DeFi composability) |

## Monitoring & Logging

### Security Monitoring

| What | How |
|------|-----|
| **Failed Auth** | Log and alert on patterns |
| **Rate Limit Hits** | Monitor and analyze |
| **Contract Events** | Monitor on-chain activity |
| **Error Rates** | Track and alert |

### Logging Policy

```typescript
// Security event logging
logger.warn('Authentication failed', {
  ip: request.ip,
  address: attemptedAddress,
  reason: 'Invalid signature',
  timestamp: new Date()
});
```

## Incident Response

### Response Process

```
1. DETECT
   Monitor alerts trigger
        ↓
2. ASSESS
   Determine severity and scope
        ↓
3. CONTAIN
   Limit damage/exposure
        ↓
4. INVESTIGATE
   Root cause analysis
        ↓
5. REMEDIATE
   Fix the issue
        ↓
6. COMMUNICATE
   Notify stakeholders
        ↓
7. REVIEW
   Post-incident analysis
```

### Severity Levels

| Level | Response Time | Example |
|-------|--------------|---------|
| **Critical** | Immediate | Smart contract exploit |
| **High** | < 4 hours | Auth bypass |
| **Medium** | < 24 hours | Data exposure |
| **Low** | < 72 hours | Minor vulnerability |

## User Security Guidance

### Wallet Security

| Practice | Importance |
|----------|------------|
| Secure seed phrase backup | Critical |
| Hardware wallet usage | Highly recommended |
| Verify transaction details | Critical |
| Use official links only | Critical |

### Warning Signs

| Warning | Action |
|---------|--------|
| Unexpected approval requests | Decline and verify |
| Unusual transaction amounts | Double-check |
| Requests for seed phrase | Never share |
| Unofficial communications | Verify source |

## Compliance

### Security Standards

| Standard | Relevance |
|----------|-----------|
| OWASP Top 10 | Web application security |
| Smart Contract Best Practices | Contract security |
| GDPR | Data protection (EU) |

### Audit Schedule

| Audit Type | Frequency |
|------------|-----------|
| Smart Contract | Major releases |
| Penetration Test | Annual |
| Code Review | Per release |
| Dependency Scan | Weekly |

## Related Pages

- [Security & Audits](../compliance/security-audits.md)
- [Risk Disclosures](../compliance/risk-disclosures.md)
- [Blockchain Layer](blockchain-layer.md)
