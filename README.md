# Frontend Tooling and Project Structure Guide At BenchFive

## Introduction

This guide establishes the standard practices, tools, and project structure for frontend development. Following these guidelines ensures consistency, maintainability, and scalability across all projects.

## Development Environment

### Required Tools

-   **Code Editor**: VS Code
    -   Essential Extensions:
        -   ESLint
        -   Prettier
        -   GitLens
        -   Error Lens
        -   TypeScript + JavaScript Language Features
        -   Tailwind CSS IntelliSense

### Version Control Standards

-   **Platform**: GitHub
-   **Branching Strategy**: Trunk-Based Development
    -   Main branch: `main`
    -   Feature branches: `feature/feature-name`
    -   Bugfix branches: `fix/bug-description`
    -   Release branches: `release/version-number`
-   **Commit Convention**: Conventional Commits

    ```
    type(scope): description

    [optional body]

    [optional footer]
    ```

    Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

## Core Technology Stack

### Primary Technologies

-   **Framework**: Next.js 14+ (App Router)
-   **Language**: TypeScript 5.x
-   **Package Manager**: pnpm
-   **Styling**: Tailwind CSS + CSS Modules
-   **State Management**:
    -   Server State: React Query
    -   Client State: Zustand
-   **Form Management**: React Hook Form + Zod
-   **Testing**:
    -   Unit/Integration: Vitest + React Testing Library
    -   E2E: Playwright
-   **API Layer**: tRPC with Zod validation

### Build Tools & Configuration

-   **Bundler**: Next.js built-in (Turbopack)
-   **Linting**: ESLint with custom config
-   **Formatting**: Prettier
-   **Git Hooks**: husky + lint-staged
-   **CI/CD**: GitHub Actions

## Project Structure

```
project-root/
├── .github/                    # GitHub specific files
│   ├── workflows/             # GitHub Actions workflows
│   └── PULL_REQUEST_TEMPLATE.md
├── src/
│   ├── app/                   # Next.js App Router
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── (routes)/
│   ├── components/
│   │   ├── ui/               # Reusable UI components
│   │   │   ├── Button/
│   │   │   │   ├── index.ts
│   │   │   │   ├── Button.tsx
│   │   │   │   ├── Button.test.tsx
│   │   │   │   └── Button.types.ts
│   │   │   └── [other-components]/
│   │   └── features/         # Feature-specific components
│   ├── hooks/                # Custom React hooks
│   │   └── use-debounce.ts
│   ├── lib/                  # Third-party library configurations
│   │   ├── react-query.ts
│   │   └── zustand.ts
│   ├── server/              # Server-side code
│   │   ├── api/            # API routes and handlers
│   │   └── db/             # Database schemas and utilities
│   ├── styles/             # Global styles
│   │   ├── globals.css
│   │   └── variables.css
│   ├── types/              # TypeScript types
│   │   └── index.d.ts
│   └── utils/              # Utility functions
├── public/                 # Static assets
├── tests/
│   ├── e2e/               # Playwright tests
│   └── setup/             # Test configuration
└── config/                # Configuration files
    ├── eslint.config.js
    └── tailwind.config.js
```

## Development Standards

### Component Structure

```typescript
// Button.tsx
import { type ButtonProps } from './Button.types';
import { cn } from '@/utils/cn';

export const Button = ({ children, variant = 'primary', className, ...props }: ButtonProps) => {
	return (
		<button className={cn('px-4 py-2 rounded-md', variantStyles[variant], className)} {...props}>
			{children}
		</button>
	);
};
```

### Naming Conventions

-   **Files**:
    -   Components: PascalCase (`Button.tsx`)
    -   Utilities: camelCase (`formatDate.ts`)
    -   Types: PascalCase (`UserProfile.types.ts`)
-   **Variables**: camelCase
-   **Constants**: SCREAMING_SNAKE_CASE
-   **CSS Classes**: kebab-case
-   **Components**: PascalCase

### State Management Guidelines

1. **Server State**:

    ```typescript
    export const useUsers = () => {
    	return useQuery({
    		queryKey: ['users'],
    		queryFn: fetchUsers,
    		staleTime: 5 * 60 * 1000,
    	});
    };
    ```

2. **Client State**:
    ```typescript
    export const useStore = create<Store>((set) => ({
    	count: 0,
    	increment: () => set((state) => ({ count: state.count + 1 })),
    }));
    ```

## Testing Strategy

### Unit Testing

```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button', () => {
	it('renders with correct text', () => {
		render(<Button>Click me</Button>);
		expect(screen.getByRole('button')).toHaveTextContent('Click me');
	});

	it('handles click events', () => {
		const handleClick = vi.fn();
		render(<Button onClick={handleClick}>Click me</Button>);
		fireEvent.click(screen.getByRole('button'));
		expect(handleClick).toHaveBeenCalledTimes(1);
	});
});
```

### E2E Testing

```typescript
import { test, expect } from '@playwright/test';

test('user can log in', async ({ page }) => {
	await page.goto('/login');
	await page.fill('[name="email"]', 'user@example.com');
	await page.fill('[name="password"]', 'password');
	await page.click('button[type="submit"]');
	await expect(page).toHaveURL('/dashboard');
});
```

## CI/CD Pipeline

```yaml
name: CI
on: [push, pull_request]
jobs:
    test:
        runs-on: ubuntu-latest
        steps:
            - uses: actions/checkout@v4
            - uses: pnpm/action-setup@v2
            - name: Install dependencies
              run: pnpm install
            - name: Run tests
              run: pnpm test
            - name: Run E2E tests
              run: pnpm test:e2e
            - name: Build
              run: pnpm build
```

## Performance Guidelines

### Key Metrics

-   Largest Contentful Paint (LCP): < 2.5s
-   First Input Delay (FID): < 100ms
-   Cumulative Layout Shift (CLS): < 0.1
-   Time to Interactive (TTI): < 3.8s

### Optimization Checklist

1. Image Optimization
2. Code Splitting
3. Route Prefetching
4. Asset Minification
5. Caching Strategy
6. Performance Monitoring

## Security Guidelines

1. Input Validation
2. CSRF Protection
3. Content Security Policy
4. API Authentication
5. Secure Data Storage
6. Regular Dependencies Audit

## Accessibility Standards

1. ARIA Labels
2. Keyboard Navigation
3. Color Contrast
4. Screen Reader Support
5. Focus Management

## Getting Started

```bash
# Clone repository
git clone [repository-url]

# Install dependencies
pnpm install

# Set up environment variables
cp .env.example .env.local

# Start development server
pnpm dev

# Run tests
pnpm test
```

## Contributing

1. Fork the repository
2. Create feature branch
3. Commit changes following conventions
4. Push to the branch
5. Create Pull Request

For questions or support, contact the frontend team through the development Slack channel.
