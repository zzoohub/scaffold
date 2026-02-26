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


## Web 3D / WebXR Project Scaffold (TanStack Start)

### Base Structure

src/
├── routes/                  # TanStack Start file-based routing
│   ├── __root.tsx           # Root layout (html shell, providers, global UI)
│   ├── index.tsx
│   └── ...
├── router.tsx               # Router config (getRouter export)
├── routeTree.gen.ts         # Auto-generated (do not edit)
│
├── scene/                   # 3D world (R3F components)
│   ├── canvas.tsx           # Canvas wrapper + defaults
│   ├── objects/             # Reusable 3D objects
│   ├── environments/        # Lighting, skybox, post-processing
│   ├── cameras/             # Camera rigs & controllers
│   ├── materials/           # Custom materials / shaders
│   └── helpers/             # Debug visuals, gizmos, grid
│
├── xr/                      # WebXR (omit if not needed)
│   ├── session.tsx          # XR session management
│   ├── controllers/         # Hand / controller mapping
│   ├── interactions/        # Grab, teleport, gaze, poke
│   └── spaces/              # XR-specific spatial layouts
│
├── ui/                      # 2D interface
│   ├── design-system/
│   ├── components/
│   ├── hud/                 # Overlay on top of 3D
│   ├── panels/              # Side panels, inspectors
│   └── layout/
│
└── shared/
    ├── types/
    ├── constants/
    ├── hooks/
    ├── utils/
    └── assets/              # glTF, textures, audio
        ├── models/
        ├── textures/
        └── loaders.ts


### Extensions (add only when triggered)

+ engine/                        ← complex frame loop; needs React-independent execution
│   ├── ecs/                     ← hundreds of homogeneous entities
│   │   ├── components/          # Pure data definitions
│   │   ├── systems/             # Pure logic (stateless)
│   │   ├── queries/
│   │   ├── prefabs/             # Entity templates (composition)
│   │   └── world.ts
│   ├── ports/                   ← library swap likely
│   ├── adapters/
│   ├── physics/
│   └── shaders/                 # Custom TSL / GLSL
│
+ domains/                       ← 2+ independent scenes/modes
│   └── [domain-name]/
│       ├── use-cases/           # Scenario logic (pure functions)
│       ├── Scene.tsx            # Composes engine + scene + ui
│       ├── config.ts
│       └── ui/                  # Domain-specific UI
│
+ networking/                    ← multiplayer or real-time sync
│   ├── client.ts
│   ├── state-sync.ts
│   ├── interpolation.ts
│   └── authority.ts
│
+ content/                       ← external data injected into 3D scene
│   └── (project-specific)       # Structure depends on domain
│
+ workers/                       ← heavy computation offload
    ├── compute-worker.ts
    └── wasm/


### Skip Conditions

| Layer        | Skip if                                      |
|--------------|----------------------------------------------|
| engine/      | static scenes or simple interactions         |
| engine/ecs/  | < 10 unique objects with distinct behavior   |
| engine/ports/| locked-in dependencies                       |
| domains/     | single unified scene                         |
| networking/  | single-user only                             |
| content/     | no external data source; pure 3D experience  |
| workers/     | all logic runs fine on main thread            |


### Dependency Direction

routes/ → domains → engine   (React-free, pure logic)
            ↓
          scene              (R3F components)
            ↓
           ui                (2D overlay)
            ↓
         shared              (referenced by all)

xr         → scene
networking → engine
workers    → engine


### Invariants

1. Public API via index.ts barrel only — internal files never imported from outside
2. No cross-import within same layer — shared logic moves to engine/ or shared/
3. engine/ never imports React


### Presets

3D Landing Page         → routes + scene + ui + shared
VR Product Showroom     → + xr + content
Multiplayer VR Social   → + xr + engine + networking + domains
Physics Simulation      → + xr + engine(full) + domains + content + workers
