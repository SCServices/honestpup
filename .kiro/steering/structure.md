# HonestPup Project Structure & Organization

## Project Root Structure
```
honestpup/
├── app/                    # Next.js 14 app directory
├── components/             # React components
├── lib/                    # Utility libraries and configurations
├── types/                  # TypeScript type definitions
├── styles/                 # Global styles and Tailwind config
├── public/                 # Static assets
├── .kiro/                  # Kiro AI specifications and steering
├── docs/                   # Project documentation
└── tests/                  # Test files
```

## App Directory Structure (Next.js 14 App Router)
```
app/
├── (public)/              # Public routes (no auth required)
│   ├── page.tsx           # Landing page
│   ├── puppies/           # Browse & detail pages
│   │   ├── page.tsx       # Browse puppies
│   │   ├── [slug]/        # Individual puppy pages
│   │   └── breed/         # Breed-specific pages
│   ├── breeders/          # Breeder profiles
│   │   └── [id]/          # Individual breeder pages
│   ├── how-it-works/      # Process explanation
│   ├── about/             # Company information
│   └── support/           # Help and FAQ
├── (auth)/                # Authenticated routes
│   ├── account/           # Buyer dashboard
│   │   ├── page.tsx       # Account overview
│   │   ├── orders/        # Order history
│   │   ├── favorites/     # Saved puppies
│   │   └── settings/      # Account settings
│   ├── breeder/           # Breeder portal
│   │   ├── page.tsx       # Breeder dashboard
│   │   ├── inventory/     # Puppy management
│   │   ├── analytics/     # Sales analytics
│   │   ├── payouts/       # Payment tracking
│   │   └── settings/      # Breeder settings
│   └── admin/             # Admin panel
│       ├── page.tsx       # Admin dashboard
│       ├── applications/  # Breeder applications
│       ├── users/         # User management
│       ├── financial/     # Financial management
│       └── moderation/    # Content moderation
├── api/                   # API routes
│   ├── auth/              # Authentication endpoints
│   ├── puppies/           # Puppy CRUD operations
│   ├── orders/            # Order processing
│   ├── payments/          # Stripe webhooks
│   └── admin/             # Admin operations
├── globals.css            # Global styles
├── layout.tsx             # Root layout
└── loading.tsx            # Global loading component
```

## Components Structure
```
components/
├── ui/                    # shadcn/ui base components
│   ├── button.tsx
│   ├── card.tsx
│   ├── input.tsx
│   ├── dialog.tsx
│   └── ...
├── puppy/                 # Puppy-specific components
│   ├── PuppyCard.tsx
│   ├── PuppyGrid.tsx
│   ├── PuppyDetail.tsx
│   ├── PuppyFilters.tsx
│   └── PuppySearch.tsx
├── breeder/               # Breeder components
│   ├── BreederCard.tsx
│   ├── BreederProfile.tsx
│   ├── BreederDashboard.tsx
│   └── BreederApplication.tsx
├── order/                 # Order/checkout components
│   ├── CheckoutForm.tsx
│   ├── OrderSummary.tsx
│   ├── PaymentForm.tsx
│   └── OrderTracking.tsx
├── auth/                  # Authentication components
│   ├── LoginForm.tsx
│   ├── RegisterForm.tsx
│   └── AuthGuard.tsx
├── admin/                 # Admin-specific components
│   ├── AdminDashboard.tsx
│   ├── UserManagement.tsx
│   └── ApplicationReview.tsx
└── common/                # Shared components
    ├── Header.tsx
    ├── Footer.tsx
    ├── Navigation.tsx
    ├── LoadingSpinner.tsx
    ├── ErrorBoundary.tsx
    └── SEOHead.tsx
```

## Lib Directory Structure
```
lib/
├── supabase/              # Supabase configuration
│   ├── client.ts          # Client-side Supabase client
│   ├── server.ts          # Server-side Supabase client
│   ├── types.ts           # Generated database types
│   └── queries/           # Database query functions
│       ├── puppies.ts
│       ├── breeders.ts
│       ├── orders.ts
│       └── users.ts
├── stripe/                # Stripe integration
│   ├── client.ts          # Stripe client setup
│   ├── webhooks.ts        # Webhook handlers
│   └── connect.ts         # Stripe Connect for breeders
├── auth/                  # Authentication utilities
│   ├── middleware.ts      # Auth middleware
│   ├── guards.ts          # Route protection
│   └── roles.ts           # Role-based access
├── validations/           # Zod schemas
│   ├── puppy.ts
│   ├── breeder.ts
│   ├── order.ts
│   └── user.ts
├── utils/                 # Helper functions
│   ├── cn.ts              # Class name utility
│   ├── format.ts          # Formatting functions
│   ├── constants.ts       # App constants
│   └── api.ts             # API utilities
└── hooks/                 # Custom React hooks
    ├── useAuth.ts
    ├── usePuppies.ts
    ├── useOrders.ts
    └── useLocalStorage.ts
```

## Types Directory
```
types/
├── database.ts            # Generated Supabase types
├── api.ts                 # API response types
├── puppy.ts               # Puppy-related types
├── breeder.ts             # Breeder-related types
├── order.ts               # Order-related types
├── user.ts                # User-related types
└── common.ts              # Shared types
```

## Documentation Structure
```
.kiro/
├── specs/                 # Formal specifications
│   ├── honestpup-platform/
│   ├── buyer-journey/
│   ├── user-authentication/
│   ├── breeder-management/
│   ├── payment-processing/
│   └── admin-dashboard/
└── steering/              # AI guidance documents
    ├── product.md
    ├── tech.md
    └── structure.md

docs/ (root level)
├── honestpup-masterplan.md
├── honestpup-database-schema.md
├── honestpup-design-guidelines.md
├── honestpup-implementation-plan.md
├── honestpup-app-flow.md
└── honestpup-prompting-guide.md
```

## File Naming Conventions

### Components
- **PascalCase** for component files: `PuppyCard.tsx`
- **camelCase** for utility functions: `formatPrice.ts`
- **kebab-case** for pages: `how-it-works/page.tsx`

### Database
- **snake_case** for table names: `puppy_parents`
- **camelCase** for TypeScript interfaces: `PuppyWithBreeder`
- **SCREAMING_SNAKE_CASE** for constants: `MAX_UPLOAD_SIZE`

### API Routes
- **kebab-case** for endpoints: `/api/breeder-applications`
- **camelCase** for parameters: `?puppyId=123`

## Import Organization
```typescript
// 1. React and Next.js imports
import React from 'react';
import { NextRequest } from 'next/server';

// 2. Third-party libraries
import { z } from 'zod';
import { stripe } from 'stripe';

// 3. Internal utilities and configs
import { supabase } from '@/lib/supabase/client';
import { cn } from '@/lib/utils';

// 4. Components (UI first, then custom)
import { Button } from '@/components/ui/button';
import { PuppyCard } from '@/components/puppy/PuppyCard';

// 5. Types
import type { Puppy, Breeder } from '@/types';
```

## Environment-Specific Organization

### Development
- Use `.env.local` for local development
- Mock external services when possible
- Enable detailed logging and debugging

### Staging
- Use `.env.staging` for staging environment
- Test with real external services
- Enable performance monitoring

### Production
- Use environment variables in Vercel
- Enable all monitoring and alerting
- Optimize for performance and security

## Key Organizational Principles

1. **Feature-Based Grouping**: Components organized by domain (puppy, breeder, order)
2. **Separation of Concerns**: Clear boundaries between UI, business logic, and data
3. **Reusability**: Shared components in common/, specific ones in feature folders
4. **Type Safety**: Comprehensive TypeScript coverage with generated database types
5. **Security First**: All sensitive operations in server components or API routes
6. **Performance**: Lazy loading, code splitting, and optimized imports
7. **Maintainability**: Clear naming conventions and consistent file structure