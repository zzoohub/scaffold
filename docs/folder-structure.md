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


## Web 3D / WebXR Application Scaffold

A progressive folder structure for R3F + WebXR projects. Start with the **Base** and add **Extension Layers** only when complexity demands it.

---

### Base Structure

```
src/
├── app/                  # Entry point, routing, providers
│   ├── main.tsx
│   ├── router.tsx
│   ├── providers.tsx
│   └── routes/
│
├── scene/                # 3D world (R3F components)
│   ├── canvas.tsx        # Canvas wrapper + defaults
│   ├── objects/          # Reusable 3D objects
│   ├── environments/     # Lighting, skybox, post-processing
│   ├── cameras/          # Camera rigs & controllers
│   ├── materials/        # Custom materials / shaders
│   └── helpers/          # Debug visuals, gizmos, grid
│
├── xr/                   # WebXR (omit if not needed)
│   ├── session.tsx       # XR session management
│   ├── controllers/      # Hand / controller mapping
│   ├── interactions/     # Grab, teleport, gaze, poke
│   └── spaces/           # XR-specific spatial layouts
│
├── ui/                   # 2D interface
│   ├── components/       # General UI components
│   ├── hud/              # Overlay on top of 3D
│   ├── panels/           # Side panels, inspectors
│   └── layout/           # Page layouts
│
└── shared/               # Referenced by all layers
    ├── types/
    ├── constants/
    ├── hooks/
    ├── utils/
    └── assets/           # glTF, textures, audio
        ├── models/
        ├── textures/
        └── loaders.ts
```

---

### Extension Layers

Add **only** when the corresponding complexity emerges.

```
src/
├── ... (base) ...
│
├── engine/               # 🔴 When: simulation loop exists
│   ├── ecs/              #    ECS pattern (e.g., Koota, Bitecs)
│   │   ├── components/   #    Pure data definitions
│   │   ├── systems/      #    Pure logic (stateless)
│   │   ├── queries/      #    Pre-defined queries
│   │   ├── prefabs/      #    Entity templates (composition)
│   │   └── world.ts
│   ├── ports/            #    Interfaces (dependency inversion)
│   ├── adapters/         #    Concrete implementations
│   ├── physics/          #    Physics engine wrapper
│   └── shaders/          #    Custom TSL / GLSL
│
├── domains/              # 🟡 When: 2+ independent scenarios
│   └── [domain-name]/
│       ├── use-cases/    #    Scenario logic (pure functions)
│       ├── Scene.tsx     #    Composes engine + scene + ui
│       ├── config.ts
│       └── ui/           #    Domain-specific UI
│
├── networking/           # 🔵 When: multiplayer / real-time sync
│   ├── client.ts
│   ├── state-sync.ts
│   ├── interpolation.ts
│   └── authority.ts
│
├── content/              # 🟢 When: CMS / education / media
│   ├── lessons/
│   ├── assessments/
│   └── media/
│
└── workers/              # 🟠 When: heavy computation offload
    ├── compute-worker.ts
    └── wasm/
```

#### When to Add Each Layer

| Layer | Trigger | Skip if |
|---|---|---|
| `engine/` | Frame-loop logic is complex; needs React-independent execution | Static scenes or simple interactions |
| `engine/ports/` | Library replacement is likely (e.g., Rapier → custom GPU physics) | Locked-in dependencies |
| `engine/ecs/` | Hundreds of homogeneous entities; system composition is key | < 10 unique objects with distinct behavior |
| `domains/` | 2+ independent scenes/modes exist | Single unified scene |
| `networking/` | Multiplayer or real-time collaboration | Single-user only |
| `content/` | Educational, CMS-driven, or media-heavy features | Pure 3D experience |
| `workers/` | WASM modules or CPU-heavy computation | All logic runs fine on main thread |

---

### Dependency Rules

```
app → domains → engine   (React-free, pure logic)
        ↓
      scene              (R3F components)
        ↓
       ui                (2D overlay)
        ↓
     shared              (referenced by all)

xr         → scene       (extends 3D layer)
networking → engine      (state synchronization)
workers    → engine      (offloaded computation)
```

#### Invariants (apply at every scale)

1. **Public API barrel** — Each module exposes only through `index.ts`. Internal files are never imported directly from outside.

```ts
// scene/objects/index.ts
export { Sphere } from './Sphere'
export { Spring } from './Spring'
// internal helpers stay hidden
```

2. **No cross-imports within the same layer**

```ts
import { X } from '@/domains/other-domain'   // ❌ forbidden
import { Y } from '@/scene/objects'           // ✅ different layer
```

If two domains need shared logic → move it down to `engine/` or `shared/`.

3. **engine/ never imports React** — Keeps simulation testable and framework-portable.

---

### Patterns Borrowed from Established Architectures

| Source | Pattern | Where Applied |
|---|---|---|
| **FSD** | Public API (barrel exports) | All modules |
| **FSD** | Cross-import prohibition | Between `domains/` |
| **Hexagonal** | Ports & Adapters | `engine/ports/`, `engine/adapters/` |
| **Clean Arch** | Use Cases | `domains/*/use-cases/` |
| **Clean Arch** | Dependency Inversion | `engine/` knows nothing about React |
| **ECS** | Component / System separation | `engine/ecs/` |
| **ECS** | Prefabs (composition templates) | `engine/ecs/prefabs/` |
| **ECS** | System scheduling | `engine/ecs/schedule.ts` |

---

### Project Type Examples

#### 3D Landing Page
```
app/ + scene/ + ui/ + shared/
```

#### VR Product Showroom
```
app/ + scene/ + xr/ + ui/ + shared/ + content/
```

#### Multiplayer VR Social
```
app/ + scene/ + xr/ + ui/ + shared/ + engine/ + networking/ + domains/
```

#### Physics Simulation Platform
```
app/ + scene/ + xr/ + ui/ + shared/ + engine/ (full) + domains/ + content/ + workers/
```

---

> **Core philosophy:** Start with 5 base folders. Add one extension layer at a time as complexity grows. Never pre-create empty layers.
