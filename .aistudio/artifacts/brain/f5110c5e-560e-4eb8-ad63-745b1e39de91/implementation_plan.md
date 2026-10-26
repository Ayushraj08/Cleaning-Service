# Redesign Implementation Plan — "Cleaning Service" Demo

Transform the current landing page into a clean, minimalist, conversion-focused residential cleaning service website titled **Cleaning Service**. Designed as a reusable sales demo for pitching local cleaning companies across North America (US/Canada), the UK, and Australia, this redesign balances authentic local credibility with zero-friction quote generation.

---

## User Review & Critical Decisions

> [!IMPORTANT]
> The following key decisions were confirmed during the initial consultation:
> - **Market Format**: Default showcase configured for **North America (US/Canada format)** with localized placeholders for easy localization into UK/AUD formats.
> - **Quote Capture Pattern**: Supported via **both an inline interactive quote section** with smooth prefilled service scrolling **and a quick-access header modal**, providing immediate conversion options across all viewport sizes.
> - **Brand Identity**: Named **Cleaning Service** with a modern geometric wordmark, clean botanical forest-green accent mark, and an all-in-one centralized configuration module (`CONFIG`) for quick white-label rebranding.

---

## 1. Overview & Core Concept

- **What It Does**: A complete, mobile-responsive local cleaning service landing page that communicates three things within 5 seconds:
  1. What services are offered (Regular Cleaning, Deep Cleaning, Move-In/Out).
  2. Which locations the company serves (Location 01: Central Metro, Location 02: North Suburbs).
  3. How to request a free quote or arrange service without friction.
- **Target Audience / Persona**: Homeowners, renters, and property managers seeking reliable, vetted residential cleaning; also serves as a turnkey sales demo shown to local cleaning business owners.
- **Key Value**: Replaces generic AI tropes, exaggerated claims, and cluttered layouts with understated Swiss-inspired architecture, genuine visual proof, transparent demo disclaimers, and a single dominant conversion path.

---

## 2. User Experience & Visual Design

### Key User Flows
1. **Hero Arrival & Value Scan**: Immediate exposure to the core promise (*“A cleaner home. More time for you.”*), trust line, service preview, and prominent *“Get a Free Quote”* action.
2. **Service Discovery & Prefill**: Visitors scan the 3 primary services (Regular, Deep, Move-In/Out). Clicking *“Request a Quote”* on any card automatically selects that service in the quote form and smoothly scrolls to it.
3. **Visual Verification (Before & After Slider)**: Visitors toggle between Kitchen and Living/Bath transformations, dragging an interactive 50% split divider to inspect cleaning results.
4. **Local Coverage & Trust**: Visitors verify local operating areas (two configurable location cards) and scan 3 transparent, authentic customer testimonials.
5. **Quote Submission**: Visitors fill a streamlined 2-column form (name, email, phone, property type, bedrooms/bathrooms, preferred date, notes) with instant validation and a demo confirmation state explaining next steps.
6. **Mobile Quick Action Bar**: On mobile viewports, a slim bottom sticky bar offers instant `Call` and `Get a Quote` actions within the safe 15% viewport height cap.

### Visual Identity & Theme
- **Color System (60-30-10 Rule)**:
  - *60% Dominant Canvas*: Clean warm ivory & crisp white (`#FFFFFF`, `#FAFAF7`, `#F4F6F3`).
  - *30% Structural Surfaces*: Deep forest green (`#14382B`, `#0E291F`) for authoritative headers and footers; soft sage tint (`#EBF2EE`, `#E2EBE5`) for cards and borders (`#D5E0D9`).
  - *10% Action Accent Budget*: Pure forest emerald (`#1B4D3E`, `#166534`) and crisp precision mint/sky accent (`#0284C7`) for interactive focus and active states.
- **Typography**:
  - *Display / Headings*: Plus Jakarta Sans (`font-extrabold tracking-tight`).
  - *Body / Form*: Plus Jakarta Sans / Inter (`font-normal leading-relaxed text-slate-700`).
  - *Metadata & Numbers*: Tabular numerals (`tabular-nums`) with unboxed dot separators (`·`).
- **Anti-Slop Discipline**:
  - Zero pill-badge sandwiches; no code comment headers (`// 01`); no pulsing neon dots.
  - Zero fabricated guarantee claims ($5M, fake policy numbers, or fake Google counts removed in favor of transparent, editable demo placeholders).
  - Single card elevation with subtle hairline borders (`1px solid #E2E8F0` / `#D5E0D9`).

---

## 3. Key Product Decisions & Trade-Offs

| Decision | Chosen Approach | Why | Alternatives Considered |
| :--- | :--- | :--- | :--- |
| **Brand Identity** | "Cleaning Service" with a clean wordmark | Fits the requested naming mandate while remaining 100% white-label friendly for client pitches. | Retaining "ShineCNDA" (confusing/inflexible) |
| **Quote Experience** | Dual-mode: Full inline section + Fast modal | Meets both user personas: visitors who want to browse before inquiring, and direct shoppers who want a quick quote from the header. | Modal-only (hurts SEO/page scan) or inline-only (requires scrolling from top) |
| **Before & After Slider** | CSS `clip-path: inset()` with 2 real paired scenes | Guarantees stationary, distortion-free images with smooth pointer/touch dragging on all screen sizes. | Canvas-based slider (heavy) or static side-by-side images (fails interactive brief) |
| **Configuration Architecture** | Central `CONFIG` object at top of code | Business name, phone, email, cities, services, and copy can be customized in under 2 minutes for client demos. | Hardcoded text across 15 different HTML tags |
| **Mobile Scrollbars** | CSS `scrollbar-width: none` & `::-webkit-scrollbar: none` | Prevents ugly desktop 17px gray scrollbars with arrows from breaking mobile responsive previews. | Default browser scrollbars (breaks mobile aesthetic) |

---

## 4. Technical Architecture & Data Strategy

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CENTRAL CONFIGURATION                           │
│  CONFIG { brand, market, contacts, services, locations, faqs, reviews }│
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ Hydrates & Renders
┌───────────────────────────────────▼────────────────────────────────────┐
│                      SINGLE-PAGE LAYOUT SECTIONS                       │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Header: Brand Wordmark · 4 Nav Links · Phone Link · "Get Free Quote"│
│ 2. Hero: 2-Column Split · Outcome Headline · Trust Line · Primary CTA  │
│ 3. Services: Exactly 3 Primary Cards (Regular, Deep, Move-In/Out)      │
│ 4. Before & After: Interactive Split Slider (Kitchen / Bathroom)       │
│ 5. Reviews: 3 Restrained Transparent Customer Cards                    │
│ 6. Where We Clean: 2 Configurable Regional Location Cards              │
│ 7. Free Quote Form: Inline 2-Column Form with Service Auto-Selection   │
│ 8. FAQ Accordion: Exactly 4 Expandable Items                           │
│ 9. Footer: Contact info · Areas · Legal Placeholders · Copyright       │
├────────────────────────────────────────────────────────────────────────┤
│                     MODALS & FLOATING CONTROLS                         │
│ • Quick-Quote Modal (Header CTA trigger)                               │
│ • Mobile Accessible Navigation Drawer                                  │
│ • Mobile Bottom Sticky Action Bar (<15% viewport height)               │
└────────────────────────────────────────────────────────────────────────┘
```

### Component & State Logic
- **`state.selectedService`**: Updates dynamically when a user clicks *"Request a Quote"* on any service card; automatically selects the corresponding radio/dropdown option in both the inline form and modal.
- **`state.activeSliderIndex`**: Toggles between Scene 0 (Kitchen Island Clean) and Scene 1 (Living/Bath Deep Clean). Maintains continuous 50% divider reset and touch tracking.
- **`state.quoteForm`**: Handles input validation (email regex, phone number, required fields), provides clear inline feedback, and displays a clean demo confirmation card with reference ID.
- **`CONFIG` Object**: Encapsulates brand wordmark, phone (`(555) 234-5678`), email (`quotes@cleaningservice.demo`), default city (`Greater Metro Area`), locations, services, and FAQ content for one-stop customization.

---

## 5. Execution Steps & Verification Checklist

1. **Centralize Data**: Construct the modular `CONFIG` object containing all customizable business information.
2. **Build Header & Mobile Drawer**: 3-zone top bar contract with working mobile hamburger drawer and phone link.
3. **Build Outcome Hero**: 2-column layout with provisional copy (*“A cleaner home. More time for you.”*), natural imagery, and clean trust indicators.
4. **Build 3 Service Cards**: Implement Regular, Deep, and Move-In/Out cards with *“Request a Quote”* prefill handlers.
5. **Implement 2-Scene Before & After Slider**: Draggable center divider with mouse/touch listeners, centered at 50%, with Before/After tags and scene toggles.
6. **Implement Reviews & 2 Locations**: Build the 3 review cards and 2 regional service area cards.
7. **Implement Free Quote Form & Modal**: Responsive fields with validation, prefilled service handling, and demo-safe confirmation screen.
8. **Implement 4 FAQ Accordions**: Smooth expand/collapse logic with accessible aria attributes.
9. **Build Footer & Mobile Sticky Bar**: Contact details, service areas, and sleek mobile action bar.
10. **Compilation & Verification**: Run `compile_applet` and `lint_applet` to confirm zero build errors or broken elements.
