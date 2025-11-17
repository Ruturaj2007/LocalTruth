# LocalTruth

**An Agentic AI Platform for Real-Time Misinformation Detection & Verification**

LocalTruth is a modern web application powered by AI for detecting, analyzing, and explaining potentially misleading claims. It provides real-time verification flows, multilingual support, community feedback integration, and a planned Chrome extension for on-the-fly claim checking.

## Table of Contents

- **Overview & Features**
- **Getting Started**
- **Tech Stack**
- **Project Structure**
- **Configuration & Environment**
- **Firebase & Data Model**
- **AI Flows & Verification**
- **Chrome Extension**
- **API Documentation**
- **Best Practices**
- **Contributing**

---

## Overview & Features

### Core Capabilities
- **Real-time Misinformation Detection** — Analyzes text using AI and web search for potential false claims.
- **Multilingual Support** — Full localization for 25+ languages (English, Arabic, Bengali, French, Spanish, etc.).
- **Community Feedback** — Users vote, comment, and contribute verification sources.
- **Confidence Scoring** — AI-calculated trust scores with supporting evidence and reasoning.
- **Chrome Extension (Planned)** — Browser integration for on-page claim checking.

### Key Technologies
- **Frontend:** Next.js, TypeScript, React, Tailwind CSS
- **Backend:** Firebase (Firestore, Authentication, Hosting)
- **AI/LLM:** Google Genkit, Gemini API
- **Search:** Google Custom Search API
- **UI Library:** Radix UI, Shadcn UI

---

## Getting Started

### Prerequisites
- **Node.js** v18 or higher
- **npm** or **pnpm**
- Firebase project (create one at [firebase.google.com](https://firebase.google.com))
- Google Gemini API key
- Google Custom Search Engine ID

### Quick Setup

1. **Clone & Install**
   ```bash
   cd '/Users/ruturajmahendrakar/Desktop/Other/SNW Practice/Local Truth'
   npm install
   ```

2. **Configure Environment**
   Copy `.env.local.example` to `.env.local` and add your API keys:
   ```
   NEXT_PUBLIC_FIREBASE_API_KEY=...
   NEXT_PUBLIC_FIREBASE_PROJECT_ID=...
   GEMINI_API_KEY=...
   GOOGLE_CX=...
   ```

3. **Run Dev Server**
   ```bash
   npm run dev
   ```
   Open [http://localhost:3000](http://localhost:3000) in your browser.

### Available Scripts
- `npm run dev` — Start development server
- `npm run build` — Build for production
- `npm run start` — Run production server
- `npm run lint` — Run linter
- `npm run typecheck` — TypeScript type checking

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | Next.js 14+, React 18+, TypeScript |
| **Styling** | Tailwind CSS, PostCSS |
| **UI Components** | Radix UI, Shadcn UI, Lucide Icons |
| **State & Context** | React Context (Firebase, Language) |
| **Backend** | Firebase (Firestore, Auth, Hosting) |
| **AI/LLM** | Google Genkit, Gemini API |
| **Search** | Google Custom Search API |
| **Internationalization** | i18n JSON files (25+ languages) |
| **Database** | Firestore (NoSQL) |

---

## Project Structure

```
src/
├── app/                    # Next.js routes & pages
│   ├── page.tsx           # Dashboard
│   ├── login/             # Authentication pages
│   ├── profile/
│   ├── settings/
│   └── extension/         # Extension integration
├── components/            # React components
│   ├── dashboard/         # Dashboard UI (claims, cards, dialogs)
│   ├── extension/         # Extension components
│   ├── layout/            # App layout, sidebar, navigation
│   ├── settings/          # User settings UI
│   └── ui/                # Base UI components (button, card, etc.)
├── ai/                    # AI flows & Genkit
│   ├── flows/             # AI flow definitions
│   │   ├── calculate-confidence-score.ts
│   │   ├── cross-reference-claims.ts
│   │   ├── detect-trending-misinformation.ts
│   │   ├── generate-search-query.ts
│   │   ├── summarize-verified-info.ts
│   │   └── translate-text.ts
│   ├── genkit.ts          # Genkit setup & config
│   └── dev.ts             # Development utilities
├── firebase/              # Firebase configuration
│   ├── config.ts
│   ├── provider.tsx
│   ├── client-provider.tsx
│   ├── firestore/         # Firestore helper functions
│   ├── error-emitter.ts
│   └── errors.ts
├── services/              # External service integrations
│   └── google-search.ts   # Google Search API
├── context/               # React Context
│   └── language-context.tsx
├── hooks/                 # Custom React hooks
│   ├── use-collection.tsx
│   ├── use-doc.tsx
│   └── use-mobile.tsx
├── lib/                   # Utilities & helpers
│   ├── types.ts
│   ├── utils.ts
│   ├── mock-claims.ts
│   └── placeholder-images.ts
├── locales/               # i18n translations (25+ languages)
│   ├── en.json
│   ├── fr.json
│   ├── es.json
│   └── ... (ar, bn, de, etc.)
└── globals.css            # Global styles

public/                    # Static assets

docs/                      # Project documentation
├── blueprint.md           # Style guide & feature overview
└── backend.json           # Data model & schema

config files at root:
- package.json
- tsconfig.json
- tailwind.config.ts
- next.config.ts
- firestore.rules
- apphosting.yaml
```

---

## Configuration & Environment

### Environment Variables (.env.local)

| Variable | Required | Description |
|----------|----------|-------------|
| `NEXT_PUBLIC_FIREBASE_API_KEY` | ✓ | Firebase API key |
| `NEXT_PUBLIC_FIREBASE_PROJECT_ID` | ✓ | Firebase project ID |
| `NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN` | ✓ | Firebase auth domain |
| `NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET` | ✓ | Firebase storage bucket |
| `GEMINI_API_KEY` | ✓ | Google Gemini API key |
| `GOOGLE_CX` | ✓ | Google Custom Search Engine ID |

### Key Config Files

**tailwind.config.ts** — Tailwind customization
- Color palette: primary, accent, background, sidebar
- Fonts: Inter (body), Space Grotesk (headings)
- Animations: Accordion, smooth transitions

**next.config.ts** — Next.js configuration
- Remote image domains: placehold.co, unsplash.com, picsum.photos
- Build optimization flags

**firestore.rules** — Security rules (see below)

**apphosting.yaml** — Firebase App Hosting backend config

---

## Firebase & Data Model

### Firestore Collections & Schema
#### Entity Overview

| Collection | Document Fields | Purpose |
|-----------|-----------------|---------|
| **claims** | `id`, `content`, `detectionTimestamp`, `lastUpdatedTimestamp`, `confidenceScore`, `status`, `language`, `explanation`, `upvotes`, `downvotes` | Detected and verified claims |
| **verifications** | `id`, `claimId`, `agentType`, `verificationTimestamp`, `result`, `explanation`, `sourceUrls[]` | Verification attempts per claim |
| **users** | `id`, `username`, `email`, `role`, `verified` | User profiles & metadata |
| **communityFeedback** | `id`, `claimId`, `userId`, `vote`, `comment`, `feedbackTimestamp` | User votes & feedback on claims |

#### Security Rules (firestore.rules)

- **Claims:** Public read; authenticated users only for create/update/delete
- **Verifications:** Authenticated users can CRUD verifications under claims
- **Community Feedback:** Authenticated users can submit feedback
- **Users:** Only the user themselves (or admins) can access their document

---

## AI Flows & Verification

### Genkit Flows (src/ai/flows/)

**detectTrendingMisinformation**
- Input: `webPageContent` (string)
- Uses Google Search + AI for fact-checking
- Output: `isMisinformation` (bool), `confidenceScore` (0–1), `reason` (string)

**crossReferenceClaims**
- Input: `claim` (string)
- Cross-references claim across sources
- Output: `isVerified` (bool), `confidenceScore`, `explanation`

**summarizeVerifiedInfo**
- Input: `claim`, `verificationResult`, `confidenceScore`, `language`, `communityFeedback`
- Produces concise, localized summaries
- Output: `summary` (string)

**generateSearchQuery**
- Input: `text` (string)
- Helps AI create effective fact-finding queries
- Output: `query` (string)

**calculateConfidenceScore**
- Input: `claim`, `supportingEvidence[]`, `refutingEvidence[]`
- Computes trust score with reasoning
- Output: `confidenceScore` (float), `reasoning` (string)

**translateText**
- Input: `text`, `targetLanguage`
- Translates text using Genkit
- Output: `translatedText` (string)

### Google Search Integration (src/services/google-search.ts)

Provides fact-checking context for AI flows. Uses Google Custom Search API to fetch web results relevant to claims.

---

## Chrome Extension

**Status:** Extension source is not yet in this repository. Coming soon.

### Planned Architecture

- **Content Script:** Scans page DOM for candidate claims, highlights or extracts them
- **Background Worker (MV3):** Routes messages, caches results, calls verification API
- **Popup/Panel:** User UI for checking claims, reviewing summaries, feedback
- **Server/API:** Runs heavy AI flows, returns confidence + sources

### Placeholders
- Extension source: `PATH_TO_EXTENSION` (to be added)
- API endpoint: `API_ENDPOINT` (to be added)
- Privacy policy: `PATH_TO_PRIVACY_POLICY` (to be added)

### Author Checklist (for Extension)
- [ ] Add extension source at `PATH_TO_EXTENSION`
- [ ] Provide `API_ENDPOINT` and sample response
- [ ] Include privacy policy URL at `PATH_TO_PRIVACY_POLICY`
- [ ] Add example screenshots
- [ ] Add `manifest.json` (MV3) and build script

---

## API Documentation

### Check Text for Misinformation

**Endpoint:** `POST /api/check-misinformation`

**Request Body:**
```json
{
  "text": "Your news headline or claim here"
}
```

**Response (200 - Success):**
```json
{
  "isMisinformation": true,
  "confidenceScore": 0.81,
  "reason": "Claim is not supported by reputable sources."
}
```

**Response (400 - Validation Error):**
```json
{
  "error": "Please enter at least 20 characters."
}
```

**Response (503 - Service Overloaded):**
```json
{
  "error": "The model is currently overloaded. Please try again later."
}
```

**Example cURL:**
```bash
curl -X POST "https://your-api-base-url.com/api/check-misinformation" \
  -H "Content-Type: application/json" \
  -d '{"text": "Your news headline here"}'
```

---

## Internationalization (i18n)

### Supported Languages (25+)
English, العربية (Arabic), বাংলা (Bengali), Deutsch (German), Español (Spanish), Français (French), हिन्दी (Hindi), Italiano (Italian), 日本語 (Japanese), 한국어 (Korean), Português (Portuguese), Русский (Russian), ไทย (Thai), Türkçe (Turkish), Українська (Ukrainian), Tiếng Việt (Vietnamese), 简体中文 (Chinese Simplified), 繁體中文 (Chinese Traditional), and 13+ more.

### Language Context (React)

- Uses `language-context.tsx` for state management
- Provides `t(key, values?)` function for translations
- Persists language preference to `localStorage`
- Loads JSON language files dynamically

### Adding a New Language

1. Create `src/locales/[lang-code].json` with all UI keys
2. Update `src/locales/languages.ts` to register the language
3. UI automatically picks up the new language in the language selector

---

## Core App Components

### App Layout (`src/components/layout/app-layout.tsx`)
- Sidebar navigation
- Dropdown user menu (profile, settings, logout)
- Mobile header with brand & sidebar toggle
- Responsive design

### Dashboard (`src/app/page.tsx`)
- Auth check, Firestore claim fetching
- Falls back to mock claims if offline
- Renders `ClaimList` component

### Claim Card (`src/components/dashboard/claim-card.tsx`)
- Displays claim details, status, confidence score
- Voting (upvote/downvote with optimistic UI)
- `ExplanationDialog` for AI-generated summary

### Misinformation Checker (`src/components/extension/misinformation-checker.tsx`)
- Text input for real-time analysis
- Calls `checkTextForMisinformation` action
- Displays confidence badge & sources
- Submits new claims to Firestore

### Profile & Settings
- **Profile Page:** View/update display name, email
- **Settings Page:** Change UI language, password

---

## Firebase & Authentication

### Context Providers

**FirebaseContext** — Provides:
- Firebase app instance
- Auth, Firestore, user state
- Loading & error states

**Custom Hooks:**
- `useUser()` — Current authenticated user
- `useAuth()` — Auth functions (login, logout, signup)
- `useFirestore()` — Firestore operations

### Non-Blocking Architecture

- Auth actions initiate immediately
- UI updates via Firebase event listeners
- Prevents blocking UI during network operations

### Error Handling

- Custom `FirestorePermissionError` class
- Event emitter for permission errors
- `FirebaseErrorListener.tsx` component for global error boundaries

---

## Best Practices & Recommendations

### Security & Privacy
- ⚠️ **Never commit API keys** — Use `.env.local` and add to `.gitignore`
- Ensure privacy policy is included and GDPR-compliant
- Chrome extension must document data transmission and retention
- Implement user opt-out for data collection

### Development Practices
- Always memoize Firestore queries
- Use event emitter for error handling
- Keep context providers at app root
- Write tests for AI flows and critical functions

### UI/UX
- Keep translations up-to-date in all 25+ locales
- Ensure responsive design across devices
- Provide clear user feedback for AI results
- Always show confidence scores with supporting evidence

---

## Contributing

### Workflow
1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Follow the style guide in `blueprint.md`
4. Write TypeScript, keep UI translatable
5. Test locally: `npm run dev`
6. Submit a pull request

### Guidelines
- Write new AI flows in `src/ai/flows/`
- Add UI components in `src/components/`
- Update translations in `src/locales/`
- Document new endpoints in README API section
- Keep commits atomic and well-messaged

---

## License & Contact

### License
This project uses the **MIT License**. See `LICENSE` file for details.

**Permissions:** Free use, modify, distribute, sublicense, and sell  
**Conditions:** Include copyright notice and license  
**Liability:** Provided "AS IS"; no warranty

### Maintainer
**GitHub:** [@HardikWahi07](https://github.com/HardikWahi07)

For issues, questions, or security concerns, open an issue on GitHub or contact the maintainer.

---

## Additional Resources

- **Style Guide & Blueprint:** See `blueprint.md` for design system and feature roadmap
- **Data Model:** See `backend.json` for detailed Firestore schema
- **Genkit Docs:** https://firebase.google.com/docs/genkit
- **Firebase Docs:** https://firebase.google.com/docs
- **Next.js Docs:** https://nextjs.org/docs

---

## Summary

LocalTruth is a comprehensive, production-ready platform for misinformation detection with:
- ✅ Real-time AI-powered verification
- ✅ 25+ language support
- ✅ Community-driven feedback
- ✅ Chrome extension integration (coming soon)
- ✅ Robust security & privacy controls

For questions or contributions, see the GitHub repository or contact the maintainer.
