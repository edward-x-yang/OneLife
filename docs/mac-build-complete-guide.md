# Complete Mac Build Guide for One Hour One Life

## Overview
This guide documents the complete process to build and run One Hour One Life natively on macOS.

## Prerequisites
- macOS with Xcode Command Line Tools
- Homebrew package manager
- OneLife source code
- OneLifeData game assets

## Step 1: Install Dependencies

```bash
# Install Xcode Command Line Tools
xcode-select --install

# Install required packages
brew install sdl12-compat
brew install imagemagick
```

## Step 2: Set Up Source Code

```bash
# Clone required repositories
cd /Users/edward/fun_projects
git clone https://github.com/jasonrohrer/minorGems.git
git clone https://github.com/jasonrohrer/OneLife.git
git clone https://github.com/jasonrohrer/OneLifeData.git
```

## Step 3: Configure Build System

```bash
cd OneLife
./configure 2  # Select option 2 for MacOSX
```

## Step 4: Fix Makefile for Homebrew SDL

Edit `gameSource/Makefile` and update the following lines:

```makefile
# Change from:
PLATFORM_COMPILE_FLAGS = -DBSD -D__mac__ -I/System/Library/Frameworks/OpenGL.framework/Headers
PLATFORM_LINK_FLAGS = -framework OpenGL -framework SDL -framework Cocoa ../../minorGems/game/platforms/SDL/mac/SDLMain.m ${CUSTOM_MACOSX_LINK_FLAGS}

# To:
PLATFORM_COMPILE_FLAGS = -DBSD -D__mac__ -I/System/Library/Frameworks/OpenGL.framework/Headers -I/opt/homebrew/Cellar/sdl12-compat/1.2.68/include
PLATFORM_LINK_FLAGS = -framework OpenGL -L/opt/homebrew/Cellar/sdl12-compat/1.2.68/lib -lSDL -framework Cocoa ../../minorGems/game/platforms/SDL/mac/SDLMain.m ${CUSTOM_MACOSX_LINK_FLAGS}
```

## Step 5: Build the Game

```bash
cd gameSource
make
```

## Step 6: Link Game Data

```bash
# Create symlinks to game data
ln -sf ../../OneLifeData/sprites .
ln -sf ../../OneLifeData/objects .
ln -sf ../../OneLifeData/transitions .
ln -sf ../../OneLifeData/categories .
ln -sf ../../OneLifeData/animations .
ln -sf ../../OneLifeData/sounds .
ln -sf ../../OneLifeData/music .
ln -sf ../../OneLifeData/ground .
ln -sf ../../OneLifeData/contentSettings .
ln -sf ../../OneLifeData/dataVersionNumber.txt .
ln -sf ../../OneLifeData/isAHAP.txt .
```

## Step 7: Generate Game Caches

```bash
./regenerateCaches
```

## Step 8: Convert Graphics Files

```bash
# Convert PNG graphics to TGA format
for file in graphicsSource/*.png; do
    basename=$(basename "$file" .png)
    magick "$file" -auto-orient -type truecolormatte "graphics/${basename}.tga"
done
```

## Step 9: Launch the Game

```bash
./OneLife
```

## Troubleshooting

### Common Issues:

1. **"SDL.h not found"**: Make sure SDL paths are correctly set in the Makefile
2. **"Cannot find game data"**: Verify all symlinks are created correctly
3. **Segmentation fault on graphics**: Run the graphics conversion step
4. **"Class SDLApplication implemented in both"**: Warning only, doesn't affect functionality

### Verification Steps:

```bash
# Check binary was created
ls -la OneLife

# Check linked libraries
otool -L OneLife

# Check symlinks
ls -la | grep -E "(sprites|objects|transitions)"

# Check graphics files
ls graphics/ | grep -E "(hiddenFieldTexture|guiPanel|notePaper)"
```

## Build Results

- **Binary Size**: ~2.5MB
- **Architecture**: ARM64 (Apple Silicon native)
- **Dependencies**: OpenGL, SDL, Cocoa frameworks
- **Performance**: Native speed on Apple Silicon Macs

## Directory Structure After Build

```
OneLife/gameSource/
├── OneLife                    # Main executable
├── graphics/                  # Converted TGA graphics
├── sprites -> ../../OneLifeData/sprites
├── objects -> ../../OneLifeData/objects
├── sounds -> ../../OneLifeData/sounds
├── music -> ../../OneLifeData/music
├── animations -> ../../OneLifeData/animations
├── transitions -> ../../OneLifeData/transitions
├── categories -> ../../OneLifeData/categories
├── ground -> ../../OneLifeData/ground
├── contentSettings -> ../../OneLifeData/contentSettings
├── dataVersionNumber.txt -> ../../OneLifeData/dataVersionNumber.txt
└── isAHAP.txt -> ../../OneLifeData/isAHAP.txt
```

## Success Criteria

✅ Game compiles without errors  
✅ All libraries link correctly  
✅ Game data loads successfully  
✅ Graphics render without crashes  
✅ Game launches and runs stably  

## Notes

- The original Steam version is Windows-only by distribution choice, not technical limitation
- This build process creates a fully functional native Mac version
- All game mechanics and features work identically to the Windows version
- The build is suitable as a foundation for creating derivative games