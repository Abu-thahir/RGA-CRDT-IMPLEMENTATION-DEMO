# RGA CRDT Implementation Demo

Interactive web demonstrations of **Replicated Growable Array (RGA)** Conflict-free Replicated Data Types for real-time collaborative editing. Experience how multiple users can edit simultaneously without conflicts, with automatic convergence and full offline support.

## 🎯 Live Demos

Try the live demos deployed on Render:

- **[Text Editor Demo](https://rga-crdt-implementation-demo.onrender.com/)** - Character-level collaborative text editing with visual algorithm inspection
- **[Block Editor Demo](https://rga-crdt-implementation-block-editor-demo.onrender.com/)** - Rich block-based editor with paragraphs, lists, and tables

> **Note**: Free tier services may take 30-60 seconds to wake up on first visit.

## ✨ Features

### Text Editor Demo
- **Real-time Collaboration**: Multiple users edit simultaneously with instant synchronization
- **Deterministic Playground**: Scripted scenarios with step-by-step execution
- **Visual Inspector**: 
  - RGA tree structure visualization
  - Linked-list view of character sequences
  - Operation timeline with readable/JSON toggle
- **Conflict Resolution Scenarios**: 
  - Sequential inserts
  - Concurrent appends
  - Concurrent delete + insert
  - Out-of-order delivery
  - Offline editing and merge
- **Live Replica Simulation**: Each browser tab becomes an independent replica
- **Presence Tracking**: See who's online/offline with unique replica IDs

### Block Editor Demo
- **Rich Block Structure**: Paragraphs, lists (bullet/ordered), and tables
- **Real-time Collaboration**: Multiple users edit simultaneously
- **Drag & Drop**: Reorder blocks by dragging the six-dot handle
- **Offline Support**: Continue editing when disconnected, auto-sync on reconnect
- **Algorithm Inspector**:
  - Operation history with conflict detection
  - RGA tree visualization
  - Document state tracking
- **User Presence**: Color-coded replica badges showing online/offline status
- **Conflict-Free Guarantees**: All replicas converge to identical state

## 🏗️ Architecture

### System Overview

```
┌─────────────┐         WebSocket (wss://)      ┌─────────────┐
│  Browser 1  │◄──────────────────────────────►│             │
│  (Replica)  │                                 │   Server    │
└─────────────┘                                 │  (Node.js)  │
                                                │             │
┌─────────────┐         WebSocket (wss://)      │   + OpLog   │
│  Browser 2  │◄──────────────────────────────►│   + Relay   │
│  (Replica)  │                                 │             │
└─────────────┘                                 └─────────────┘
```

### CRDT Properties

- **Strong Eventual Consistency**: All replicas converge to the same state
- **Commutativity**: Operations can be applied in any order
- **Idempotence**: Duplicate operations have no additional effect
- **Deterministic Conflict Resolution**: Concurrent edits ordered by replica ID

### Data Flow

```
User Edit → Generate CRDT Op → Apply Locally → Send to Server → Broadcast → Apply on All Replicas
```

## 🚀 Local Development

### Prerequisites

- Node.js 20+
- npm

### Setup

1. Clone the repository with submodules:
```bash
git clone --recursive https://github.com/Abu-thahir/RGA-CRDT-IMPLEMENTATION-DEMO.git
cd RGA-CRDT-IMPLEMENTATION-DEMO
```

2. Run the text editor demo:
```bash
cd RGA-CRDT-IMPLEMENTATION-DEMO/crdt-web
npm install
npm run dev
```
Open http://localhost:8080 in multiple browser tabs to test collaboration.

3. Run the block editor demo (in a new terminal):
```bash
cd RGA-CRDT-IMPLEMENTATION-DEMO/crdt-block-web-ui
npm install
npm run dev
```
Open http://localhost:8081 in multiple browser tabs to test collaboration.

## 📖 How to Use

### Text Editor

**Deterministic Mode (Scenarios Tab)**:
1. Select a scenario from the list (e.g., "Concurrent Appends")
2. Click "Step" to execute operations one at a time
3. Click "Run" to execute all operations automatically
4. Watch the RGA tree and operation log update in real-time
5. Use the Inspector to examine replica state

**Real-time Mode (Real-time Collaboration Tab)**:
1. Open the demo in multiple browser tabs
2. Each tab gets a unique replica ID (R1, R2, R3, etc.)
3. Type in any tab and watch changes sync instantly
4. Click "Go Offline" to simulate network disconnection
5. Make edits while offline, then "Go Online" to sync

### Block Editor

**Adding Content**:
- Click "Add Paragraph" and start typing
- Click "Add List" for bullet points, "+ Add item" for more items
- Click "Add Table" for a 2x2 table, use "+ Row" and "+ Column" to expand

**Editing**:
- Click any block to edit its content
- Press Enter in a paragraph to create a new one below
- Press Enter in a list item to add a new item
- Hover over blocks to see the delete button (×)

**Reordering**:
- Hover over any block to see the six-dot drag handle
- Click and drag to reorder blocks

**Collaboration**:
- Open the same URL in multiple tabs
- Each user gets a unique replica ID and color badge
- Changes sync instantly across all connected clients
- Test offline editing with the "Go Offline" button

## 🔧 Technical Details

### Project Structure

```
├── crdt-implementation/           (git submodule - core CRDT packages)
│   └── packages/
│       ├── text-crdt/            (Character-level RGA CRDT)
│       │   ├── identifier.mts    (Unique ID logic)
│       │   ├── operation.mts     (Insert/Delete operations)
│       │   ├── rgaDocument.mts   (RGA tree structure)
│       │   └── rgaReplica.mts    (Replica + clock management)
│       └── block-crdt/           (Block-level RGA CRDT)
│           ├── block.mjs         (Block types and operations)
│           ├── crdtDocument.mjs  (Document structure)
│           └── rga.mjs           (RGA implementation)
├── RGA-CRDT-IMPLEMENTATION-DEMO/
│   ├── crdt-web/                 (Text editor UI)
│   │   ├── src/
│   │   │   ├── server.mts        (WebSocket server + relay)
│   │   │   └── client.mts        (UI + CRDT logic)
│   │   └── public/
│   │       └── index.html        (UI markup and styles)
│   └── crdt-block-web-ui/        (Block editor UI)
│       ├── src/
│       │   ├── server.mts        (WebSocket server)
│       │   └── client.mts        (Frontend application)
│       └── public/
│           ├── index.html        (HTML template)
│           └── styles.css        (Styling)
└── README.md
```

### Operation Types

**Text CRDT**:
- `InsertOp`: Add a character after a specific identifier
- `DeleteOp`: Mark a character as deleted (tombstone)

**Block CRDT**:
- `insert_block`: Add a new block (paragraph, list, table)
- `delete_block`: Remove a block
- `insert_char`: Add a character to text
- `delete_char`: Remove a character
- `insert_list_item`: Add item to list
- `insert_row` / `insert_column`: Expand table

### Conflict Resolution

When two users edit the same position:
- Operations are ordered deterministically by `(counter, replicaId)`
- Both edits are preserved in a consistent order across all replicas
- No data loss, guaranteed convergence
- Example: If R1 inserts "A" and R2 inserts "B" at the same position, all replicas converge to "AB" (R1 < R2)

## 🧪 Testing Scenarios

### Basic Collaboration Test
1. Open demo in two browser tabs
2. Type "Hello" in Tab 1
3. Verify "Hello" appears in Tab 2
4. Type " World" in Tab 2
5. Both tabs show "Hello World"

### Offline Editing Test
1. Click "Go Offline" in Tab 1
2. Make edits in Tab 1 (queued locally)
3. Make different edits in Tab 2 (synced to server)
4. Click "Go Online" in Tab 1
5. Watch automatic merge and convergence

### Concurrent Editing Test
1. Open demo in three tabs
2. Type simultaneously in all tabs
3. Observe real-time synchronization
4. All tabs converge to the same final state

## 🛠️ Technology Stack

- **CRDT Implementation**: TypeScript
- **Build Tool**: esbuild
- **WebSocket**: ws library
- **Server**: Node.js
- **Deployment**: Render.com

## 📚 Learn More

### CRDT Resources
- [CRDT Tech](https://crdt.tech/) - Overview and resources
- [RGA Paper](https://pages.lip6.fr/Marc.Shapiro/papers/RGA-TPDS-2011.pdf) - Original algorithm
- [Conflict-free Replicated Data Types](https://arxiv.org/abs/1805.06358) - Academic survey

### Related Projects
- [Yjs](https://github.com/yjs/yjs) - Production CRDT library
- [Automerge](https://github.com/automerge/automerge) - JSON CRDT
- [ProseMirror](https://prosemirror.net/) - Rich text editor framework

## 🎓 Educational Purpose

This project is designed as a **learning tool** for understanding:
- How sequence CRDTs work
- How concurrent text edits converge without conflicts
- How tombstones and deterministic ordering enable consistency
- Real-time collaborative editing architecture

This is **not** a production-ready editor but focuses on correctness, clarity, and approachability.

## ⚠️ Known Limitations

- Text editing uses simple diff algorithm (can be optimized)
- No undo/redo support
- No rich text formatting (bold, italic, etc.)
- No cursor position sync between users
- Tombstones not garbage collected (memory grows with edits)
- No persistence (in-memory only)

## 🚧 Future Enhancements

- [ ] Undo/redo support
- [ ] Rich text formatting
- [ ] Cursor/selection indicators for other users
- [ ] Better text diff algorithm
- [ ] Persistence (database storage)
- [ ] User authentication
- [ ] Export to Markdown/HTML
- [ ] Version history
- [ ] Tombstone garbage collection

## 🐛 Troubleshooting

### Demo Not Loading
- Free tier services sleep after inactivity
- Wait 30-60 seconds for the service to wake up
- Refresh the page if needed

### Changes Not Syncing
1. Check "Online" status indicator
2. Open browser DevTools (F12) → Console for errors
3. Check Network tab → WS filter for WebSocket connection
4. Try refreshing both tabs

### Local Development Issues
```bash
# If port 8080 is in use, edit src/server.mts:
const port = 3000; // Change to any available port

# If submodule is empty:
git submodule update --init --recursive

# If build fails:
node --version  # Ensure Node.js 20+
npm install     # Reinstall dependencies
```

## 👥 Credits

- **Core CRDT Implementation**: [ashwinhprasad](https://github.com/ashwinhprasad/RGA-CRDT)
- **Demo Implementation**: [Abu-thahir](https://github.com/Abu-thahir)

## 📄 License

MIT - Use freely in your projects!

---

**Ready to explore CRDTs?** Try the [live demos](https://rga-crdt-implementation-demo.onrender.com/) or run locally with `npm run dev`! 🎉
