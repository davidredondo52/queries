# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is an e-commerce data utilities project that provides query functions for a SQLite database. The project uses TypeScript.

## Database Schema

The SQLite database contains tables for a complete e-commerce system including:

- customers, addresses, customer_segments, customer_activity_log
- products, categories, inventory, warehouses
- orders, order_items
- reviews
- promotions

See `schema.ts` for the complete database schema definition.

## Project Structure

- `src/main.ts` - Entry point (currently minimal implementation)
- `src/schema.ts` - Database schema creation functions
- `src/queries/` - Directory containing all query modules:
  - `customer_queries.ts` - Customer-related queries
  - `product_queries.ts` - Product catalog queries
  - `order_queries.ts` - Order management queries
  - `analytics_queries.ts` - Analytics and reporting queries
  - `inventory_queries.ts` - Inventory management queries
  - `promotion_queries.ts` - Promotion queries
  - `review_queries.ts` - Product review queries
  - `shipping_queries.ts` - Shipping queries

## Development Commands

```bash
# Install dependencies
npm run setup
```

## Git Configuration

This project is hosted on GitHub at [davidredondo52/queries](https://github.com/davidredondo52/queries).

### Initial Setup

```bash
# Configure git user (if not already configured)
git config user.email "davidredondo52@hotmail.com"
git config user.name "David Redondo"

# Add remote repository
git remote add origin https://github.com/davidredondo52/queries.git

# Push to GitHub
git push -u origin master
```

### Pushing Changes

```bash
# Stage changes
git add .

# Commit changes
git commit -m "Your commit message"

# Push to GitHub
git push
```

## Working with Queries

All query functions return Promises and follow these patterns:

- Single record queries use `db.get()`
- Multiple record queries use `db.all()`
- Use parameterized queries to prevent SQL injection
- Handle errors by rejecting the Promise

Example query pattern:

```typescript
export function getCustomerByEmail(db: Database, email: string): Promise<any> {
  const query = `SELECT * FROM customers WHERE email = ?`;
  return new Promise((resolve, reject) => {
    db.get(query, [email], (err, row) => {
      if (err) reject(err);
      else resolve(row);
    });
  });
}
```

## GitHub Actions & Secrets

This project uses GitHub Actions for CI/CD. The workflow is defined in `.github/workflows/ci.yml`.

### Setting up the ANTHROPIC_API_KEY secret

1. Go to your GitHub repository settings
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Value:** Your Anthropic API key from https://console.anthropic.com/keys

### Workflow Features

The CI/CD pipeline automatically:
- Runs on push to `master`, `main`, or `develop` branches
- Runs on pull requests to those branches
- Tests on Node.js 18.x and 20.x
- Installs dependencies
- Builds TypeScript
- Runs tests
- Makes `ANTHROPIC_API_KEY` available to all steps via environment variables

The secret is never exposed in logs and is only accessible during workflow execution.

## Critical Guidance

- Critical: All database queries must be written in the ./src/queries dir
