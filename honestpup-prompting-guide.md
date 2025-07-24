# HonestPup AI Prompting Guide & Templates

> **Purpose**: This guide provides proven prompting strategies and ready-to-use templates for building HonestPup efficiently with Lovable, Cursor, or other AI coding assistants.

## 🎯 Core Prompting Principles

### 1. Context-First Approach
Always provide the AI with:
- What you're building (specific feature)
- Where it fits in the app (page/component)
- Who will use it (buyer/breeder/admin)
- What problem it solves

### 2. Reference Documentation
Start prompts with: "Using the patterns from `[specific_doc].md`..."
- `database_schema.md` → Database operations
- `design_guidelines.md` → UI components
- `app_flow.md` → User journeys
- `masterplan.md` → Business logic

### 3. Incremental Building
Break complex features into smaller, testable chunks:
1. Data model first
2. Basic UI structure
3. Core functionality
4. Polish and edge cases

---

## 📋 Master Prompt Templates

### 🏗️ Feature Development Template

```markdown
I need to build [FEATURE NAME] for HonestPup.

Context:
- User Type: [buyer/breeder/admin]
- Location: [specific page/component]
- Purpose: [what problem this solves]

Requirements:
1. [Specific requirement 1]
2. [Specific requirement 2]
3. [Mobile-responsive design]
4. [Loading and error states]

Using patterns from:
- Database: [relevant tables from database_schema.md]
- Design: [component patterns from design_guidelines.md]
- Flow: [user journey from app_flow.md]

Success Criteria:
- [ ] Works on mobile (test at 375px)
- [ ] Handles loading states
- [ ] Shows appropriate errors
- [ ] Follows HonestPup design system
```

### 🎨 UI Component Template

```markdown
Create a [COMPONENT NAME] component for HonestPup.

Design Requirements:
- Follow color scheme: primary-green (#22C55E), accent-orange (#FB923C)
- Use shadcn/ui components as base
- Mobile-first responsive design
- Include hover states and transitions

Component Should:
1. [Primary function]
2. [Secondary function]
3. Show loading state when [condition]
4. Handle errors gracefully

Reference the [similar component] pattern from design_guidelines.md

Props:
- [prop1]: [type] - [description]
- [prop2]: [type] - [description]

Example Usage:
<ComponentName prop1={value} prop2={value} />
```

### 🗄️ Database Operation Template

```markdown
I need to [CREATE/READ/UPDATE/DELETE] [data type] in HonestPup.

Database Context:
- Table: [table name from database_schema.md]
- Related tables: [any joins needed]
- RLS policies: [relevant policies]

Operation Requirements:
1. [Specific requirement]
2. Check user permissions via RLS
3. Return typed response
4. Handle errors appropriately

Using Supabase client, create a function that:
- Input: [parameters with types]
- Output: [expected return type]
- Errors: [possible error cases]

Include proper TypeScript types based on database schema.
```

### 🔐 Authentication Flow Template

```markdown
Implement [AUTH FEATURE] for HonestPup.

Auth Requirements:
- User type: [buyer/breeder/admin]
- Allow guest checkout for buyers
- Redirect after auth: [destination]

Using Supabase Auth, create:
1. [UI component/page]
2. [Auth logic/hooks]
3. [Error handling]
4. [Success redirect]

Reference patterns:
- Guest checkout flow from app_flow.md
- Auth UI components from design_guidelines.md
- User roles from database_schema.md

Handle these cases:
- Existing user login
- New user signup
- Guest continuation
- Password reset
```

---

## 🚀 Common Feature Templates

### 1. Puppy Search & Filter

```markdown
Build the puppy search and filter functionality for the browse page.

Requirements:
- Filters: breed, gender, location, price range, age, size, color, ready date
- Special filters: apartment-friendly, hypoallergenic, family dogs
- Real-time results update
- Mobile-friendly filter drawer
- Show result count

Database: Query the `puppies` table with joins to `breeders`
Design: Use the filter sidebar pattern from design_guidelines.md
Performance: Implement debouncing for real-time updates

Include:
- Loading skeleton while fetching
- "No results" state with suggestions
- Filter chip badges for active filters
- Clear all filters button
```

### 2. Checkout Flow Component

```markdown
Create the checkout flow for HonestPup following our 3-step process.

Steps:
1. Contact Information (no account required)
2. Delivery Information  
3. Payment (Stripe + Affirm options)

Requirements:
- Guest checkout (no forced account creation)
- Show puppy details in sidebar/summary
- Calculate and display delivery fee clearly
- Mobile-optimized form inputs
- Progress indicator

After successful payment:
- Option to create account
- Order confirmation with next steps
- Send confirmation email

Reference checkout flow from app_flow.md and use Stripe Elements.
```

### 3. Breeder Dashboard

```markdown
Build the breeder dashboard overview page.

Display:
- Active listings count
- Pending sales
- This month's earnings  
- Pending payouts
- Recent activity feed

Quick Actions:
- Add new puppy button
- View all listings
- Check payout schedule

Use these tables from database_schema.md:
- breeders (for stats)
- puppies (for listings)
- orders (for sales)
- breeder_payouts (for payment info)

Follow dashboard card pattern from design_guidelines.md
Make it mobile-responsive with stacked cards on small screens.
```

### 4. Puppy Card Component

```markdown
Create a reusable PuppyCard component for grid displays.

Card should show:
- Primary photo (4:3 aspect ratio)
- Puppy name and breed
- Price (prominent, green color)
- Breeder location (city, state)
- Age and ready date
- Favorite button (heart icon)
- Verified breeder badge if applicable

Interactions:
- Hover: slight lift with shadow
- Click: navigate to puppy detail page
- Favorite: optimistic UI update
- Image: lazy load with blur placeholder

Use the card pattern from design_guidelines.md
Make it responsive: full width on mobile, fixed width on desktop.
```

---

## 💡 Prompting Best Practices

### DO's ✅

1. **Be Specific About State**
   ```markdown
   "When the user clicks submit, show loading state, disable the button, 
   and display success message after completion"
   ```

2. **Include Error Scenarios**
   ```markdown
   "Handle these error cases: network failure, invalid data, 
   unauthorized access. Show user-friendly messages."
   ```

3. **Specify Data Types**
   ```markdown
   "Expect an array of PuppyListing objects with id: string, 
   name: string, price: number, and breeder: BreederProfile"
   ```

4. **Request Mobile Consideration**
   ```markdown
   "Ensure this works on mobile viewports starting at 375px. 
   Use touch-friendly tap targets (min 44px)."
   ```

### DON'T's ❌

1. **Don't Be Vague**
   ```markdown
   ❌ "Make a form"
   ✅ "Create a breeder application form with fields for business name, 
       USDA license, breeds, experience years"
   ```

2. **Don't Skip Context**
   ```markdown
   ❌ "Add search"  
   ✅ "Add puppy search to the browse page that filters by breed, 
       price, and location using the puppies table"
   ```

3. **Don't Forget Edge Cases**
   ```markdown
   ❌ "Show the puppies"
   ✅ "Show puppies in a grid, handle empty state, loading state, 
       and error state with appropriate messages"
   ```

---

## 🔧 Debugging & Optimization Prompts

### Performance Optimization
```markdown
Optimize the [COMPONENT/FEATURE] for better performance.

Current issues:
- [Issue 1]
- [Issue 2]

Consider:
- Image lazy loading with next/image
- Component memoization where appropriate  
- Database query optimization
- Client-side caching
- Debouncing user inputs

Maintain functionality while improving speed.
```

### Bug Fixing
```markdown
Debug this issue in [FEATURE]:

Error: [error message]
When: [steps to reproduce]
Expected: [what should happen]
Actual: [what happens instead]

Check:
- Console for errors
- Network tab for failed requests
- RLS policies if database-related
- Component props and state

Provide fix with explanation of root cause.
```

### Refactoring
```markdown
Refactor [COMPONENT/FEATURE] to be more maintainable.

Goals:
- Extract reusable logic into hooks
- Improve TypeScript types
- Add proper error boundaries
- Enhance accessibility
- Add unit tests for critical paths

Keep existing functionality intact.
```

---

## 📱 Mobile-First Templates

### Responsive Component
```markdown
Make [COMPONENT] fully responsive with mobile-first approach.

Breakpoints:
- Mobile: 320px - 767px (base)
- Tablet: 768px - 1023px  
- Desktop: 1024px+

Mobile should:
- Stack elements vertically
- Use full width
- Have larger touch targets
- Show condensed information

Tablet/Desktop should:
- Use grid/flex layouts
- Show additional information
- Have hover states

Test at 375px, 768px, and 1440px widths.
```

---

## 🧪 Testing Templates

### Component Test
```markdown
Write tests for [COMPONENT] using React Testing Library.

Test cases:
1. Renders with required props
2. Handles user interactions
3. Shows loading state
4. Displays errors appropriately
5. Accessibility (ARIA labels)

Mock:
- Supabase queries
- User authentication
- External API calls

Achieve >80% coverage for this component.
```

---

## 🎨 Quick Copy Templates

### Form Fields
```markdown
Create form fields for [FORM PURPOSE]:
- Full Name (required, text)
- Email (required, email validation)
- Phone (optional, format: (xxx) xxx-xxxx)
- [Additional fields]

Use react-hook-form with zod validation.
Show inline errors below each field.
```

### Data Tables
```markdown
Create a data table for [DATA TYPE]:
- Columns: [col1, col2, col3]
- Sortable by: [columns]
- Filterable by: [columns]
- Pagination: 20 items per page
- Mobile: Card view instead of table

Include bulk actions: [action1, action2]
```

### Modal Dialogs
```markdown
Create a modal for [PURPOSE]:
- Trigger: [button/link text]
- Content: [what to show]
- Actions: [Cancel, Confirm]
- Close on: ESC key, backdrop click, or button

Use shadcn/ui Dialog component.
Add appropriate ARIA labels.
```

---

## 🚦 Progressive Enhancement

### Start Simple
```markdown
First, create a basic version of [FEATURE] that:
- Works without JavaScript
- Has semantic HTML
- Functions on slow connections
- Is accessible via keyboard
```

### Then Enhance
```markdown
Now enhance [FEATURE] with:
- Client-side interactivity
- Optimistic updates
- Real-time features
- Smooth animations
- Advanced error handling
```

---

## 📝 Documentation Prompt

```markdown
Document the [FEATURE/COMPONENT] you just built:

Include:
1. What it does
2. Props/parameters with types
3. Usage example
4. Key dependencies
5. Any business logic notes

Format as JSDoc comments and update README if needed.
```

---

## 🎯 Remember

1. **Always reference the docs** - Point AI to specific files
2. **Think mobile-first** - Every feature must work on phones
3. **Handle all states** - Loading, error, empty, success
4. **Type everything** - Full TypeScript coverage
5. **Stay honest** - Our brand is transparency

Use these templates as starting points and customize based on specific needs. The key is providing clear context and expected outcomes to get the best results from AI assistants.