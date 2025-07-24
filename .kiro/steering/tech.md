# HonestPup Technical Stack & Guidelines

## Technology Stack

### Frontend
- **Framework**: Next.js 14 (App Router)
- **Language**: TypeScript (required for all code)
- **Styling**: Tailwind CSS + shadcn/ui components
- **State Management**: React hooks + Zustand (when needed)
- **Forms**: React Hook Form + Zod validation
- **Icons**: Lucide React or Heroicons

### Backend & Database
- **Database**: Supabase PostgreSQL
- **Authentication**: Supabase Auth (multi-role: buyer, breeder, admin)
- **Storage**: Supabase Storage (images, documents)
- **Real-time**: Supabase Realtime
- **Functions**: Supabase Edge Functions
- **Security**: Row Level Security (RLS) on all tables

### Payments & External Services
- **Payment Processing**: Stripe Elements
- **Marketplace Payouts**: Stripe Connect
- **Financing**: Affirm via Stripe
- **Email**: SendGrid for transactional emails
- **SMS**: Twilio for notifications
- **Delivery**: CitizenShipper integration

### Infrastructure
- **Hosting**: Vercel
- **CDN**: Cloudflare
- **Monitoring**: Sentry + Vercel Analytics
- **CI/CD**: GitHub Actions

## Development Commands

### Setup
```bash
# Clone and install
git clone https://github.com/your-org/honestpup.git
cd honestpup
npm install

# Environment setup
cp .env.example .env.local
# Add Supabase and Stripe keys

# Development server
npm run dev
```

### Database Operations
```bash
# Run migrations
npm run db:migrate

# Reset database (development only)
npm run db:reset

# Generate types from Supabase
npm run db:types
```

### Build & Deploy
```bash
# Build for production
npm run build

# Run production build locally
npm run start

# Deploy (automatic via Vercel on push to main)
git push origin main
```

### Testing
```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run E2E tests
npm run test:e2e
```

## Code Standards

### TypeScript Requirements
- All code must be TypeScript
- Explicit types for all function parameters and returns
- Use Zod schemas for all user input validation
- Generate types from Supabase schema

### Component Patterns
```typescript
// Server components for data fetching
async function PuppyList() {
  const puppies = await fetchPuppies();
  return <PuppyGrid puppies={puppies} />;
}

// Client components for interactivity
'use client';
function FavoriteButton({ puppyId }: { puppyId: string }) {
  // Interactive behavior
}

// Type-safe database queries
const puppies = await supabase
  .from('puppies')
  .select('*, breeder:breeders(*)')
  .eq('status', 'available')
  .returns<PuppyWithBreeder[]>();
```

### Security Patterns
- All database access through Supabase RLS policies
- Server-side validation for all sensitive operations
- Input sanitization with Zod schemas
- Rate limiting on API endpoints

### Performance Requirements
- Page load speed < 3 seconds on 3G
- Mobile Lighthouse score > 90
- Images optimized with next/image
- Database queries < 500ms average response
- Client bundles < 200KB per route

## Architecture Principles

### Mobile-First Design
- Design for 375px width first
- Enhance for tablet (768px) and desktop (1024px+)
- Touch targets minimum 44x44px
- Thumb-friendly navigation

### Error Handling
```typescript
try {
  const result = await riskyOperation();
  return { success: true, data: result };
} catch (error) {
  console.error('Operation failed:', error);
  return { success: false, error: error.message };
}
```

### Loading States
- Always show loading UI for async operations
- Skeleton screens for content loading
- Optimistic UI updates where appropriate
- Progress indicators for uploads

## Environment Variables

### Required Variables
```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_key
SUPABASE_SERVICE_ROLE_KEY=your_key

# Stripe
STRIPE_SECRET_KEY=sk_live_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_xxx

# External Services
SENDGRID_API_KEY=your_key
TWILIO_AUTH_TOKEN=your_token
```

## Common Patterns

### Database Queries
- Use RLS policies for all data access
- Prefer server components for data fetching
- Use proper indexes for performance
- Implement soft deletes with `deleted_at`

### Form Handling
- React Hook Form + Zod validation
- Server-side validation as backup
- Clear error messages
- Loading states during submission

### Image Handling
- WebP format with fallbacks
- Lazy loading for below-fold images
- Responsive srcset
- Maximum 10MB upload, optimize to <500KB