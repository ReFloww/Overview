# Open Source

Open-source projects and licenses used in ReFlow.

## Our Commitment

ReFlow is built with and contributes to open-source software. We believe in the power of open-source collaboration and are committed to giving back to the community.

## Smart Contract Dependencies

### OpenZeppelin Contracts

**License:** MIT
**Version:** 5.x
**Repository:** [OpenZeppelin/openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts)

Used for:
- `ERC20.sol` - Token standard implementation
- `Ownable.sol` - Access control
- `ReentrancyGuard.sol` - Security

### Foundry Standard Library

**License:** MIT/Apache-2.0
**Repository:** [foundry-rs/forge-std](https://github.com/foundry-rs/forge-std)

Used for:
- Testing utilities
- Console logging
- Assertions

## Backend Dependencies

### NestJS

**License:** MIT
**Version:** 10.x
**Repository:** [nestjs/nest](https://github.com/nestjs/nest)

Core application framework.

### Prisma

**License:** Apache-2.0
**Version:** 5.x
**Repository:** [prisma/prisma](https://github.com/prisma/prisma)

Database ORM and toolkit.

### Ethers.js

**License:** MIT
**Version:** 6.x
**Repository:** [ethers-io/ethers.js](https://github.com/ethers-io/ethers.js)

Ethereum library for blockchain interactions.

### TypeScript

**License:** Apache-2.0
**Repository:** [microsoft/TypeScript](https://github.com/microsoft/TypeScript)

Type-safe JavaScript.

## Frontend Dependencies

### React

**License:** MIT
**Version:** 18.x
**Repository:** [facebook/react](https://github.com/facebook/react)

UI component library.

### Next.js

**License:** MIT
**Version:** 16.x
**Repository:** [vercel/next.js](https://github.com/vercel/next.js)

React framework for production.

### Wagmi

**License:** MIT
**Version:** 2.x
**Repository:** [wevm/wagmi](https://github.com/wevm/wagmi)

React hooks for Ethereum.

### Viem

**License:** MIT
**Version:** 2.x
**Repository:** [wevm/viem](https://github.com/wevm/viem)

TypeScript Ethereum interface.

### RainbowKit

**License:** MIT
**Version:** 2.x
**Repository:** [rainbow-me/rainbowkit](https://github.com/rainbow-me/rainbowkit)

Wallet connection UI.

### Tailwind CSS

**License:** MIT
**Version:** 4.x
**Repository:** [tailwindlabs/tailwindcss](https://github.com/tailwindlabs/tailwindcss)

CSS framework.

### Radix UI

**License:** MIT
**Repository:** [radix-ui/primitives](https://github.com/radix-ui/primitives)

Accessible UI primitives.

### Lucide Icons

**License:** ISC
**Repository:** [lucide-icons/lucide](https://github.com/lucide-icons/lucide)

Icon library.

## Development Tools

### ESLint

**License:** MIT
**Repository:** [eslint/eslint](https://github.com/eslint/eslint)

JavaScript linting.

### Prettier

**License:** MIT
**Repository:** [prettier/prettier](https://github.com/prettier/prettier)

Code formatting.

### Jest

**License:** MIT
**Repository:** [jestjs/jest](https://github.com/jestjs/jest)

Testing framework.

## Full License Texts

Complete license texts for all dependencies are available in:
- `LICENSES/` directory in our repositories
- `node_modules/*/LICENSE` files
- `lib/*/LICENSE` files in smart contract projects

## Contributing Back

We contribute to open-source through:

- **Bug Reports:** Reporting issues we discover
- **Pull Requests:** Contributing fixes and features
- **Documentation:** Improving docs for projects we use
- **Sponsorship:** Supporting critical open-source projects

## Security

If you discover a vulnerability in any open-source dependency we use:

1. Report to the original project maintainers
2. Notify us at security@reflow.io
3. We'll coordinate disclosure and updates

## Acknowledgment

We are deeply grateful to the open-source community. The developers and maintainers of these projects have made ReFlow possible. Thank you for your dedication to building software that benefits everyone.

---

*This list is not exhaustive. Run `npm list` or check `package.json` files for complete dependency information.*
