# Changelog

Version history and release notes for ReFlow.

## Version Format

We use semantic versioning: `MAJOR.MINOR.PATCH`

- **MAJOR**: Breaking changes or significant new features
- **MINOR**: New features, backward compatible
- **PATCH**: Bug fixes and minor improvements

---

## [0.2.0] - Testnet Release

### Added

**Smart Contracts**
- LoanToken ERC-20 implementation
- FactoryP2P for loan token deployment
- FactoryManager for protocol management
- Investment and repayment functionality
- Secondary market listing and trading

**Backend**
- Complete REST API (v1)
- User authentication with JWT
- Market endpoints for loan discovery
- Portfolio management endpoints
- Investment transaction handling
- Blockchain event indexing

**Frontend**
- Modern Next.js 16 application
- Wallet connection (RainbowKit)
- Marketplace interface
- Portfolio dashboard
- Investment flow
- Secondary market interface

**Infrastructure**
- Mantle Sepolia deployment
- PostgreSQL database
- Prisma ORM integration
- API documentation

### Security

- Non-custodial architecture
- Secure token approval flow
- Input validation throughout
- Rate limiting on API endpoints

---

## [0.1.0] - Alpha Release

### Added

**Smart Contracts**
- Initial LoanToken contract
- Basic factory pattern
- Investment functionality

**Backend**
- Basic API structure
- Database schema
- Authentication foundation

**Frontend**
- Basic UI components
- Wallet connection
- Simple marketplace view

### Known Issues

- Limited error handling
- Basic UI without polish
- Missing secondary market

---

## Upcoming

### [0.3.0] - Planned

**Features**
- Managed investment mode
- Enhanced portfolio analytics
- Improved secondary market UX
- Mobile-responsive improvements

**Technical**
- Performance optimizations
- Enhanced error handling
- API v1.1 with additional endpoints
- Improved indexing performance

### [1.0.0] - Mainnet Launch

**Features**
- Production-ready platform
- Full feature set
- Comprehensive documentation
- Support infrastructure

**Requirements**
- Security audit completion
- Regulatory compliance verification
- Partner integration complete
- Operations team ready

---

## Migration Notes

### Testnet to Mainnet

When migrating from testnet:

1. **Contracts**: New addresses on mainnet
2. **Tokens**: Testnet tokens have no value
3. **Data**: User data will not transfer
4. **Configuration**: Update all environment variables

### Breaking Changes

We'll document any breaking changes here when they occur, including:

- API endpoint changes
- Contract interface updates
- Configuration requirement changes
- Deprecation notices

---

## Deprecation Policy

### API Deprecation

- Minimum 30 days notice
- Documentation updates
- Migration guides provided
- Support during transition

### Contract Deprecation

- Minimum 90 days notice
- Migration tools provided
- Full user communication
- Extended support period

---

## Reporting Issues

### Bug Reports

Report bugs through:
- GitHub Issues
- Email: support@reflow.io
- Discord #bug-reports

Include:
- Version number
- Steps to reproduce
- Expected vs actual behavior
- Screenshots if applicable

### Security Issues

Report security issues privately:
- Email: security@reflow.io
- Do not disclose publicly
- Bug bounty program available

---

## Subscribe to Updates

Stay informed about new releases:

- **GitHub**: Watch releases on our repository
- **Discord**: Join #announcements channel
- **Twitter**: Follow @reflowfi
- **Email**: Subscribe to newsletter at reflow.io
