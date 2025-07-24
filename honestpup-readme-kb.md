# HonestPup - The Honest Puppy Marketplace

> **AI Assistant Instructions**: This is the primary knowledge base for HonestPup. Always read this document first, then consult the specific documentation mentioned in each section. For database work, refer to `database_schema.md`. For UI/UX work, consult `design_guidelines.md`. For user flows, see `app_flow.md`.

## 🐕 Project Overview

**HonestPup** is a transparent, ethical puppy marketplace that connects verified breeders with families across the United States. We're fixing the broken puppy marketplace industry by eliminating hidden fees, ensuring fast breeder payments, and providing complete transparency throughout the adoption process.

### Core Mission
> "The honest way to find your puppy" - We believe in transparency, ethical breeding, and fair treatment for all parties involved.

### What Makes Us Different
- **No Hidden Fees**: The price you see is the price you pay
- **Fast Breeder Payments**: 20% within 3 days, 80% on delivery (vs. industry standard 4-6 weeks)
- **Complete Transparency**: See breeder profiles, reviews, and all costs upfront
- **Integrated Delivery**: Professional door-to-door delivery with live tracking (~$550 nationwide)
- **Flexible Payments**: 6, 12, or 18-month payment plans through Affirm

### Target Users
- **Primary**: Families looking for puppies who are tired of hidden fees and scams
- **Secondary**: Ethical breeders who want fair treatment and fast payments
- **Key Insight**: Current marketplaces hide fees, delay payments, and prevent direct communication

---

## 🚀 Quick Start for Developers

```bash
# Clone the repository
git clone https://github.com/your-org/honestpup.git
cd honestpup

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local
# Add your Supabase and Stripe keys

# Run development server
npm run dev

# Run database migrations
npm run db:migrate
```

### Tech Stack
```yaml
Frontend:
  - Next.js 14 (App Router)
  - TypeScript
  - Tailwind CSS
  - shadcn/ui components
  
Backend:
  - Supabase (PostgreSQL, Auth, Storage, Realtime)
  - Stripe (Payments & Connect)
  - Edge Functions
  
Infrastructure:
  - Vercel (Hosting)
  - Cloudflare (CDN)
  - SendGrid (Email)
  - Twilio (SMS)
```

---

## 📚 Documentation Structure

> **AI Agents**: Always consult these documents in order of relevance to your task:

1. **[masterplan.md](./docs/masterplan.md)** - Business model, platform overview, success metrics
2. **[app_flow.md](./docs/app_flow.md)** - Detailed user journeys, page flows, state management
3. **[design_guidelines.md](./docs/design_guidelines.md)** - Colors, typography, components, patterns
4. **[database_schema.md](./docs/database_schema.md)** - Tables, RLS policies, functions, triggers
5. **[implementation_plan.md](./docs/implementation_plan.md)** - Sprint planning, roadmap, milestones

### Quick Reference for AI Agents
```typescript
// When building features, check these sections:
DatabaseWork     → database_schema.md
UIComponents     → design_guidelines.md  
UserFlows        → app_flow.md
BusinessLogic    → masterplan.md
ProjectTimeline  → implementation_plan.md
```

---

## 🏗️ Architecture & Code Structure

### Project Structure
```
honestpup/
├── app/                    # Next.js 14 app directory
│   ├── (public)/          # Public routes
│   │   ├── page.tsx       # Landing page
│   │   ├── puppies/       # Browse & detail pages
│   │   └── breeders/      # Breeder profiles
│   ├── (auth)/            # Authenticated routes
│   │   ├── account/       # Buyer dashboard
│   │   ├── breeder/       # Breeder portal
│   │   └── admin/         # Admin panel
│   └── api/               # API routes
├── components/
│   ├── ui/                # shadcn/ui base components
│   ├── puppy/             # Puppy-specific components
│   ├── breeder/           # Breeder components
│   └── common/            # Shared components
├── lib/
│   ├── supabase/          # Supabase client & types
│   ├── stripe/            # Stripe integration
│   └── utils/             # Helper functions
├── types/                 # TypeScript definitions
├── styles/                # Global styles
└── public/                # Static assets
```

### Database Architecture
```sql
-- Core tables (see database_schema.md for full details)
profiles        → User accounts (buyers, breeders, admins)
breeders        → Breeder business info & verification
puppies         → Available puppies with photos/videos
orders          → Purchase transactions
breeder_payouts → Payment scheduling & tracking
reviews         → Breeder ratings
favorites       → Saved puppies
```

### Key Design Patterns
```typescript
// 1. Type-safe database queries
const puppies = await supabase
  .from('puppies')
  .select('*, breeder:breeders(*)')
  .eq('status', 'available')
  .returns<PuppyWithBreeder[]>();

// 2. Server components for data fetching
async function PuppyList() {
  const puppies = await fetchPuppies();
  return <PuppyGrid puppies={puppies} />;
}

// 3. Client components for interactivity
'use client';
function FavoriteButton({ puppyId }: { puppyId: string }) {
  // Interactive behavior
}

// 4. RLS policies for security (all data access goes through Supabase)
```

---

## 🎨 Design System

### Brand Colors
```css
/* Primary Palette */
--primary-green: #22C55E;     /* Trust, growth */
--accent-orange: #FB923C;     /* CTAs, energy */

/* Semantic Colors */
--success: #10B981;
--warning: #F59E0B;  
--error: #EF4444;

/* Always use Tailwind classes */
bg-green-500   → Primary actions
bg-orange-500  → CTAs
text-gray-700  → Body text
```

### Component Patterns
```tsx
// Puppy Card
<Card className="overflow-hidden hover:shadow-lg transition-shadow">
  <AspectRatio ratio={4/3}>
    <img src={puppy.primaryPhoto} />
  </AspectRatio>
  <CardContent>
    <h3 className="font-semibold">{puppy.name}</h3>
    <p className="text-green-600 font-bold">${puppy.price}</p>
  </CardContent>
</Card>

// Trust Badge
<Badge variant="outline" className="border-green-500">
  <CheckCircle className="w-4 h-4 mr-1" />
  Verified Breeder
</Badge>
```

---

## 🔒 Security & Privacy

### Core Security Principles
1. **RLS Everything**: All tables have Row Level Security enabled
2. **Server-First**: Sensitive operations only on server
3. **Input Validation**: Zod schemas for all user input
4. **API Protection**: Rate limiting on all endpoints

### Authentication Flow
```typescript
// Supabase Auth handles everything
- Email/password signup
- OAuth providers (Google, etc.)
- Session management
- Role-based access (buyer, breeder, admin)
```

### Payment Security
```typescript
// Stripe handles all payment data
- PCI compliance built-in
- Strong Customer Authentication (SCA)
- Webhook signature verification
- Idempotent requests
```

---

## 🚦 Development Guidelines

### Code Style
```typescript
// 1. Explicit types for everything
interface PuppyListing {
  id: string;
  name: string;
  breed: Breed;
  price: number;
  breeder: BreederProfile;
}

// 2. Error handling
try {
  const result = await riskyOperation();
  return { success: true, data: result };
} catch (error) {
  console.error('Operation failed:', error);
  return { success: false, error: error.message };
}

// 3. Loading states
const [isLoading, setIsLoading] = useState(false);
// Always show loading UI

// 4. Mobile-first responsive
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3">
```

### Testing Requirements
```typescript
// Test critical paths:
1. Puppy browsing → filtering → viewing details
2. Checkout flow → payment → confirmation  
3. Breeder dashboard → listing creation → payout tracking

// Use React Testing Library + Jest
test('displays puppy price correctly', () => {
  render(<PuppyCard puppy={mockPuppy} />);
  expect(screen.getByText('$2,500')).toBeInTheDocument();
});
```

### Performance Checklist
- [ ] Images optimized with next/image
- [ ] Database queries use proper indexes
- [ ] Client bundles < 200KB per route
- [ ] Core Web Vitals all green
- [ ] Mobile performance score > 90

---

## 🌟 Core Features

### For Buyers
1. **Advanced Search**: Filter by breed, location, price, size, ready date
2. **Transparent Pricing**: All costs shown upfront, no checkout surprises
3. **Breeder Profiles**: See reviews, verification, location before buying
4. **Secure Checkout**: Guest checkout, multiple payment options
5. **Delivery Tracking**: Live GPS updates from pickup to your door
6. **48-Hour Guarantee**: Full refund window after purchase

### For Breeders
1. **Free Listings**: No fees to list puppies
2. **Fast Payments**: 20% within 3 days, 80% on delivery
3. **Easy Management**: Dashboard for listings, sales, payouts
4. **Fair Commission**: Transparent fee structure
5. **Marketing Support**: Professional listing optimization
6. **Bulk Tools**: CSV upload for multiple puppies

### For Admins
1. **Application Review**: Breeder verification queue
2. **Financial Management**: Payout scheduling, refunds
3. **Content Moderation**: Listing and review management
4. **Analytics Dashboard**: Platform metrics, insights
5. **Support Tools**: Ticket management, user communication

---

## 📊 Business Logic

### Commission Structure
```typescript
// Commission is invisible to buyers
const commissionRate = 0.15; // 15%
const puppyPrice = 2500;     // What buyer sees/pays
const breederReceives = puppyPrice * (1 - commissionRate); // $2,125
```

### Payout Schedule
```typescript
// Automatic payout creation on order confirmation
const initialPayout = breederAmount * 0.20;  // 20% in 3 days
const finalPayout = breederAmount * 0.80;    // 80% on delivery

// Triggers in database handle this automatically
```

### Refund Policy
```typescript
// 48-hour window from purchase
if (hoursSincePurchase < 48) {
  // Full refund available
} else {
  // Case-by-case review
}
```

---

## 🚀 Deployment & Infrastructure

### Environment Variables
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

### Deployment Checklist
- [ ] Environment variables set
- [ ] Database migrations run
- [ ] Stripe webhooks configured
- [ ] Email templates uploaded
- [ ] CDN cache rules set
- [ ] Monitoring alerts configured

---

## 🐛 Troubleshooting

### Common Issues

#### RLS Policy Errors
```typescript
// Check user role
const { data: profile } = await supabase
  .from('profiles')
  .select('role')
  .single();

// Ensure policies match user type
```

#### Payment Failures
```typescript
// Always check Stripe dashboard
// Verify webhook signatures
// Check for test vs. live keys
```

#### Image Upload Issues
```typescript
// Max 10MB per image
// Supported: jpg, png, webp
// Auto-resize on upload
```

---

## 📈 Monitoring & Analytics

### Key Metrics
- **Conversion Rate**: Visitors → Purchases
- **Average Order Value**: Currently ~$2,500
- **Breeder Satisfaction**: Payout speed, ease of use
- **Support Tickets**: Response time < 2 hours

### Error Tracking
```typescript
// Sentry integration for production
Sentry.captureException(error, {
  tags: {
    section: 'checkout',
    user_type: 'buyer'
  }
});
```

---

## 🤝 Contributing

### Development Workflow
1. Create feature branch from `main`
2. Follow code style guidelines
3. Write tests for new features
4. Submit PR with description
5. Wait for code review
6. Merge after approval

### Commit Messages
```bash
feat: Add breeder dashboard analytics
fix: Correct payout calculation for partial refunds
docs: Update README with deployment steps
chore: Upgrade dependencies
```

---

## 📞 Support & Resources

- **Documentation**: See `/docs` folder
- **Discord**: [Join our community](#)
- **Email**: dev@honestpup.com
- **Issues**: GitHub Issues for bugs

---

## 🎯 Project Status

Currently in Phase 2 of 4 (see `implementation_plan.md`):
- ✅ Phase 1: Foundation (Complete)
- 🚧 Phase 2: Marketplace Core (In Progress)
- ⏳ Phase 3: Advanced Features
- ⏳ Phase 4: Polish & Launch

---

**Remember**: Every line of code should reflect our core values - honesty, transparency, and fair treatment for all. When in doubt, choose the option that's most transparent and fair to users.