# Local Development Setup

Complete guide to setting up ReFlow for local development.

## Prerequisites

### Required Software

| Software | Version | Purpose |
|----------|---------|---------|
| Node.js | 18+ | JavaScript runtime |
| npm | 9+ | Package manager |
| PostgreSQL | 14+ | Database |
| Git | Latest | Version control |

### Verify Installation

```bash
node --version    # Should be v18.0.0 or higher
npm --version     # Should be v9.0.0 or higher
psql --version    # Should be 14.0 or higher
git --version     # Any recent version
```

## Quick Start

### 1. Clone Repository

```bash
git clone https://github.com/reflow/reflow.git
cd reflow
```

### 2. Setup Backend

```bash
cd Backend
npm install
cp .env.example .env
# Edit .env with your database credentials
npx prisma generate
npx prisma migrate dev --name init
npm run start:dev
```

### 3. Setup Frontend

```bash
cd ../Frontend
npm install
cp .env.example .env
# Edit .env with required keys
npm run dev
```

### 4. Access Application

- Frontend: http://localhost:3000
- Backend API: http://localhost:30000
- API Docs: http://localhost:30000/api/v1/docs

## Detailed Setup

### Database Setup

#### Install PostgreSQL

**macOS:**
```bash
brew install postgresql@14
brew services start postgresql@14
```

**Ubuntu:**
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
```

**Windows:**
Download from postgresql.org

#### Create Database

```bash
psql -U postgres
```

```sql
CREATE DATABASE reflow_db;
CREATE USER reflow_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE reflow_db TO reflow_user;
\q
```

### Backend Configuration

#### Environment Variables

Create `Backend/.env`:

```env
# Database
DATABASE_URL=postgresql://reflow_user:your_password@localhost:5432/reflow_db

# Server
PORT=30000
NODE_ENV=development

# JWT
JWT_SECRET=your-super-secret-jwt-key-change-in-production
JWT_REFRESH_SECRET=your-refresh-secret-key

# Blockchain
RPC_URL=https://rpc.sepolia.mantle.xyz
CHAIN_ID=5003
```

#### Run Migrations

```bash
cd Backend
npx prisma generate
npx prisma migrate dev --name init
```

#### Start Backend

```bash
npm run start:dev
```

Verify at: http://localhost:30000/api/v1/docs

### Frontend Configuration

#### Environment Variables

Create `Frontend/.env`:

```env
# API
NEXT_PUBLIC_API_BASE_URL=http://localhost:30000/api/v1

# Web3
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_walletconnect_id
NEXT_PUBLIC_USDT_ADDRESS=0xe01c5464816a544d4d0d6a336032578bd4629F10
NEXT_PUBLIC_ALCHEMY_API_KEY=your_alchemy_key

# Auth (optional)
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
NEXTAUTH_SECRET=your_nextauth_secret
NEXTAUTH_URL=http://localhost:3000
```

#### Get WalletConnect Project ID

1. Go to [cloud.walletconnect.com](https://cloud.walletconnect.com)
2. Create a new project
3. Copy the Project ID

#### Start Frontend

```bash
npm run dev
```

Access at: http://localhost:3000

## Project Structure

```
reflow/
├── Backend/
│   ├── src/
│   │   ├── auth/           # Authentication
│   │   ├── market/         # Loan marketplace
│   │   ├── portfolio/      # User portfolios
│   │   ├── dashboard/      # Analytics
│   │   ├── blockchain/     # Web3 integration
│   │   └── prisma/         # Database
│   ├── prisma/
│   │   └── schema.prisma   # Database schema
│   └── package.json
│
├── Frontend/
│   ├── src/
│   │   ├── app/            # Next.js pages
│   │   ├── components/     # React components
│   │   ├── lib/            # Utilities
│   │   └── hooks/          # Custom hooks
│   └── package.json
│
└── Overview/               # Documentation
```

## Development Commands

### Backend

```bash
# Start development server
npm run start:dev

# Start production server
npm run start:prod

# Run tests
npm run test

# Generate Prisma client
npx prisma generate

# Run migrations
npx prisma migrate dev

# Open Prisma Studio
npx prisma studio
```

### Frontend

```bash
# Start development server
npm run dev

# Build for production
npm run build

# Start production server
npm run start

# Run linter
npm run lint
```

## Troubleshooting

### Database Connection Failed

1. Verify PostgreSQL is running
2. Check DATABASE_URL in .env
3. Ensure user has permissions

### Port Already in Use

```bash
# Find process using port
lsof -i :3000
lsof -i :30000

# Kill process
kill -9 <PID>
```

### Prisma Issues

```bash
# Reset database
npx prisma migrate reset

# Regenerate client
npx prisma generate
```

### Module Not Found

```bash
# Clear node_modules and reinstall
rm -rf node_modules
npm install
```

## Next Steps

- [Configure environment variables](environment-variables.md)
- [Explore API endpoints](api-reference/README.md)
- [Set up Web3 integration](web3-integration/README.md)
