# Volume of Revolution - 3D Calculus Visualization

## Overview

This is an interactive 3D educational application that visualizes volumes of revolution in calculus. The application allows users to explore how 2D curves create 3D solids when rotated around an axis, demonstrating both the disk method (rotation around x-axis) and shell method (rotation around y-axis). Built with React and Three.js, it provides real-time 3D rendering with mathematical formulas and calculated volumes for various geometric shapes including semicircles, parabolas, triangles, and custom 3D blocks.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework & Rendering:**
- React 18 with TypeScript for UI components
- Three.js ecosystem (@react-three/fiber, @react-three/drei, @react-three/postprocessing) for 3D graphics
- Vite as the build tool and development server with hot module replacement (HMR)
- GLSL shader support via vite-plugin-glsl for custom graphics effects

**UI Component System:**
- Radix UI primitives for accessible, unstyled components (dialogs, selects, sliders, switches, etc.)
- Tailwind CSS for styling with a custom dark theme configuration
- shadcn/ui component patterns for consistent design system
- Responsive design with mobile-specific layouts using custom useIsMobile hook

**State Management:**
- Zustand for lightweight state management
- Three separate stores:
  - `useRevolution`: Manages shape selection, rotation axis, animation state, and cross-section display
  - `useGame`: Handles game phases (ready/playing/ended) - appears to be legacy code from template
  - `useAudio`: Controls audio playback and muting - appears to be legacy code from template

**3D Scene Architecture:**
- Canvas component wraps the entire 3D scene
- OrbitControls for camera manipulation (drag to rotate, scroll to zoom, pinch on mobile)
- Separate components for modular 3D elements:
  - `CoordinateAxes`: Visual xyz-axis guides with labeled arrows
  - `Curve2D`: Renders the original 2D curve with filled region
  - `RevolutionSolid`: Animated 3D solid generation using lathe geometry
  - `CrossSection`: Optional disk/ring visualization showing volume element
- Grid system for spatial reference
- Multiple light sources (ambient, directional, point) for proper 3D visualization

### Backend Architecture

**Server Framework:**
- Express.js HTTP server
- Custom logging middleware for API requests
- Static file serving for production builds
- Vite middleware integration for development with HMR

**Development vs Production:**
- Development: Vite dev server with middleware mode for seamless HMR
- Production: Pre-built static files served via Express
- Build process bundles both client (Vite) and server (esbuild) separately

**Storage Layer:**
- Abstracted storage interface (`IStorage`) for CRUD operations
- In-memory storage implementation (`MemStorage`) as default
- User schema defined but currently unused (appears to be template boilerplate)
- Ready for database integration via Drizzle ORM

**Build System:**
- Custom build script that:
  - Cleans dist directory
  - Builds client with Vite (outputs to dist/public)
  - Bundles server with esbuild (outputs to dist/index.cjs)
  - Selectively bundles server dependencies to reduce syscalls and improve cold start times
  - Externalizes dependencies not in allowlist

### Data Flow & Interaction

**User Interaction Flow:**
1. User selects shape from dropdown (Control Panel)
2. User chooses rotation axis (x or y)
3. Animation plays showing gradual rotation (0° to 360°)
4. Cross-section visualization can be toggled and positioned
5. Formulas and calculated volumes update in real-time

**Mathematical Calculations:**
- Shape definitions include pre-computed formulas and volume expressions
- Sample points generated procedurally for curve rendering
- Lathe geometry construction for revolution solids
- Support for both standard functions and custom 3D block geometries (washers/shells)

**Responsive Behavior:**
- Mobile: Consolidated bottom panel with vertical layout
- Desktop: Side-by-side panels (controls left, formulas right)
- Touch-optimized with manipulation gestures disabled on UI elements
- Overscroll prevention for immersive 3D experience

## External Dependencies

**3D Graphics & Visualization:**
- `three` (via @react-three/fiber): Core 3D rendering engine
- `@react-three/drei`: Helper components (Text, Line, Grid, OrbitControls, Environment)
- `@react-three/postprocessing`: Post-processing effects for enhanced visuals
- `vite-plugin-glsl`: GLSL shader compilation support

**Database & ORM:**
- `@neondatabase/serverless`: Neon Postgres serverless driver (configured but not actively used)
- `drizzle-orm`: TypeScript ORM for database operations
- `drizzle-kit`: Database migration and schema management tool
- PostgreSQL dialect configured via drizzle.config.ts

**UI Component Libraries:**
- `@radix-ui/*`: Complete suite of accessible UI primitives (20+ components)
- `class-variance-authority`: Type-safe variant styling
- `tailwind-merge` & `clsx`: Utility class management
- `cmdk`: Command palette component
- `lucide-react`: Icon library

**State & Data Management:**
- `zustand`: Lightweight state management
- `@tanstack/react-query`: Server state management and caching (configured but appears unused in current implementation)

**Styling & Theming:**
- `tailwindcss`: Utility-first CSS framework
- `autoprefixer`: CSS vendor prefixing
- `@fontsource/inter`: Custom font loading

**Development Tools:**
- `@replit/vite-plugin-runtime-error-modal`: Enhanced error display during development
- TypeScript with strict mode enabled
- Path aliases configured for clean imports (@/, @shared/)

**Build & Server:**
- `vite`: Frontend build tool and dev server
- `esbuild`: Server-side bundling
- `express`: HTTP server framework
- `tsx`: TypeScript execution for development

**Legacy/Unused Dependencies:**
- Session management packages (connect-pg-simple, express-session, memorystore)
- Authentication (passport, passport-local, jsonwebtoken)
- Various utilities (nanoid, uuid, date-fns, multer)
- Third-party integrations (stripe, nodemailer, openai, @google/generative-ai)
- File processing (xlsx)
- WebSocket (ws)

Note: The application appears to be built from a full-stack template with authentication and database capabilities, but the current implementation focuses solely on the 3D visualization feature. Many dependencies are unused and could be removed for a cleaner production build.