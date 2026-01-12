# Backend Services

Detailed documentation of ReFlow's backend services architecture and implementation.

## Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         NestJS Backend                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌────────────┐ │
│  │    Auth     │  │   Market    │  │  Portfolio  │  │ Dashboard  │ │
│  │   Module    │  │   Module    │  │   Module    │  │  Module    │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └─────┬──────┘ │
│         │                │                │                │        │
│         └────────────────┴────────────────┴────────────────┘        │
│                                   │                                  │
│  ┌────────────────────────────────┴────────────────────────────────┐│
│  │                       Common Services                            ││
│  │   PrismaService  │  BlockchainService  │  ConfigService          ││
│  └──────────────────────────────────────────────────────────────────┘│
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## Core Services

### AuthService

Handles authentication and authorization.

```typescript
// Key methods
class AuthService {
  // Wallet-based authentication
  async walletLogin(address: string, signature: string): Promise<TokenPair>;

  // JWT token management
  async refreshToken(token: string): Promise<TokenPair>;

  // User profile
  async getProfile(userId: string): Promise<UserProfile>;

  // KYC status
  async checkKycStatus(userId: string): Promise<KycStatus>;
}
```

| Method | Purpose |
|--------|---------|
| `walletLogin` | Authenticate via wallet signature |
| `refreshToken` | Refresh expired access token |
| `getProfile` | Get user profile data |
| `checkKycStatus` | Verify KYC completion |

### MarketService

Manages loan marketplace operations.

```typescript
class MarketService {
  // Loan listing
  async getLoans(filters: LoanFilters): Promise<PaginatedLoans>;

  // Loan details
  async getLoanById(id: string): Promise<LoanDetails>;

  // Investment
  async createInvestment(dto: CreateInvestmentDto): Promise<Investment>;

  // Secondary market
  async getListings(filters: ListingFilters): Promise<Listings>;
}
```

| Method | Purpose |
|--------|---------|
| `getLoans` | List available loans with filters |
| `getLoanById` | Get detailed loan information |
| `createInvestment` | Record new investment |
| `getListings` | Secondary market listings |

### PortfolioService

Manages user portfolio data.

```typescript
class PortfolioService {
  // Holdings
  async getHoldings(userId: string): Promise<Holdings>;

  // Portfolio value
  async calculateValue(userId: string): Promise<PortfolioValue>;

  // Performance metrics
  async getPerformance(userId: string, period: Period): Promise<Performance>;

  // Transaction history
  async getTransactions(userId: string, filters: TxFilters): Promise<Transactions>;
}
```

### DashboardService

Provides analytics and metrics.

```typescript
class DashboardService {
  // User dashboard
  async getUserDashboard(userId: string): Promise<Dashboard>;

  // Platform stats
  async getPlatformStats(): Promise<PlatformStats>;

  // Performance charts
  async getChartData(userId: string, type: ChartType): Promise<ChartData>;
}
```

### BlockchainService

Handles all blockchain interactions.

```typescript
class BlockchainService {
  // Contract reads
  async getTokenBalance(address: string, token: string): Promise<bigint>;
  async getLoanDetails(contractAddress: string): Promise<LoanData>;

  // Event monitoring
  async subscribeToEvents(contract: string): void;
  async getHistoricalEvents(contract: string, fromBlock: number): Promise<Events>;

  // Transaction monitoring
  async waitForTransaction(txHash: string): Promise<TxReceipt>;
}
```

## API Structure

### Route Organization

```
/api/v1
├── /auth
│   ├── POST /login
│   ├── POST /register
│   ├── POST /refresh
│   └── GET  /profile
│
├── /market
│   ├── GET  /list
│   ├── GET  /:id
│   └── POST /invest
│
├── /portfolio
│   ├── GET  /
│   ├── GET  /holdings
│   └── GET  /transactions
│
├── /dashboard
│   ├── GET  /
│   └── GET  /stats
│
├── /investment-funds
│   ├── GET  /list
│   ├── GET  /:id
│   └── POST /invest
│
└── /history
    └── GET  /transactions
```

### Request/Response Flow

```
Request
   │
   ▼
┌──────────────────┐
│   Middleware     │  ← Authentication, validation
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│   Controller     │  ← Route handling, DTO validation
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    Service       │  ← Business logic
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│   Repository     │  ← Data access (Prisma)
└────────┬─────────┘
         │
         ▼
Response
```

## Authentication

### JWT Strategy

```typescript
// JWT payload structure
interface JwtPayload {
  sub: string;        // User ID
  address: string;    // Wallet address
  iat: number;        // Issued at
  exp: number;        // Expiration
}

// Token configuration
const jwtConfig = {
  accessToken: {
    expiresIn: '15m',
    secret: process.env.JWT_SECRET
  },
  refreshToken: {
    expiresIn: '7d',
    secret: process.env.JWT_REFRESH_SECRET
  }
};
```

### Auth Guards

```typescript
@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {
  // Protects routes requiring authentication
}

@Injectable()
export class KycGuard implements CanActivate {
  // Ensures KYC completion for certain routes
}
```

## Database Integration

### Prisma Setup

```typescript
// prisma.service.ts
@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit {
  async onModuleInit() {
    await this.$connect();
  }
}
```

### Example Query

```typescript
// portfolio.service.ts
async getHoldings(userId: string): Promise<Holdings> {
  return this.prisma.investment.findMany({
    where: { userId },
    include: {
      loan: true,
      transactions: true
    },
    orderBy: { createdAt: 'desc' }
  });
}
```

## Blockchain Integration

### Viem Setup

```typescript
// blockchain.service.ts
import { createPublicClient, http } from 'viem';
import { mantleSepoliaTestnet } from 'viem/chains';

const client = createPublicClient({
  chain: mantleSepoliaTestnet,
  transport: http('https://rpc.sepolia.mantle.xyz')
});
```

### Contract Reads

```typescript
async getTokenBalance(address: string, tokenAddress: string): Promise<bigint> {
  return client.readContract({
    address: tokenAddress,
    abi: erc20Abi,
    functionName: 'balanceOf',
    args: [address]
  });
}
```

## Error Handling

### Global Exception Filter

```typescript
@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();

    const status = exception instanceof HttpException
      ? exception.getStatus()
      : HttpStatus.INTERNAL_SERVER_ERROR;

    response.status(status).json({
      success: false,
      error: {
        code: status,
        message: this.getMessage(exception)
      }
    });
  }
}
```

### Custom Exceptions

```typescript
export class InsufficientFundsException extends HttpException {
  constructor() {
    super('Insufficient funds for this operation', HttpStatus.BAD_REQUEST);
  }
}

export class LoanNotFoundException extends HttpException {
  constructor(id: string) {
    super(`Loan with ID ${id} not found`, HttpStatus.NOT_FOUND);
  }
}
```

## Configuration

### Environment Variables

```env
# Server
PORT=30000
NODE_ENV=development

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/reflow

# JWT
JWT_SECRET=your-secret-key
JWT_REFRESH_SECRET=your-refresh-secret

# Blockchain
RPC_URL=https://rpc.sepolia.mantle.xyz
CHAIN_ID=5003

# External Services
RESTOCK_API_URL=https://api.restock.id
KYC_PROVIDER_KEY=your-kyc-key
```

### Config Module

```typescript
@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      validationSchema: configValidationSchema
    })
  ]
})
export class AppModule {}
```

## Testing

### Unit Tests

```typescript
describe('MarketService', () => {
  let service: MarketService;
  let prisma: PrismaService;

  beforeEach(async () => {
    const module = await Test.createTestingModule({
      providers: [
        MarketService,
        { provide: PrismaService, useValue: mockPrisma }
      ]
    }).compile();

    service = module.get<MarketService>(MarketService);
  });

  it('should return loans list', async () => {
    const result = await service.getLoans({});
    expect(result.data).toBeInstanceOf(Array);
  });
});
```

## Related Pages

- [Frontend Application](frontend-application.md)
- [API Reference](../developers/api-reference/README.md)
- [Tech Stack](tech-stack.md)
