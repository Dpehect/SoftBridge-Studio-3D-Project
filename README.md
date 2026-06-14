# SoftBridge Cafe - Interactive 3D Web Experience
Live at https://soft-bridge-studio-3-d-project.vercel.app/
## Project Purpose
SoftBridge Cafe is a high-performance, immersive 3D web application designed as an interactive exploration of a virtual café space. The project serves to demonstrate advanced front-end integration of declarative 3D graphics in modern web frameworks, focusing on web-optimized asset delivery, timeline-based animation, spatial sound design, and smooth user interactions on both desktop and mobile platforms.

---

## Technical Stack and Architecture

### 1. Framework and Core Runtime
* **SvelteKit (v2) and Svelte (v5):** Built on Svelte's reactive component model, utilizing Svelte 5 runes (`$state`, `$derived`, `$effect`) for state management across the canvas and UI overlays.
* **Vite (v6):** Powering the build pipeline and dev server configuration, featuring hot module reloading (HMR) for both Svelte components and Svelte-Three.js assets.

### 2. 3D Engine and Canvas Rendering
* **Three.js:** The underlying WebGL library managing the 3D scene graph, cameras, lights, geometries, and materials.
* **Threlte (v8):** A declarative 3D framework for Svelte, structuring Three.js objects as reactive components. It coordinates the render loop on-demand, optimizing frame rates and reducing CPU/GPU overhead.
* **WebGLRenderer:** Configured with antialiasing disabled and alpha channel transparency enabled to overlay the 3D canvas seamlessly with Svelte UI components.

### 3. Asset Pipeline and Compression
To ensure rapid loading times and low memory footprint on mobile devices, the application uses an optimized asset pipeline:
* **Draco Compression:** Compresses mesh geometry data to minimize GLTF/GLB download sizes.
* **Meshopt Simplifier:** Compresses geometry indices and vertex data for efficient GPU decoding.
* **KTX2 Textures:** Uses GPU-compressed texture formats (ETC1S / UASTC) via Basis Universal, allowing direct execution in GPU memory without runtime decompression overhead.

### 4. Animation and Scene Staging
* **Theatre.js:** Utilized alongside `@threlte/theatre` to choreograph complex animations. It defines camera paths, object translations, and lighting states via a timeline configuration loaded from `configState.json`.
* **Svelte Motion:** Handles smooth camera transitions and UI fade-ins using spring and tweened animation modules.

### 5. Sound Design and Web Audio
* **Howler.js:** Powering the application's audio pipeline. It manages background music (BGM) tracks and handles visiblity-state events to pause/resume audio playback when the browser tab is out of focus.
* **MediaSession API:** Connects the active audio stream to system-level media overlays, registering metadata and playback states.

### 6. Post-Processing and Shaders
* **Subpixel Morphological Antialiasing (SMAA):** Applied via `threlte-postprocessing` to smooth jagged edges of 3D geometry without the performance penalty of multi-sample antialiasing (MSAA).
* **Depth of Field (DoF):** Generates a realistic camera lens effect, focusing on selected interactable objects while softening the background.

### 7. UI and Styling
* **Tailwind CSS (v4):** Used for layout styling of HUD controls, loading screens, dialog popups, and the info panel.
* **Responsive Layouts:** Designed to adapt dynamically to varied viewport ratios, using CSS Custom Properties and media queries to resize interaction panels and text overlays.

---

## Vercel Readiness
The application is pre-configured for Vercel deployment:
* **Adapter Auto:** Uses `@sveltejs/adapter-auto` in `svelte.config.js` to automatically detect the Vercel hosting environment during deployment and adapt the build to Vercel's edge network structure.
* **Asset Caching:** Configured to compile static assets (images, 3D glb models, compressed textures) into optimized distribution directories, ensuring fast global CDN delivery.
