# Comprehensive Electron App Development Reference Manual

**A detailed, app-focused guide to Electron development, covering process architecture, desktop UI patterns, IPC, native integration, security, packaging, updates, performance, and production practices that matter when building real cross-platform desktop applications with web technologies.**

---

## Table of Contents
1. [Introduction to Electron App Development](#1-introduction-to-electron-app-development)
2. [Electron Development Environment](#2-electron-development-environment)
3. [Project Structure in an Electron App](#3-project-structure-in-an-electron-app)
4. [Electron Architecture Basics](#4-electron-architecture-basics)
5. [Main Process, Renderer Process, and Preload Responsibilities](#5-main-process-renderer-process-and-preload-responsibilities)
6. [IPC and Cross-process Communication](#6-ipc-and-cross-process-communication)
7. [Windows, Menus, Tray, and Native UX](#7-windows-menus-tray-and-native-ux)
8. [Frontend App Architecture](#8-frontend-app-architecture)
9. [Filesystem, Local Data, and OS Integration](#9-filesystem-local-data-and-os-integration)
10. [Networking and External Services](#10-networking-and-external-services)
11. [Security Hardening](#11-security-hardening)
12. [Performance and Resource Management](#12-performance-and-resource-management)
13. [Background Tasks and Long-running Work](#13-background-tasks-and-long-running-work)
14. [Testing Electron Applications](#14-testing-electron-applications)
15. [Debugging and Diagnostics](#15-debugging-and-diagnostics)
16. [Packaging, Signing, and Distribution](#16-packaging-signing-and-distribution)
17. [Updates and Maintenance](#17-updates-and-maintenance)
18. [Best Practices](#18-best-practices)
19. [Common Mistakes](#19-common-mistakes)
20. [Real-world Examples](#20-real-world-examples)

---

## 1. Introduction to Electron App Development

### What Electron Is
Electron is a framework for building cross-platform desktop applications using:
- web technologies such as HTML, CSS, and JavaScript or TypeScript
- Chromium for rendering
- Node.js for native-capable JavaScript execution

### Why Teams Choose Electron
Electron is widely used because it offers:
- strong web ecosystem compatibility
- cross-platform desktop support
- mature packaging and update tooling
- powerful native integration
- fast team adoption for web developers

### Where Electron Fits
Electron is commonly used for:
- productivity tools
- collaboration apps
- editors
- internal business software
- desktop wrappers around web-powered products

### Main Tradeoff
Electron is productive and flexible, but teams must manage:
- memory footprint
- process boundaries
- desktop packaging complexity
- security hardening

---

## 2. Electron Development Environment

### Main Tooling Pieces
A typical Electron setup includes:
- Node.js
- package manager
- Electron runtime
- frontend build tooling
- packager or builder tooling

### Frontend Tooling
Many Electron apps use:
- React
- Vue
- Svelte
- Vite or Webpack-based bundling
- TypeScript

### Typical Workflow
1. run frontend dev server or bundle watcher
2. launch Electron app shell
3. iterate on renderer and main process logic
4. test platform behaviors

### Cross-platform Development Awareness
Desktop apps must account for:
- Windows path and installer behavior
- macOS signing and app bundle expectations
- Linux desktop environment differences

---

## 3. Project Structure in an Electron App

### Typical Layout

```text
my-electron-app/
  src/
    main/
    preload/
    renderer/
  assets/
  package.json
  electron-builder.yml
```

### Main Process Code
Usually contains:
- app bootstrap
- window creation
- menus
- tray integration
- filesystem or OS integration
- IPC handlers

### Preload Code
Usually contains:
- carefully exposed safe APIs
- bridge between renderer and privileged functionality

### Renderer Code
Usually contains:
- UI framework code
- routes
- components
- frontend state

### Clear Boundaries Matter
Good Electron apps avoid mixing all responsibilities together.

---

## 4. Electron Architecture Basics

### Main Process
The main process controls:
- application lifecycle
- window creation
- native integrations
- privileged APIs

### Renderer Process
The renderer process handles:
- UI rendering
- interaction logic
- screen state
- calling into safe bridged APIs

### Preload Script
The preload layer is critical for security-conscious Electron architecture. It lets you expose a narrow, intentional surface to the renderer.

### Why This Separation Matters
Without clear process boundaries, Electron apps become:
- insecure
- hard to reason about
- difficult to test
- too dependent on unrestricted renderer access

---

## 5. Main Process, Renderer Process, and Preload Responsibilities

### Main Process Responsibilities
Keep in the main process:
- app lifecycle management
- file dialogs
- system tray
- native menus
- shell integration
- privileged filesystem operations
- update orchestration

### Renderer Responsibilities
Keep in the renderer:
- view rendering
- form state
- user workflows
- screen-level orchestration

### Preload Responsibilities
Keep in preload:
- safe IPC wrappers
- narrowly scoped exposed methods
- serialization-safe API boundaries

### A Good Rule
The renderer should consume capabilities, not own privileged implementation details.

---

## 6. IPC and Cross-process Communication

### Why IPC Matters
Electron apps are process-based desktop apps. Cross-process communication is unavoidable.

### Common IPC Patterns
- renderer invokes main-process action
- main process returns result
- main process emits events or status updates

### Design IPC Carefully
Treat IPC as a real API.

Good IPC design includes:
- narrow method surface
- validated payloads
- explicit return models
- minimal privilege

### Example Exposure Pattern
Preload:
```ts
contextBridge.exposeInMainWorld("desktopApi", {
  loadVersion: () => ipcRenderer.invoke("app:version")
});
```

Main process:
```ts
ipcMain.handle("app:version", async () => app.getVersion());
```

### Avoid Direct Node Access in Renderer
Unrestricted renderer access to Node APIs is a major security and architecture problem in many Electron apps.

---

## 7. Windows, Menus, Tray, and Native UX

### Desktop UX Expectations
Desktop users expect:
- proper window controls
- menus
- dialogs
- drag-and-drop
- keyboard shortcuts
- clipboard support

### Window Management
Electron apps often create:
- main window
- settings window
- modal dialogs
- helper or tray windows

### Menus
Menus can expose:
- file operations
- edit actions
- view toggles
- help commands

### Tray Apps
Some Electron apps mostly live in the tray. In those apps, think carefully about:
- minimize-to-tray behavior
- quit semantics
- startup behavior
- notification flow

### Native Feel Matters
A strong Electron app should feel desktop-appropriate, not like a webpage awkwardly trapped in a window.

---

## 8. Frontend App Architecture

### Renderer Architecture Still Matters
Even though Electron gives desktop access, the renderer is still an application frontend and benefits from normal frontend discipline.

### Good Separation
- view components
- state management
- service layer
- desktop API wrappers

### Wrap Desktop APIs
The renderer should usually talk to a frontend service, not call `window.desktopApi` from every component directly.

### State Models
Use clear state for:
- idle
- loading
- loaded
- error

This matters for file and API workflows just as much as in web apps.

---

## 9. Filesystem, Local Data, and OS Integration

### Why Electron Apps Often Exist
Many Electron apps need:
- local file access
- directory scanning
- native dialogs
- OS integration
- rich local persistence

### Typical Local Data
Apps may store:
- settings
- local database files
- logs
- cached content
- exported documents

### File Dialogs and User Trust
Prefer explicit user-driven file and folder selection rather than hidden path assumptions.

### Path and Environment Differences
Cross-platform apps must handle:
- path separators
- permissions
- desktop conventions
- user data directories

---

## 10. Networking and External Services

### Common Network Use Cases
Electron apps often connect to:
- backend APIs
- collaboration platforms
- sync services
- AI or automation endpoints

### Main vs Renderer Networking
Networking can happen in either process depending on security and architecture needs. Decide intentionally.

### Practical Considerations
Think about:
- where auth tokens live
- whether a request interacts with local files
- whether background sync belongs outside visible UI flow

### Offline Support
Desktop apps benefit from:
- cache
- retry logic
- sync visibility
- local-first behavior where appropriate

---

## 11. Security Hardening

### Security Is Non-optional
Electron apps can access powerful desktop capabilities. Security hardening is essential.

### Key Security Principles
- enable context isolation
- disable unnecessary renderer privileges
- expose only a narrow preload API
- validate IPC inputs
- avoid remote code execution patterns

### Dangerous Patterns
- unrestricted `nodeIntegration`
- exposing raw `ipcRenderer`
- passing unsanitized user input to shell commands
- loading untrusted remote content carelessly

### Treat Renderer as Less Trusted
Even in your own app, renderer code should not automatically be treated as a fully privileged environment.

---

## 12. Performance and Resource Management

### Common Performance Concerns
Electron apps often struggle with:
- heavy memory use
- too many background windows
- large bundle sizes
- renderer jank
- expensive IPC chatter

### Keep Renderers Lean
Avoid:
- giant monolithic bundles
- unnecessary re-renders
- large hidden window trees
- memory-heavy retained state

### Use Process Boundaries Wisely
Electron’s architecture can help isolate responsibilities, but overusing processes or windows also increases overhead.

### Perceived Performance
Show:
- startup progress
- loading states
- incremental rendering
- background task status

These improve UX even before raw performance tuning.

---

## 13. Background Tasks and Long-running Work

### Why Background Work Matters
Desktop apps often perform:
- file imports
- indexing
- syncing
- downloads
- local processing

### Keep UI Responsive
Long tasks should not freeze the renderer. Consider whether work belongs in:
- main process
- utility process
- worker-style frontend logic

### Progress Updates
Use IPC or event-style updates when the user needs visibility into long-running operations.

### Shutdown and Cancellation
Think about:
- closing mid-task
- quitting app during sync
- replacing earlier requests with newer ones

---

## 14. Testing Electron Applications

### Strong Testing Strategy
Good Electron testing often includes:
- renderer unit/component tests
- preload API tests
- main-process logic tests
- end-to-end desktop interaction tests

### Renderer Testing
Test:
- UI rendering
- screen state
- service wrappers
- interaction flows

### Main Process Testing
Test:
- file operations
- IPC handlers
- menu or tray logic
- path handling

### End-to-end Testing
Important for:
- open/save workflows
- multi-window behavior
- desktop integration
- preload-to-main IPC correctness

---

## 15. Debugging and Diagnostics

### Debugging Surfaces
Electron debugging spans:
- renderer errors
- preload bridge issues
- main-process logic
- packaging-specific failures

### Useful Tools
- Chromium devtools
- main-process logs
- source maps
- app lifecycle logging
- packaged build diagnostics

### Common Problems
- missing preload exposure
- wrong IPC channel names
- path issues only in packaged builds
- security settings that break expected behavior

---

## 16. Packaging, Signing, and Distribution

### Production Build Work
Electron distribution often includes:
- packaging app assets
- creating installer outputs
- code signing
- platform metadata

### Platform Differences
Windows, macOS, and Linux have different:
- installer expectations
- signing requirements
- permission prompts
- update models

### Test Real Install Builds
Always test:
- packaged app startup
- file dialogs
- updater behavior
- tray/menu behavior
- local path resolution

Development mode is not enough.

---

## 17. Updates and Maintenance

### Desktop Update Expectations
Users expect:
- reliable updates
- version transparency
- minimal disruption

### Maintenance Tasks
After release:
- monitor crashes
- update Electron carefully
- review dependency security
- refine diagnostics
- reduce startup and memory regressions

### Update Discipline
Because Electron sits on Chromium and Node foundations, version updates often matter for security as well as features.

---

## 18. Best Practices

1. **Keep main, preload, and renderer responsibilities clearly separated:**
   This improves both security and maintainability.

2. **Treat IPC like a formal API boundary:**
   Validate payloads and keep exposed channels narrow.

3. **Use context isolation and safe preload exposure:**
   This is a core Electron security habit.

4. **Wrap desktop APIs behind frontend services:**
   UI components should stay focused on presentation.

5. **Design desktop-native UX intentionally:**
   Menus, shortcuts, dialogs, and tray behavior matter.

6. **Test packaged builds early and often:**
   Production behavior differs from dev mode.

7. **Keep heavy or privileged logic out of the renderer when sensible:**
   Better boundaries improve trust and control.

8. **Watch memory and window count carefully:**
   Electron flexibility can become overhead quickly.

9. **Use local data and path handling explicitly:**
   Desktop apps are path-sensitive and platform-sensitive.

10. **Update Electron and dependencies with security awareness:**
    Runtime drift has real cost.

---

## 19. Common Mistakes

1. **Enabling unsafe renderer privileges for convenience:**
   This creates long-term security risk.

2. **Letting every component talk to raw desktop APIs directly:**
   Architecture becomes messy fast.

3. **Ignoring packaged-build differences:**
   Resource paths and OS behavior often change.

4. **Treating IPC channel names and payloads informally:**
   Bugs and security issues become more likely.

5. **Blocking renderer responsiveness with large local work:**
   Desktop apps feel sluggish immediately.

6. **Creating too many hidden windows or retaining unused state:**
   Memory usage grows fast.

7. **Assuming web-only UX patterns are enough for desktop users:**
   Native expectations still matter.

8. **Using shell commands carelessly with user input:**
   This is a serious risk.

9. **Failing to narrow the preload API surface:**
   Exposed power tends to spread.

10. **Skipping security review because the app is “just internal”:**
    Internal desktop apps still handle sensitive data.

---

## 20. Real-world Examples

### Example 1: Main-process IPC Handler

```ts
ipcMain.handle("app:version", async () => {
  return app.getVersion();
});
```

Why this is good:
- narrow single-purpose channel
- easy to reason about
- simple desktop integration point

### Example 2: Safe Preload Exposure

```ts
contextBridge.exposeInMainWorld("desktopApi", {
  getVersion: () => ipcRenderer.invoke("app:version")
});
```

Why this is good:
- avoids exposing raw privileged APIs broadly
- creates a small trusted surface
- easy for renderer code to consume

### Example 3: Renderer Service Wrapper

```ts
export async function loadVersion(): Promise<string> {
  return window.desktopApi.getVersion();
}
```

Why this is good:
- components stay decoupled from bridge details
- easier to test or mock
- architecture remains cleaner

### Example 4: Explicit File Screen State

```ts
type FileScreenState =
  | { kind: "idle" }
  | { kind: "loading" }
  | { kind: "loaded"; content: string }
  | { kind: "error"; message: string };
```

Why this is good:
- explicit async UI modeling
- avoids scattered booleans
- fits well with file and desktop workflows

### Example 5: File Open IPC Contract

```ts
ipcMain.handle("file:openText", async (_event, path: string) => {
  return fs.promises.readFile(path, "utf8");
});
```

Why this is useful:
- clear division between renderer request and main-process privileged action
- appropriate for desktop file operations
- easy to wrap with validation and error handling

---
*This guide is intentionally centered on Electron app development for real desktop software built with web technologies and native platform integration. Use it as a practical reference while building secure, responsive, maintainable cross-platform desktop apps.*
