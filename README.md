# RGA CRDT Implementation Demo

Live demonstrations of Replicated Growable Array (RGA) Conflict-free Replicated Data Types for collaborative editing.

## Project Structure

```
├── crdt-implementation/    (git submodule → core CRDT packages)
│   └── packages/
│       ├── text-crdt/      (RGA text CRDT implementation)
│       └── block-crdt/     (RGA block CRDT implementation)
├── RGA-CRDT-IMPLEMENTATION-DEMO/
│   ├── crdt-web/           (Text editor UI demo)
│   └── crdt-block-web-ui/  (Block editor UI demo)
├── index.html              (Landing page)
└── README.md
```

## Live Demos

Visit the [GitHub Pages deployment](#) to try the demos:

- **Text Editor**: Collaborative text editing with multiple replicas
- **Block Editor**: Rich block-based editor with paragraphs, lists, and tables

## Local Development

### Prerequisites

- Node.js 20+
- npm

### Setup

1. Clone the repository with submodules:
```bash
git clone --recursive <your-repo-url>
cd CRDT_DEMO
```

2. Run the text editor demo:
```bash
cd RGA-CRDT-IMPLEMENTATION-DEMO/crdt-web
npm install
npm run dev
```

Open http://localhost:8080 in multiple browser windows to test collaboration.

3. Run the block editor demo:
```bash
cd RGA-CRDT-IMPLEMENTATION-DEMO/crdt-block-web-ui
npm install
npm run dev
```

Open http://localhost:8080 in multiple browser windows to test collaboration.

## Features

### Text Editor Demo
- Real-time collaborative text editing
- Multiple replica simulation
- Operation log visualization
- RGA tree structure inspector
- Conflict resolution scenarios

### Block Editor Demo
- Rich block-based editing (paragraphs, lists, tables)
- Real-time collaboration
- Drag-and-drop block reordering
- Algorithm inspector with operation history
- Conflict detection and resolution visualization

## Technology Stack

- **CRDT Implementation**: TypeScript
- **Build Tool**: esbuild
- **WebSocket**: ws library
- **Deployment**: GitHub Pages + GitHub Actions

## Credits

Core CRDT implementation by [ashwinhprasad](https://github.com/ashwinhprasad/RGA-CRDT)
