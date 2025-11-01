# Testing Guide

This project includes automated tests for both Backend and Frontend.

## Backend Testing

### Setup

1. Install dependencies:
```bash
cd Fitmat-BackEnd
npm install
```

2. Set up test environment:
   - Create a `.env.test` file in `Fitmat-BackEnd/` directory
   - Add your test database URL:
   ```
   DATABASE_URL="your_test_database_url"
   JWT_SECRET="test-secret-key"
   ```

### Running Tests

```bash
cd Fitmat-BackEnd

# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage
```

### Test Structure

Tests are located in `Fitmat-BackEnd/src/__tests__/`:

- `auth.test.ts` - Authentication endpoints (register, login, logout)
- `jwt.test.ts` - JWT utility functions
- `trainer.test.ts` - Trainer endpoints

### Test Coverage

- Authentication API endpoints
- JWT token generation and verification
- Trainer listing endpoints
- Basic API contract validation

## Frontend Testing

### Setup

1. Install dependencies:
```bash
cd Fitmat-FrontEnd
npm install
```

### Running Tests

```bash
cd Fitmat-FrontEnd

# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage
```

### Test Structure

Tests are located in `Fitmat-FrontEnd/src/__tests__/`:

#### Utility Tests
- `utils/auth.test.ts` - Auth utility functions (parseJwt, isTokenValid, etc.)

#### Component Tests
- `components/Button.test.tsx` - Button component
- `components/Input.test.tsx` - Input component
- `components/LoadingSpinner.test.tsx` - LoadingSpinner component

### Test Coverage

- Authentication utility functions
- Common UI components
- Component interactions and states

## Writing New Tests

### Backend Test Example

```typescript
import request from 'supertest';
import express from 'express';
import yourRoutes from '../routes/your.routes';

const app = express();
app.use(express.json());
app.use('/api', yourRoutes);

describe('Your API', () => {
  it('should handle request correctly', async () => {
    const response = await request(app)
      .get('/api/endpoint')
      .expect(200);
    
    expect(response.body).toHaveProperty('data');
  });
});
```

### Frontend Test Example

```typescript
import React from 'react';
import { render, screen, fireEvent } from '@testing-library/react';
import YourComponent from '../../components/YourComponent';

describe('YourComponent', () => {
  it('renders correctly', () => {
    render(<YourComponent />);
    expect(screen.getByText('Hello')).toBeInTheDocument();
  });
});
```

## Continuous Integration

To run tests in CI/CD pipeline:

```bash
# Backend
cd Fitmat-BackEnd && npm test

# Frontend
cd Fitmat-FrontEnd && npm test
```

## Notes

- Backend tests require a test database (can be SQLite for testing)
- Frontend tests use jsdom for DOM simulation
- All tests should be independent and not rely on external services
- Mock external API calls and services in tests


