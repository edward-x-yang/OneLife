# One Hour One Life - Tech Stack & Architecture Analysis

## Overview

One Hour One Life is a multiplayer survival game created by Jason Rohrer and released into the public domain. This document provides a comprehensive analysis of the game's technical architecture and technology stack.

## Tech Stack

### Programming Languages
- **C++**: Primary language for client, server, and editor components
- **PHP**: Web services and auxiliary servers
- **Shell Scripts**: Build system automation

### Core Dependencies

#### Graphics & Multimedia
- **SDL (Simple DirectMedia Layer)**: Cross-platform multimedia library
  - Graphics rendering
  - Audio playback
  - Input handling
  - Window management

- **OpenGL**: 3D graphics rendering API
- **libpng**: PNG image format support

#### Game Engine
- **minorGems**: Custom game engine/framework by Jason Rohrer
  - Located in `../minorGems` (external dependency)
  - Provides core game development utilities
  - Networking, graphics, audio, and system abstractions

#### Audio Libraries
- **stb_vorbis**: OGG Vorbis audio decoding
- **STB libraries**: Image processing and manipulation

#### Build System
- **Traditional Unix configure/make**: Primary build system
- **Custom shell scripts**: Platform-specific build automation
- **Cross-platform support**: Windows, Mac, Linux

## Architecture Overview

The codebase is organized into several distinct components:

### 1. Client (`gameSource/`)

**Main Entry Point**: `game.cpp`

**Key Components**:
- **GUI System**: Page-based user interface
  - `GamePage.cpp`: Main gameplay interface
  - `SettingsPage.cpp`: Configuration management
  - `LivingLifePage.cpp`: Core gameplay logic
  - Various specialized pages (Login, Editor, etc.)

- **Rendering System**:
  - `spriteBank.cpp`: Sprite management and rendering
  - `animationBank.cpp`: Animation system
  - `groundSprites.cpp`: Terrain rendering
  - `zoomView.cpp`: Camera and viewport management

- **Audio System**:
  - `soundBank.cpp`: Sound effect management
  - `musicPlayer.cpp`: Background music playback
  - `ogg.cpp`: Audio format support

- **Game Logic**:
  - Object and transition systems
  - Player lifecycle management
  - Network communication with server

### 2. Server (`server/`)

**Main Entry Point**: `server.cpp`

**Key Components**:
- **Networking**: Socket-based multiplayer communication
- **World Simulation**:
  - `map.cpp`: Game world management
  - `playerStats.cpp`: Player state tracking
  - `lifeLog.cpp`: Player lifecycle logging

- **Database Systems**:
  - Custom database implementations
  - Player data persistence
  - Lineage and family tracking

- **Game Mechanics**:
  - `triggers.cpp`: Event system
  - `monument.cpp`: World monuments
  - `specialBiomes.cpp`: Environmental systems

### 3. Editor (`gameSource/editor.cpp`)

**Purpose**: Content creation and modding tool

**Features**:
- Sprite import/export
- Object creation and editing
- Transition system editing
- Animation creation

### 4. Web Services (PHP-based)

Multiple specialized servers for extended functionality:

- **ahapGate/**: AHAP (A Home for All People) integration
- **curseServer/**: Player reputation system
- **lineageServer/**: Family tree tracking
- **photoServer/**: In-game photography
- **fitnessServer/**: Player fitness scoring
- **lifeTokenServer/**: Life token management

## Directory Structure

```
OneLife/
├── gameSource/          # Client and editor source code
├── server/              # Game server implementation
├── commonSource/        # Shared code between client/server
├── documentation/       # Game documentation and notes
├── scripts/             # Build and deployment scripts
├── build/              # Build system files
├── ahapGate/           # AHAP web service
├── curseServer/        # Curse system web service
├── lineageServer/      # Lineage tracking web service
├── photoServer/        # Photo sharing web service
├── fitnessServer/      # Fitness scoring web service
├── lifeTokenServer/    # Life token web service
└── reflector/          # Server discovery service
```

## Build System

### Configuration
- **Main configure script**: `./configure`
- **Dependencies**: Requires minorGems framework
- **Platform-specific**: Separate configurations for different platforms

### Build Process
1. **Client**: `cd gameSource && ./makeEditor.sh`
2. **Server**: `cd server && ./configure && make`
3. **Dependencies**: Requires linking to game data folders

### Key Build Files
- `makeFileList`: Generates file lists for compilation
- `makeEditor.sh`: Client build script
- Various platform-specific makefiles

## Network Architecture

### Client-Server Communication
- **Protocol**: Custom TCP-based protocol
- **Real-time**: Low-latency gameplay communication
- **Authentication**: Optional ticket server integration

### Web Services Integration
- **Modular design**: Separate services for different features
- **REST-like APIs**: HTTP-based communication
- **Database backing**: MySQL/PHP stack for persistence

## Modding and Extensibility

### Content System
- **Data-driven**: Game content stored in external files
- **Editor support**: Built-in content creation tools
- **Version control**: Data versioning system

### Custom Servers
- **Private servers**: Support for custom server deployment
- **Password protection**: Client password authentication
- **Custom content**: Support for modified game data

## Development Notes

### Code Quality
- **Well-organized**: Clear separation of concerns
- **Cross-platform**: Consistent across Windows, Mac, Linux
- **Modular**: Loosely coupled components
- **Public domain**: Open source with permissive licensing

### Performance Considerations
- **C++ core**: High-performance game logic
- **Efficient rendering**: Optimized sprite and animation systems
- **Scalable server**: Designed for multiplayer gameplay

## Dependencies Setup

### Required Libraries
- SDL development libraries
- libpng development package
- Standard C++ compilation tools

### Example Ubuntu Setup
```bash
sudo apt-get install libpng12-dev
```

### minorGems Framework
- External dependency located at `../minorGems`
- Provides core game engine functionality
- Must be properly configured before building

## Conclusion

One Hour One Life demonstrates a well-architected multiplayer game with clear separation between client, server, and content systems. The use of C++ for performance-critical components combined with PHP web services for extended functionality provides a solid foundation for creating derivative versions of the game.

The codebase's public domain status and modular design make it an excellent starting point for developing custom versions or studying multiplayer game architecture.