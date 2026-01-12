# Deployment Guide

Guide to deploying ReFlow to production.

## Overview

| Component | Recommended Platform |
|-----------|---------------------|
| Frontend | Vercel |
| Backend | Railway, Render, AWS |
| Database | Supabase, Railway, AWS RDS |
| Blockchain | Mantle Mainnet |

## Deployment Checklist

- [ ] Environment variables configured
- [ ] Database migrated
- [ ] Contracts deployed and verified
- [ ] CORS configured
- [ ] SSL/TLS enabled
- [ ] Monitoring setup

## Quick Links

- [Backend Deployment](backend.md)
- [Frontend Deployment](frontend.md)
- [Smart Contract Deployment](contracts.md)

## Environment Configuration

### Production Environment Variables

See [Environment Variables](../environment-variables.md) for complete list.

### Security Considerations

- Use strong, unique secrets
- Enable HTTPS everywhere
- Configure CORS properly
- Set up rate limiting
- Enable monitoring

## Post-Deployment

1. Verify all services running
2. Test critical flows
3. Monitor error rates
4. Set up alerts
