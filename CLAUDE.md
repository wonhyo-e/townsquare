# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Blood on the Clocktower Town Square** is an unofficial online tool for running Blood on the Clocktower games through Discord or other digital means. It aids storytellers and players by providing a digital grimoire, town square, and live session capabilities.

The project consists of:
- **Frontend**: Vue.js 2 application with interactive game interface
- **Backend**: Node.js WebSocket server for live sessions
- **Game Data**: JSON files containing character definitions, editions, and game mechanics

## Development Commands

### Frontend Development
```bash
# Start development server (includes image cloning from botc-icons repo)
npm run serve

# Build for production
npm run build

# Lint code
npm run lint

# Lint with CI settings (no auto-fix, max warnings = 0)
npm run lint-ci
```

### Backend Development (Live Session Server)
```bash
# Install dependencies (from project root)
npm install

# Start backend locally
cd server/
NODE_ENV=development node index.js
```

The backend runs on `localhost:8001` in development mode and requires adjusting `/src/store/socket.js` to connect to the local backend.

## Architecture

### Frontend (Vue.js 2 + Vuex)

**Store Structure:**
- **Core State**: `src/store/index.js` - Main application state, roles, editions, modals
- **Players Module**: `src/store/modules/players.js` - Player management, seating, roles
- **Session Module**: `src/store/modules/session.js` - Live session state, voting, WebSocket integration
- **Persistence**: `src/store/persistence.js` - LocalStorage integration
- **Socket**: `src/store/socket.js` - WebSocket communication with backend

**Component Hierarchy:**
- `App.vue` - Root component with global shortcuts and modal management
- `TownSquare.vue` - Main game board component
- `Player.vue` - Individual player tokens
- `Vote.vue` - Voting interface component
- `Menu.vue` - Main navigation and game controls
- Modal components in `src/components/modals/` for various game features

**Game Data Files:**
- `src/roles.json` - All character definitions with abilities and night order
- `src/editions.json` - Game edition definitions (TB, BMR, SNV)
- `src/nightsheet.json` - Night phase ordering for characters
- `src/fabled.json` - Fabled character definitions
- `src/customs.json` - Custom character support
- `src/hatred.json` - Character jinx definitions

### Backend (WebSocket Server)

**Key Features:**
- WebSocket server with SSL support for production
- Channel-based room management
- Rate limiting and spam protection
- Player validation with secret-based authentication
- Prometheus metrics collection
- Host duplicate prevention per channel

**Configuration:**
- Development: Runs on port 8001 without SSL
- Production: Uses Let's Encrypt certificates from `/etc/letsencrypt/live/clocktower.live/`
- Origin validation for `github.io`, `localhost`, and `clocktower.live` domains

## Custom Character Support

The application supports fully custom characters and scripts. Custom characters require:
- **Required**: `id`, `name`, `team`, `ability`
- **Optional**: `image`, `edition`, `firstNight`/`otherNight`, `reminders`, `setup`

Teams: `townsfolk`, `outsider`, `minion`, `demon`, `traveller`, `fabled`

## Project Structure

```
src/
├── components/           # Vue components
│   ├── modals/          # Modal dialog components
│   ├── TownSquare.vue   # Main game board
│   ├── Player.vue       # Player token component
│   └── ...
├── store/               # Vuex state management
│   ├── modules/         # Store modules (players, session)
│   ├── index.js         # Main store configuration
│   └── socket.js        # WebSocket integration
├── assets/              # Static assets (images, fonts, sounds)
├── *.json               # Game data files
└── main.js              # Application entry point

server/
├── index.js             # WebSocket server
├── ecosystem.config.js  # PM2 configuration
└── README.md           # Backend documentation
```

## Keyboard Shortcuts

The application includes extensive keyboard shortcuts (defined in `App.vue`):
- **G**: Toggle Grimoire/Town Square view
- **A**: Add player
- **H**: Host session
- **J**: Join session
- **R**: Reference modal
- **N**: Night order modal
- **E**: Edition modal (storyteller only)
- **C**: Character modal (storyteller only)
- **S**: Toggle night phase (storyteller only)
- **Arrow Up/Down**: Vote during nominations

## Live Session Features

- Real-time player synchronization via WebSocket
- Live voting with visual feedback  
- Character distribution and role assignment
- Night phase coordination
- Spectator mode support
- Cross-platform compatibility