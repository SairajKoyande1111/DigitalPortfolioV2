# Design Guidelines: Digital Portfolio Website

## Design Approach

**Selected Approach:** Reference-Based Design drawing from Linear's minimalist sophistication, Apple's clean aesthetics, and Stripe's elegant card patterns.

**Design Philosophy:** Ultra-clean, interaction-driven experience where content takes center stage. Every element serves the purpose of showcasing work beautifully without distraction.

---

## Typography System

**Font Family:** 
- Primary: 'Inter' or 'DM Sans' from Google Fonts
- Monospace (for tech tags): 'JetBrains Mono'

**Type Scale:**
- Service card titles: text-2xl to text-3xl, font-semibold
- Project card titles: text-xl, font-semibold
- Project detail headings: text-4xl to text-5xl, font-bold
- Body text: text-base to text-lg
- Metadata/labels: text-sm, font-medium
- Technology tags: text-xs to text-sm, uppercase tracking-wide

---

## Layout System

**Spacing Primitives:** Use Tailwind units of 4, 6, 8, 12, 16, 24 for consistent rhythm (p-4, gap-6, mt-8, etc.)

**Container Strategy:**
- Main page: max-w-7xl mx-auto, centered service grid
- Projects gallery: max-w-7xl mx-auto with generous padding
- Project details: max-w-4xl mx-auto for optimal reading

**Grid Systems:**
- Service cards: grid-cols-1 md:grid-cols-2 lg:grid-cols-4 with gap-6 to gap-8
- Project cards: grid-cols-1 md:grid-cols-2 lg:grid-cols-3 with gap-6

---

## Component Specifications

### Service Cards (Main Page)
- Card size: Substantial presence, min-h-64 to min-h-80
- Rounded corners: rounded-2xl for modern feel
- Elevation: Subtle shadow that intensifies on hover
- Icon placement: Top-center or left-aligned, size-12 to size-16
- Content hierarchy: Icon → Title → Brief tagline (2-3 words)
- Interaction: Smooth scale transform on hover (scale-105), cursor-pointer
- Use Heroicons for service icons (Code, DevicePhone, Server, Megaphone)

### Project Cards (Gallery Page)
- Aspect ratio: Consistent 16:9 or 4:3 for thumbnails
- Image treatment: Full-width with rounded-xl, object-cover
- Card structure: Image + overlay gradient on hover + title/description below
- Spacing: p-6 internal padding for text content
- Hover state: Lift effect with enhanced shadow

### Project Details Page
- Hero image: Full-width, max-h-96 to max-h-screen/2, rounded-2xl with subtle shadow
- Information grid: Two-column layout (lg:grid-cols-3) for metadata sidebar + main content
- Metadata section: Sticky sidebar with client, duration, technologies
- Technology tags: Pill-shaped badges with monospace font, gap-2 flex-wrap
- Features list: Checklist style with icons, generous spacing between items
- Outcomes section: Highlighted with distinct background treatment

### Navigation Elements
- Back buttons: Positioned top-left, arrow-left icon + "Back" text, hover:underline
- All buttons: px-6 py-3, rounded-lg, font-medium transitions

---

## Page-Specific Layouts

### Main Page (Services)
- Full viewport centering: min-h-screen flex items-center justify-center
- Page title: Optional "Our Services" or brand name at top, text-5xl font-bold mb-16
- Grid takes visual focus with ample breathing room (py-20)

### Projects Gallery
- Page header: Service name as h1 (text-4xl font-bold) + back button
- Grid starts after generous top padding (pt-16)
- Empty state handling: If no projects, show elegant placeholder

### Project Details
- Vertical flow: Hero image → Title → Metadata grid → Description → Features → Outcomes
- Section spacing: space-y-12 to space-y-16 for clear visual separation
- Back button always visible at top

---

## Animations & Transitions

**Page Transitions:** Fade-in animations using opacity and transform (translate-y-4 → translate-y-0)

**Hover Effects:**
- Cards: Scale + shadow enhancement (duration-300 ease-in-out)
- Buttons: Subtle background shift only
- No complex animations - keep it performant and professional

**Loading States:** Simple fade-in for images (loading="lazy")

---

## Responsive Behavior

**Breakpoint Strategy:**
- Mobile (base): Single column, touch-optimized tap targets
- Tablet (md): Two-column grids, increased spacing
- Desktop (lg): Full grid layouts, maximum visual impact

**Touch Considerations:**
- Card tap areas: min-h-20 for comfortable mobile interaction
- Button sizing: py-4 on mobile for easy tapping

---

## Visual Hierarchy

**Emphasis Patterns:**
- Service cards: Equal visual weight, differentiated by icons only
- Project cards: Image-first approach, title prominence
- Project details: Hero image dominates, content flows naturally below

**Whitespace Usage:** Generous spacing between all major sections (space-y-12 minimum) to create breathing room and elegance

---

## Icons

**Library:** Heroicons (outline style for consistency)
**Usage:**
- Service icons: Distinct, recognizable symbols for each service type
- Back buttons: arrow-left
- Feature lists: check-circle
**Size:** w-12 h-12 for service cards, w-6 h-6 for inline usage

---

## Images

**Service Cards:** No background images - icon-based design
**Project Thumbnails:** Required for all project cards - placeholder images showing representative work (websites, apps, software UIs, marketing materials)
**Project Detail Hero:** Large, high-quality project screenshot or mockup as primary visual (max-h-96, full-width)