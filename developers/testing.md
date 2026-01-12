# Testing

Guide to testing ReFlow applications.

## Backend Testing

### Unit Tests

```bash
cd Backend
npm run test
```

### E2E Tests

```bash
npm run test:e2e
```

### Coverage

```bash
npm run test:cov
```

### Example Test

```typescript
// market.service.spec.ts
describe('MarketService', () => {
  let service: MarketService;

  beforeEach(async () => {
    const module = await Test.createTestingModule({
      providers: [MarketService, { provide: PrismaService, useValue: mockPrisma }]
    }).compile();

    service = module.get<MarketService>(MarketService);
  });

  it('should return loans list', async () => {
    const result = await service.getLoans({});
    expect(result.items).toBeInstanceOf(Array);
  });
});
```

## Frontend Testing

### Run Tests

```bash
cd Frontend
npm run test
```

### Example Component Test

```typescript
// LoanCard.test.tsx
import { render, screen } from '@testing-library/react';
import { LoanCard } from './LoanCard';

const mockLoan = {
  name: 'Test Loan',
  principal: '10000',
  interestRate: '12'
};

test('renders loan name', () => {
  render(<LoanCard loan={mockLoan} />);
  expect(screen.getByText('Test Loan')).toBeInTheDocument();
});
```

## Contract Testing

### Using Foundry

```bash
cd Contracts
forge test
```

### Example Test

```solidity
// LoanToken.t.sol
contract LoanTokenTest is Test {
    LoanToken token;

    function setUp() public {
        token = new LoanToken("Test", "TST", 10000, 1200, 365);
    }

    function testInvest() public {
        token.invest(1000);
        assertEq(token.balanceOf(address(this)), 1000);
    }
}
```

## Testing Tools

| Tool | Purpose |
|------|---------|
| Jest | Backend/Frontend unit tests |
| Supertest | API E2E tests |
| React Testing Library | Component tests |
| Foundry | Contract tests |

## Test Environment

Use separate database and testnet for testing:

```env
DATABASE_URL=postgresql://localhost/reflow_test
RPC_URL=https://rpc.sepolia.mantle.xyz
```
