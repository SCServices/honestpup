# HonestPup App Flow Documentation

## User Journey Maps

### 1. Buyer Journey

#### 1.1 Discovery Phase
```
Landing Page → Browse Puppies → Search/Filter → View Results
                    ↓
            Create Account (Optional)
```

**Landing Page**
- Hero with value prop: "The Honest Way to Find Your Puppy"
- Trust signals (verification badges, reviews)
- CTA: "Browse Available Puppies"
- Secondary: "Learn How It Works"

**Browse Puppies Page**
- Grid view of available puppies
- Advanced filtering sidebar:
  - Breed (dropdown with search)
  - Gender (Male/Female/No Preference)
  - Location (State/City or Zip radius)
  - Price Range (slider)
  - Age (weeks/months)
  - Size (Toy/Small/Medium/Large)
  - Color
  - Ready Date (calendar picker)
  - Type filters:
    - Apartment Friendly
    - Hypoallergenic
    - Family Dogs
    - Doodle Breeds
    - Active Lifestyle
    - Non-Shedding
- Sort options: Price, Age, Newest Listed, Ready Date
- Favorite button on each card
- Quick view: Photo, Breed, Price, Location, Age

#### 1.2 Evaluation Phase
```
Puppy Grid → Puppy Detail Page → Breeder Profile (Optional)
     ↓              ↓                    ↓
Favorite ←→ Compare Favorites → Make Decision
```

**Puppy Detail Page** (Based on screenshots)
- Photo carousel with main image + thumbnails
- Video player for breeder video
- Key Information Panel:
  - Name and Price (prominent)
  - Breed, Gender, Age
  - Ready to go home date
  - Color, Weight, Registry
- "Take Me Home" CTA button
- "Add to Favorites" secondary button
- Tabbed Content:
  - About (description from breeder)
  - What's Included (vaccinations, health guarantee, etc.)
  - Meet Parents (parent info and photos)
  - Breeder Info (profile link, verification status, location)
- Trust Signals:
  - Verified Breeder badge
  - Health documentation status
  - Number of successful adoptions
  - Response time

**Breeder Profile Page**
- Breeder business name and location
- Verification badges (USDA, AKC, etc.)
- Years in business
- Specializes in (breeds)
- Customer reviews and ratings
- Current available puppies
- About section
- Policies (health guarantee, return policy)

#### 1.3 Purchase Phase
```
Puppy Detail → Checkout → Payment → Confirmation
                  ↓
           Login/Register
```

**Checkout Flow**
1. **Contact Information**
   - Full Name
   - Email Address
   - Phone Number
   - Option to login if existing customer (small link)
   - No account required to continue

2. **Delivery Information**
   - Delivery address
   - Preferred delivery date window
   - Special instructions
   - Same as billing address checkbox

3. **Payment Step**
   - Order Summary:
     - Puppy details and photo
     - Puppy price
     - Delivery fee (clearly shown)
     - Total amount
   - Payment Options:
     - Credit/Debit Card (Stripe)
     - Affirm financing (if eligible)
   - Billing address (if different from delivery)
   - Agreement checkboxes:
     - Terms of Service
     - 48-hour refund policy acknowledgment
     - Understanding this is a living animal

4. **Confirmation Page**
   - Order number
   - Order details summary
   - Next steps explanation
   - What to expect timeline
   - **Optional Account Creation**:
     - "Create an account to track your order and save your information for future purchases"
     - Password field
     - Create Account button
     - Skip button (continue as guest)
   - Contact support option

#### 1.4 Post-Purchase Phase
```
Confirmation → Account Dashboard → Track Order → Receive Puppy → Leave Review
                      ↓
              Email/SMS Updates
```

**Account Dashboard**
- Current Orders (with status)
- Order History
- Favorites
- Profile Settings
- Messages/Notifications

**Order Detail Page**
- Order information
- Puppy details
- Breeder information
- Delivery tracking (when available)
- Documents (health certificate, etc.)
- Support contact

### 2. Breeder Journey

#### 2.1 Application Phase
```
Landing Page → "Become a Breeder" → Application Form → Submit → Wait for Review
                                           ↓
                                    Email Confirmation
```

**Breeder Application Form**
- Business Information:
  - Breeder/Business Name
  - Full Legal Name
  - Physical Address
  - Phone Number
  - Email Address
  - Website (optional)
- Credentials:
  - USDA License Number
  - Years in Business
  - AKC or Other Memberships
- Breeding Information:
  - Breeds You Raise
  - Number of Breeding Dogs
  - Litters per Year
  - Average Litter Size
- Uploads:
  - USDA License
  - Facility Photos
  - References
- Agreement to Terms

#### 2.2 Onboarding Phase
```
Approval Email → Set Password → Complete Profile → Breeder Dashboard
                      ↓
              Onboarding Checklist
```

**Breeder Onboarding**
- Welcome tutorial
- Profile completion:
  - Business description
  - Policies (health guarantee, etc.)
  - Profile photo/logo
  - Additional certifications
- Banking setup (Stripe Connect)
- First listing walkthrough

#### 2.3 Listing Management Phase
```
Breeder Dashboard → Add Listing → Fill Details → Publish
        ↓               ↓
   Bulk Upload    Manage Inventory
```

**Breeder Dashboard**
- Overview Cards:
  - Active Listings
  - Pending Sales
  - This Month's Earnings
  - Pending Payouts
- Quick Actions:
  - Add New Puppy
  - View Messages
  - Update Availability
- Recent Activity Feed

**Add Individual Listing**
- Puppy Information:
  - Name
  - Breed (dropdown)
  - Gender
  - Birth Date
  - Color
  - Predicted Adult Weight
  - Price
  - Ready to Go Home Date
- Health Information:
  - Vaccination Schedule
  - Deworming Status
  - Health Clearances
  - Microchip (Y/N)
- Registration:
  - Registry (AKC, etc.)
  - Registration Type
  - Eligible for Registration
- Photos/Video:
  - Upload 3-10 photos (10MB max each)
  - Upload 1 video (optional)
  - Primary photo selection
- Description:
  - Personality traits
  - Special characteristics
  - What's included

**Bulk Upload**
- Download CSV template
- Fill template with multiple puppies
- Upload CSV
- Review and confirm entries
- Add photos to each listing

**Inventory Management**
- Tabs: Available / Pending / Sold
- Bulk actions: Update prices, Mark as sold
- Individual actions: Edit, Remove, Duplicate
- Calendar view for ready dates

#### 2.4 Sales Phase
```
New Order Alert → Review Order → Await Payment → Ship Puppy → Get Paid
       ↓              ↓              ↓
   Email/SMS    Dashboard Alert  20% Payout
```

**Order Management**
- New order notification
- Order details page:
  - Buyer information (name, location)
  - Puppy information
  - Payment status
  - Delivery coordination status
- Action items checklist:
  - Prepare health certificate
  - Coordinate with delivery
  - Upload final documents

**Payout Tracking**
- Pending payouts
- Payout history
- Transaction details
- Bank account management

### 3. Admin Journey

#### 3.1 Breeder Management
```
Admin Dashboard → Applications Queue → Review Application → Approve/Reject
                        ↓                     ↓
                  Active Breeders    Follow-up Required
```

**Application Review**
- Queue of pending applications
- Detailed application view
- Verification checklist
- Approve/Reject/Request More Info actions
- Notes and communication log

#### 3.2 Platform Management
```
Admin Dashboard → Monitoring Tools → Issue Detection → Resolution
       ↓               ↓                   ↓
   User Mgmt     Financial Mgmt    Content Moderation
```

**Admin Dashboard**
- Platform metrics
- Alerts and issues
- Recent activity
- Quick actions

**User Management**
- Search users (buyers/breeders)
- View user details
- Account actions (suspend, delete)
- Communication history

**Financial Management**
- Pending payouts
- Refund requests
- Transaction history
- Dispute resolution
- Manual payout triggers

## Page Specifications

### Public Pages

#### Landing Page
- **URL**: `/`
- **Purpose**: Convert visitors to browse or apply
- **Key Elements**: Hero, trust signals, value props, CTAs

#### Browse Puppies
- **URL**: `/puppies`
- **Purpose**: Discovery and search
- **Key Elements**: Filter sidebar, grid results, pagination

#### Puppy Detail
- **URL**: `/puppies/[id]`
- **Purpose**: Full information and purchase decision
- **Key Elements**: Photos, details, breeder info, CTA

#### Breeder Profile
- **URL**: `/breeders/[id]`
- **Purpose**: Build trust in breeder
- **Key Elements**: Credentials, reviews, available puppies

### Authenticated Pages

#### Buyer Dashboard
- **URL**: `/account`
- **Auth**: Required (Buyer)
- **Purpose**: Manage orders and profile
- **Key Elements**: Orders, favorites, settings

#### Breeder Dashboard
- **URL**: `/breeder/dashboard`
- **Auth**: Required (Breeder)
- **Purpose**: Business management hub
- **Key Elements**: Metrics, listings, payouts

#### Admin Dashboard
- **URL**: `/admin`
- **Auth**: Required (Admin)
- **Purpose**: Platform management
- **Key Elements**: All administrative tools

## Mobile Responsiveness

### Mobile-First Design Principles
1. **Touch-Friendly**: Minimum 44px touch targets
2. **Simplified Navigation**: Hamburger menu for secondary items
3. **Optimized Forms**: Native inputs, smart defaults
4. **Performance**: Lazy loading images, minimal JS

### Responsive Breakpoints
- Mobile: 320px - 767px
- Tablet: 768px - 1023px
- Desktop: 1024px+

### Mobile-Specific Features
- Swipeable photo galleries
- Sticky filter button (not sidebar)
- Simplified checkout flow
- Bottom navigation for key actions

## Error States & Edge Cases

### Common Error Scenarios
1. **Puppy No Longer Available**
   - Clear messaging
   - Suggest similar puppies
   - Remove from favorites

2. **Payment Failure**
   - Specific error message
   - Retry option
   - Support contact

3. **Breeder Application Issues**
   - Missing documents → Clear requirements
   - Invalid license → Verification help

4. **No Search Results**
   - Helpful message
   - Suggest broadening filters
   - Show new arrivals

### Loading States
- Skeleton screens for content
- Progress indicators for uploads
- Optimistic UI updates where appropriate

## Notification Touchpoints

### Email Notifications
1. **Buyers**
   - Order confirmation
   - Payment receipt
   - Delivery updates
   - Review request

2. **Breeders**
   - New order
   - Payment notifications
   - Review received
   - Monthly summary

### SMS Notifications
1. **Buyers**
   - Order confirmation
   - Delivery day reminder
   - Delivery completion

2. **Breeders**
   - Urgent order alerts
   - Payout confirmations

### In-App Notifications
- New messages
- Order status changes
- Price drops on favorited puppies
- System announcements