# Folder Structure

## Web (Nextjs)
```
app/                 # Next.js App Router — routing only (thin re-exports)
├── layout.tsx       # Root layout (imports @/app/providers, @/app/globals.css)
├── page.tsx         # import { DashboardPage } from '@/views/dashboard'
└── some-page/
    └── page.tsx     # Thin: import page from @/views/, render it

src/                 # All FSD layers
├── app/             # FSD app-layer: providers, global styles (NO routing files)
│   ├── globals.css
│   └── providers/
├── views/           # FSD pages layer (named "views" to avoid Next.js pages/ conflict)
│   └── dashboard/   # Compose widgets into full page layouts
│       └── ui/
├── widgets/         # Sections/blocks (Header, Sidebar, StatsCards, RecentRuns)
├── features/        # User interactions (auth, send-comment, add-to-cart)
│   └── auth/
│       ├── ui/
│       ├── model/
│       ├── api/
│       └── actions/   # Server Actions
├── entities/        # Business entities (user, product, order)
│   └── user/
│       ├── ui/
│       ├── model/
│       └── api/
└── shared/          # Reusable infrastructure
    ├── ui/          # Design system
    ├── lib/         # Utilities, helpers
    ├── api/         # API client
    └── config/      # Environment, constants
```

- Root `app/` is for Next.js routing only. `src/` holds all FSD layers.
- `src/app/` is the FSD app-layer (providers, global styles), NOT routing.

## Mobile (Expo)
```
app/                 # Expo Router (file-based routing)
├── _layout.tsx      # Root layout
├── index.tsx        # Home (/)
└── some-page/
    └── index.tsx    # /some-page (routing + page composition)
src/
├── app/             # App-wide settings, providers, global styles
│   └── providers/
├── widgets/         # Large composite blocks (Header, Sidebar, Feed)
├── features/        # User interactions (auth, send-comment, add-to-cart)
│   └── auth/
│       ├── ui/
│       ├── model/
│       └── api/
├── entities/        # Business entities (user, product, order)
│   └── user/
│       ├── ui/
│       ├── model/
│       └── api/
└── shared/          # Reusable infrastructure
    ├── ui/          # Design system
    ├── lib/         # Utilities, helpers
    ├── api/         # API client
    └── config/      # Environment, constants
```
