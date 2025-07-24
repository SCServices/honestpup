# HonestPup Design Guidelines

## Brand Identity

### Mission Statement
HonestPup represents transparency, trust, and ethical connections in the puppy adoption industry. Our design should reflect honesty, warmth, and professionalism while maintaining an approachable, family-friendly aesthetic.

### Brand Values in Design
- **Transparency**: Clear information hierarchy, no hidden elements
- **Trust**: Professional yet warm, consistent experience
- **Simplicity**: Intuitive navigation, minimal cognitive load
- **Joy**: Celebrating the happiness of puppy adoption
- **Ethical**: Clean, honest presentation of information

## Color Palette

### Primary Colors
```css
--primary-green: #22C55E;      /* Trust, growth, nature */
--primary-green-dark: #16A34A; /* Hover states, emphasis */
--primary-green-light: #BBF7D0; /* Backgrounds, subtle accents */
```

### Secondary Colors
```css
--accent-orange: #FB923C;      /* CTAs, energy, warmth */
--accent-orange-dark: #F97316; /* Hover states */
--accent-orange-light: #FED7AA; /* Subtle highlights */
```

### Neutral Colors
```css
--gray-900: #111827; /* Primary text */
--gray-700: #374151; /* Secondary text */
--gray-500: #6B7280; /* Muted text, borders */
--gray-300: #D1D5DB; /* Borders, dividers */
--gray-100: #F3F4F6; /* Backgrounds */
--gray-50: #F9FAFB;  /* Light backgrounds */
--white: #FFFFFF;    /* Base background */
```

### Semantic Colors
```css
--success: #10B981;  /* Success states */
--warning: #F59E0B;  /* Warnings */
--error: #EF4444;    /* Errors */
--info: #3B82F6;     /* Information */
```

### Usage Guidelines
- **Primary Green**: Headers, primary buttons, success states, verified badges
- **Accent Orange**: CTAs, promotional elements, urgency indicators
- **Grays**: Text hierarchy, borders, backgrounds
- **White**: Primary background, cards, clean space

## Typography

### Font Stack
```css
--font-primary: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 
                'Helvetica Neue', Arial, sans-serif;
--font-mono: 'Courier New', monospace;
```

### Type Scale
```css
--text-xs: 0.75rem;    /* 12px - Labels, captions */
--text-sm: 0.875rem;   /* 14px - Secondary text */
--text-base: 1rem;     /* 16px - Body text */
--text-lg: 1.125rem;   /* 18px - Emphasized body */
--text-xl: 1.25rem;    /* 20px - Small headers */
--text-2xl: 1.5rem;    /* 24px - Section headers */
--text-3xl: 1.875rem;  /* 30px - Page headers */
--text-4xl: 2.25rem;   /* 36px - Hero headers (tablet+) */
--text-5xl: 3rem;      /* 48px - Hero headers (desktop) */
```

### Font Weights
- **Regular (400)**: Body text, descriptions
- **Medium (500)**: Navigation, labels
- **Semibold (600)**: Subheadings, emphasis
- **Bold (700)**: Headers, CTAs

### Line Heights
- **Tight (1.25)**: Headers
- **Normal (1.5)**: Body text
- **Relaxed (1.75)**: Long-form content

## Spacing System

### Base Unit: 4px
```css
--space-1: 0.25rem;  /* 4px */
--space-2: 0.5rem;   /* 8px */
--space-3: 0.75rem;  /* 12px */
--space-4: 1rem;     /* 16px */
--space-5: 1.25rem;  /* 20px */
--space-6: 1.5rem;   /* 24px */
--space-8: 2rem;     /* 32px */
--space-10: 2.5rem;  /* 40px */
--space-12: 3rem;    /* 48px */
--space-16: 4rem;    /* 64px */
--space-20: 5rem;    /* 80px */
```

### Usage Patterns
- **Component padding**: space-4 to space-6
- **Section spacing**: space-12 to space-20
- **Element gaps**: space-2 to space-4
- **Form fields**: space-3 vertical spacing

## Component Design

### Buttons

#### Primary Button (CTA)
```css
background: var(--accent-orange);
color: white;
padding: var(--space-3) var(--space-6);
border-radius: 8px;
font-weight: 600;
transition: all 0.2s;
hover: transform: translateY(-2px);
```

#### Secondary Button
```css
background: white;
color: var(--primary-green);
border: 2px solid var(--primary-green);
padding: var(--space-3) var(--space-6);
border-radius: 8px;
font-weight: 600;
```

#### Button Sizes
- **Small**: text-sm, padding space-2/space-4
- **Medium**: text-base, padding space-3/space-6
- **Large**: text-lg, padding space-4/space-8

### Cards

#### Puppy Card
```css
background: white;
border-radius: 12px;
overflow: hidden;
box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
transition: transform 0.2s;
hover: transform: translateY(-4px);
```

**Structure**:
- Image container (aspect-ratio: 4/3)
- Content padding: space-4
- Title: text-lg, font-semibold
- Price: text-xl, font-bold, color primary-green
- Meta info: text-sm, color gray-500
- Favorite button: absolute top-right

#### Feature Card
```css
background: white;
padding: var(--space-6);
border-radius: 12px;
border-top: 4px solid var(--primary-green);
```

### Forms

#### Input Fields
```css
border: 1px solid var(--gray-300);
border-radius: 6px;
padding: var(--space-3) var(--space-4);
font-size: var(--text-base);
focus: border-color: var(--primary-green);
focus: outline: 2px solid var(--primary-green-light);
```

#### Labels
```css
font-size: var(--text-sm);
font-weight: 500;
color: var(--gray-700);
margin-bottom: var(--space-2);
```

#### Error States
```css
border-color: var(--error);
background: #FEF2F2;
```

### Navigation

#### Desktop Nav
- Height: 64px
- Background: white
- Shadow: 0 1px 3px rgba(0, 0, 0, 0.1)
- Logo: text-2xl, font-bold
- Links: text-base, font-medium
- CTA button: accent-orange

#### Mobile Nav
- Hamburger menu: 24px icon
- Slide-out drawer: 80% width
- Full-height overlay
- Links: text-lg, padding space-4

## Layout Principles

### Grid System
- 12-column grid on desktop
- 4-column grid on tablet
- 2-column grid on mobile
- Gutter: space-6
- Max container width: 1200px

### Responsive Breakpoints
```css
--mobile: 320px;
--tablet: 768px;
--desktop: 1024px;
--wide: 1280px;
```

### Mobile-First Approach
1. Design for 375px width first
2. Enhance for tablet (768px)
3. Optimize for desktop (1024px+)

## Imagery Guidelines

### Puppy Photos
- **Aspect Ratio**: 4:3 for cards, 16:9 for hero
- **Quality**: Minimum 800x600px
- **Style**: Natural lighting, clear focus on puppy
- **Background**: Clean, uncluttered
- **Composition**: Puppy should fill 60-80% of frame

### Icons
- **Library**: Lucide React or Heroicons
- **Size**: 20px (small), 24px (default), 32px (large)
- **Stroke Width**: 2px
- **Color**: Inherit from parent

### Image Optimization
- WebP format with fallbacks
- Lazy loading for below-fold
- Responsive srcset
- Maximum file size: 10MB upload, optimized to <500KB

## Animation & Interactions

### Transitions
```css
--transition-fast: 150ms ease;
--transition-base: 200ms ease;
--transition-slow: 300ms ease;
```

### Hover Effects
- Buttons: Slight lift (-2px translateY)
- Cards: Shadow increase + lift
- Links: Color change to primary-green
- Images: Subtle zoom (scale 1.05)

### Loading States
- Skeleton screens for content
- Spinner for actions
- Progress bars for uploads
- Optimistic UI updates

### Micro-Interactions
- Favorite button: Heart fill animation
- Add to cart: Subtle bounce
- Form submission: Button state change
- Success states: Checkmark animation

## Accessibility

### WCAG 2.1 AA Compliance
- Color contrast: 4.5:1 minimum
- Focus indicators: 2px outline
- Touch targets: 44x44px minimum
- Alt text for all images

### Keyboard Navigation
- Logical tab order
- Skip links
- Focus trapping in modals
- Escape key closes overlays

### Screen Reader Support
- Semantic HTML
- ARIA labels where needed
- Live regions for updates
- Descriptive link text

## Component States

### Interactive Elements
1. **Default**: Base styling
2. **Hover**: Visual feedback
3. **Active**: Pressed state
4. **Focus**: Keyboard navigation
5. **Disabled**: 50% opacity, no-cursor

### Form Validation
1. **Empty**: Default state
2. **Focused**: Green border
3. **Valid**: Green checkmark
4. **Invalid**: Red border + message
5. **Loading**: Disabled + spinner

## Email Design

### Template Structure
- Max width: 600px
- Single column layout
- Logo at top
- Clear hierarchy
- Mobile-responsive

### Email Colors
- Limit to brand palette
- White background
- High contrast text
- Bulletproof buttons

## Do's and Don'ts

### Do's
- ✅ Use consistent spacing
- ✅ Maintain visual hierarchy
- ✅ Provide clear CTAs
- ✅ Show loading states
- ✅ Design for thumb reach
- ✅ Test on real devices

### Don'ts
- ❌ Hide important information
- ❌ Use more than 2 fonts
- ❌ Rely only on color
- ❌ Create tiny touch targets
- ❌ Forget empty states
- ❌ Skip error handling

## Design Handoff

### Developer Assets
- Figma components library
- Color tokens (CSS variables)
- Spacing tokens
- Icon set (SVG)
- Image templates

### Documentation
- Component usage guide
- State specifications
- Animation details
- Responsive behavior
- Accessibility notes