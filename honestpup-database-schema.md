# HonestPup Database Schema

## Overview
This document outlines the complete database schema for HonestPup using Supabase (PostgreSQL). It includes tables, relationships, RLS (Row Level Security) policies, triggers, and functions needed for the marketplace.

## Database Architecture Principles

1. **Security First**: All tables have RLS enabled by default
2. **Soft Deletes**: Use `deleted_at` timestamps instead of hard deletes
3. **Audit Trail**: Track created/updated timestamps and users
4. **UUID Primary Keys**: For better security and distribution
5. **Normalized Design**: Minimize redundancy while maintaining performance

## Core Tables

### 1. Users Table (extends Supabase Auth)
```sql
CREATE TABLE public.profiles (
    id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
    email TEXT NOT NULL,
    full_name TEXT NOT NULL,
    phone TEXT,
    role TEXT NOT NULL CHECK (role IN ('buyer', 'breeder', 'admin', 'support')),
    avatar_url TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    deleted_at TIMESTAMPTZ,
    UNIQUE(email)
);

-- RLS Policies
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view own profile" ON profiles
    FOR SELECT USING (auth.uid() = id);

CREATE POLICY "Users can update own profile" ON profiles
    FOR UPDATE USING (auth.uid() = id)
    WITH CHECK (auth.uid() = id AND role = (SELECT role FROM profiles WHERE id = auth.uid()));

CREATE POLICY "Admins can view all profiles" ON profiles
    FOR ALL USING (
        EXISTS (SELECT 1 FROM profiles WHERE id = auth.uid() AND role = 'admin')
    );
```

### 2. Breeders Table
```sql
CREATE TABLE public.breeders (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
    business_name TEXT NOT NULL,
    legal_name TEXT NOT NULL,
    address_line1 TEXT NOT NULL,
    address_line2 TEXT,
    city TEXT NOT NULL,
    state TEXT NOT NULL,
    zip_code TEXT NOT NULL,
    country TEXT DEFAULT 'US',
    usda_license_number TEXT,
    years_in_business INTEGER,
    website TEXT,
    bio TEXT,
    
    -- Verification
    status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'approved', 'suspended', 'rejected')),
    verified_at TIMESTAMPTZ,
    verified_by UUID REFERENCES profiles(id),
    rejection_reason TEXT,
    
    -- Business details
    breeds JSONB DEFAULT '[]', -- Array of breed names
    certifications JSONB DEFAULT '[]', -- AKC, etc.
    policies JSONB DEFAULT '{}', -- Health guarantee, return policy, etc.
    
    -- Banking (encrypted)
    stripe_account_id TEXT,
    payout_method TEXT CHECK (payout_method IN ('stripe', 'wire')),
    
    -- Stats
    total_sales INTEGER DEFAULT 0,
    total_revenue DECIMAL(10,2) DEFAULT 0,
    average_rating DECIMAL(3,2) DEFAULT 0,
    review_count INTEGER DEFAULT 0,
    
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    deleted_at TIMESTAMPTZ,
    
    UNIQUE(user_id)
);

-- RLS Policies
ALTER TABLE breeders ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Breeders can view own record" ON breeders
    FOR SELECT USING (user_id = auth.uid());

CREATE POLICY "Approved breeders are public" ON breeders
    FOR SELECT USING (status = 'approved');

CREATE POLICY "Breeders can update own record" ON breeders
    FOR UPDATE USING (user_id = auth.uid())
    WITH CHECK (user_id = auth.uid() AND status = status); -- Can't change own status

CREATE POLICY "Admins have full access" ON breeders
    FOR ALL USING (
        EXISTS (SELECT 1 FROM profiles WHERE id = auth.uid() AND role = 'admin')
    );
```

### 3. Puppies Table
```sql
CREATE TABLE public.puppies (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    breeder_id UUID NOT NULL REFERENCES breeders(id) ON DELETE CASCADE,
    
    -- Basic Info
    name TEXT NOT NULL,
    breed TEXT NOT NULL,
    gender TEXT NOT NULL CHECK (gender IN ('male', 'female')),
    birth_date DATE NOT NULL,
    color TEXT NOT NULL,
    weight_current DECIMAL(5,2),
    weight_adult_estimated DECIMAL(5,2),
    
    -- Pricing
    price DECIMAL(10,2) NOT NULL CHECK (price > 0),
    
    -- Status
    status TEXT NOT NULL DEFAULT 'available' 
        CHECK (status IN ('draft', 'available', 'reserved', 'sold', 'unavailable')),
    ready_date DATE NOT NULL,
    
    -- Health & Registration
    microchipped BOOLEAN DEFAULT false,
    health_records JSONB DEFAULT '{}',
    vaccinations JSONB DEFAULT '[]',
    registration_type TEXT,
    registration_papers BOOLEAN DEFAULT false,
    
    -- Description
    description TEXT,
    personality_traits JSONB DEFAULT '[]',
    included_items JSONB DEFAULT '[]',
    
    -- Media
    primary_photo_url TEXT,
    photos JSONB DEFAULT '[]', -- Array of photo URLs
    video_url TEXT,
    
    -- SEO
    slug TEXT UNIQUE,
    
    -- Stats
    view_count INTEGER DEFAULT 0,
    favorite_count INTEGER DEFAULT 0,
    
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    deleted_at TIMESTAMPTZ,
    
    -- Indexes
    INDEX idx_puppies_breed (breed),
    INDEX idx_puppies_status (status),
    INDEX idx_puppies_ready_date (ready_date),
    INDEX idx_puppies_price (price)
);

-- RLS Policies
ALTER TABLE puppies ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Anyone can view available puppies" ON puppies
    FOR SELECT USING (
        status = 'available' 
        AND deleted_at IS NULL
        AND EXISTS (
            SELECT 1 FROM breeders 
            WHERE breeders.id = puppies.breeder_id 
            AND breeders.status = 'approved'
        )
    );

CREATE POLICY "Breeders can manage own puppies" ON puppies
    FOR ALL USING (
        EXISTS (
            SELECT 1 FROM breeders 
            WHERE breeders.id = puppies.breeder_id 
            AND breeders.user_id = auth.uid()
        )
    );

CREATE POLICY "Admins have full access" ON puppies
    FOR ALL USING (
        EXISTS (SELECT 1 FROM profiles WHERE id = auth.uid() AND role = 'admin')
    );
```

### 4. Puppy Parents Table
```sql
CREATE TABLE public.puppy_parents (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    puppy_id UUID NOT NULL REFERENCES puppies(id) ON DELETE CASCADE,
    parent_type TEXT NOT NULL CHECK (parent_type IN ('mother', 'father')),
    name TEXT NOT NULL,
    breed TEXT,
    weight DECIMAL(5,2),
    photo_url TEXT,
    health_clearances JSONB DEFAULT '[]',
    
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Same RLS as puppies table
ALTER TABLE puppy_parents ENABLE ROW LEVEL SECURITY;

CREATE POLICY "View with puppy" ON puppy_parents
    FOR SELECT USING (
        EXISTS (
            SELECT 1 FROM puppies 
            WHERE puppies.id = puppy_parents.puppy_id
            AND puppies.status = 'available'
        )
    );
```

### 5. Orders Table
```sql
CREATE TABLE public.orders (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    order_number TEXT UNIQUE NOT NULL, -- Human readable: HNP-2024-0001
    
    -- Buyer info (can be guest)
    user_id UUID REFERENCES profiles(id),
    buyer_email TEXT NOT NULL,
    buyer_name TEXT NOT NULL,
    buyer_phone TEXT NOT NULL,
    
    -- Puppy & Breeder
    puppy_id UUID NOT NULL REFERENCES puppies(id),
    breeder_id UUID NOT NULL REFERENCES breeders(id),
    
    -- Pricing
    puppy_price DECIMAL(10,2) NOT NULL,
    delivery_fee DECIMAL(10,2) DEFAULT 0,
    total_amount DECIMAL(10,2) NOT NULL,
    
    -- Payment
    payment_method TEXT NOT NULL CHECK (payment_method IN ('card', 'affirm')),
    payment_status TEXT NOT NULL DEFAULT 'pending' 
        CHECK (payment_status IN ('pending', 'processing', 'succeeded', 'failed', 'refunded')),
    stripe_payment_intent_id TEXT,
    stripe_charge_id TEXT,
    
    -- Delivery
    delivery_address JSONB NOT NULL,
    delivery_date_requested DATE,
    delivery_date_actual DATE,
    delivery_instructions TEXT,
    delivery_status TEXT DEFAULT 'pending'
        CHECK (delivery_status IN ('pending', 'scheduled', 'in_transit', 'delivered')),
    
    -- Status
    status TEXT NOT NULL DEFAULT 'pending'
        CHECK (status IN ('pending', 'confirmed', 'preparing', 'shipped', 'delivered', 'completed', 'cancelled', 'refunded')),
    
    -- Refund window
    refund_eligible_until TIMESTAMPTZ,
    refund_requested_at TIMESTAMPTZ,
    refund_reason TEXT,
    refund_amount DECIMAL(10,2),
    
    -- Metadata
    ip_address INET,
    user_agent TEXT,
    
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    completed_at TIMESTAMPTZ,
    cancelled_at TIMESTAMPTZ,
    
    -- Indexes
    INDEX idx_orders_user (user_id),
    INDEX idx_orders_breeder (breeder_id),
    INDEX idx_orders_status (status),
    INDEX idx_orders_created (created_at)
);

-- RLS Policies
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view own orders" ON orders
    FOR SELECT USING (
        user_id = auth.uid() 
        OR buyer_email = (SELECT email FROM profiles WHERE id = auth.uid())
    );

CREATE POLICY "Breeders can view their orders" ON orders
    FOR SELECT USING (
        EXISTS (
            SELECT 1 FROM breeders 
            WHERE breeders.id = orders.breeder_id 
            AND breeders.user_id = auth.uid()
        )
    );

CREATE POLICY "Admins have full access" ON orders
    FOR ALL USING (
        EXISTS (SELECT 1 FROM profiles WHERE id = auth.uid() AND role = 'admin')
    );
```

### 6. Breeder Payouts Table
```sql
CREATE TABLE public.breeder_payouts (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    breeder_id UUID NOT NULL REFERENCES breeders(id),
    order_id UUID NOT NULL REFERENCES orders(id),
    
    -- Amounts
    order_amount DECIMAL(10,2) NOT NULL,
    commission_rate DECIMAL(5,2) NOT NULL, -- Percentage
    commission_amount DECIMAL(10,2) NOT NULL,
    payout_amount DECIMAL(10,2) NOT NULL,
    
    -- Payout details
    payout_type TEXT NOT NULL CHECK (payout_type IN ('initial', 'final')), -- 20% initial, 80% final
    payout_method TEXT NOT NULL CHECK (payout_method IN ('stripe', 'wire')),
    payout_status TEXT NOT NULL DEFAULT 'pending'
        CHECK (payout_status IN ('pending', 'processing', 'completed', 'failed')),
    
    -- Stripe/Wire details
    stripe_transfer_id TEXT,
    wire_reference TEXT,
    
    -- Timing
    scheduled_for TIMESTAMPTZ NOT NULL,
    initiated_at TIMESTAMPTZ,
    completed_at TIMESTAMPTZ,
    failed_at TIMESTAMPTZ,
    failure_reason TEXT,
    
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    
    -- Indexes
    INDEX idx_payouts_breeder (breeder_id),
    INDEX idx_payouts_status (payout_status),
    INDEX idx_payouts_scheduled (scheduled_for)
);

-- RLS Policies
ALTER TABLE breeder_payouts ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Breeders can view own payouts" ON breeder_payouts
    FOR SELECT USING (
        EXISTS (
            SELECT 1 FROM breeders 
            WHERE breeders.id = breeder_payouts.breeder_id 
            AND breeders.user_id = auth.uid()
        )
    );
```

### 7. Reviews Table
```sql
CREATE TABLE public.reviews (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    breeder_id UUID NOT NULL REFERENCES breeders(id),
    order_id UUID NOT NULL REFERENCES orders(id),
    user_id UUID NOT NULL REFERENCES profiles(id),
    
    rating INTEGER NOT NULL CHECK (rating >= 1 AND rating <= 5),
    title TEXT,
    comment TEXT,
    
    -- Review metadata
    verified_purchase BOOLEAN DEFAULT true,
    helpful_count INTEGER DEFAULT 0,
    
    -- Moderation
    status TEXT DEFAULT 'pending' CHECK (status IN ('pending', 'approved', 'rejected')),
    moderated_at TIMESTAMPTZ,
    moderated_by UUID REFERENCES profiles(id),
    
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    
    UNIQUE(order_id) -- One review per order
);

-- RLS Policies
ALTER TABLE reviews ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Approved reviews are public" ON reviews
    FOR SELECT USING (status = 'approved');

CREATE POLICY "Users can create own reviews" ON reviews
    FOR INSERT WITH CHECK (
        user_id = auth.uid()
        AND EXISTS (
            SELECT 1 FROM orders 
            WHERE orders.id = order_id 
            AND orders.user_id = auth.uid()
            AND orders.status = 'completed'
        )
    );

CREATE POLICY "Users can update own reviews" ON reviews
    FOR UPDATE USING (user_id = auth.uid())
    WITH CHECK (user_id = auth.uid());
```

### 8. Favorites Table
```sql
CREATE TABLE public.favorites (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID NOT NULL REFERENCES profiles(id),
    puppy_id UUID NOT NULL REFERENCES puppies(id),
    
    created_at TIMESTAMPTZ DEFAULT NOW(),
    
    UNIQUE(user_id, puppy_id)
);

-- RLS Policies
ALTER TABLE favorites ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can manage own favorites" ON favorites
    FOR ALL USING (user_id = auth.uid());
```

### 9. Breeder Applications Table
```sql
CREATE TABLE public.breeder_applications (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID REFERENCES profiles(id),
    
    -- Application data
    business_name TEXT NOT NULL,
    legal_name TEXT NOT NULL,
    email TEXT NOT NULL,
    phone TEXT NOT NULL,
    address JSONB NOT NULL,
    
    usda_license_number TEXT,
    years_breeding INTEGER,
    breeds JSONB DEFAULT '[]',
    litters_per_year INTEGER,
    memberships JSONB DEFAULT '[]', -- AKC, etc.
    
    -- Documents
    documents JSONB DEFAULT '[]', -- URLs to uploaded docs
    
    -- Review process
    status TEXT DEFAULT 'pending' 
        CHECK (status IN ('pending', 'under_review', 'approved', 'rejected', 'more_info_needed')),
    reviewed_by UUID REFERENCES profiles(id),
    reviewed_at TIMESTAMPTZ,
    review_notes TEXT,
    
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- RLS Policies
ALTER TABLE breeder_applications ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view own applications" ON breeder_applications
    FOR SELECT USING (user_id = auth.uid() OR email = (SELECT email FROM profiles WHERE id = auth.uid()));

CREATE POLICY "Users can create applications" ON breeder_applications
    FOR INSERT WITH CHECK (true);

CREATE POLICY "Admins have full access" ON breeder_applications
    FOR ALL USING (
        EXISTS (SELECT 1 FROM profiles WHERE id = auth.uid() AND