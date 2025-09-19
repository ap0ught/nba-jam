# NBA JAM Arcade Game Source Code

NBA JAM is a classic 1990s arcade basketball game developed by Midway/Williams Electronics. This repository contains the complete source code written in TMS34010 assembly language for the arcade hardware.

**ALWAYS follow these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.**

## Working Effectively

### Important Limitations
- **CRITICAL**: This codebase CANNOT be built in modern environments. The build system requires specialized vintage tools:
  - `gsplnk` (GSP linker for TMS34010 processor)
  - `tv` (TMS34010 viewer/debugger)
  - `load2` (Image loading utility)
  - DOS/Windows 95-era development environment
- **DO NOT attempt to build or run this code** - it will fail due to missing specialized assemblers and tools.
- **NEVER CANCEL**: Any build attempts will fail quickly, but search and analysis operations should complete normally.

### Platform Architecture
- **Processor**: TMS34010 Graphics System Processor (GSP)
- **Platform**: Williams/Midway arcade hardware (1990s)
- **Language**: TMS34010 assembly language
- **Development Environment**: Originally developed on DOS/early Windows systems

### Repository Structure
The main components are:
- `BB.ASM`, `BB2.ASM`, `BB3.ASM`, `BB4.ASM` - Core basketball game logic
- `MAIN.ASM` - System initialization and interrupt handlers  
- `PLYR*.ASM` - Player movement, actions, and animation sequences
- `SCORE*.ASM` - Scoring and statistics systems
- `SELECT*.ASM` - Team and player selection screens
- `ATTRACT.ASM` - Attract mode and demonstrations
- `MAKEPLR.ASM` - Player creation system
- `SOUNDS.ASM`, `SPEECH.ASM` - Audio systems
- `*.EQU` files - Assembly equates (constants and definitions)
- `*.HDR` files - Macro definitions and includes
- `*.TBL` files - Data tables for graphics and game data
- `IMG/` directory - Graphics assets and image data
- `OLD/` directory - Legacy/backup versions of source files

### Build System (Historical Reference Only)
The original build process used these steps:
1. `make -f MAKEFILE` - Assembles .ASM files to .OBJ files
2. `gsplnk bb.cmd` - Links objects using GSP linker
3. `tv bb /v` - Loads to TMS34010 development system
4. `BBL.BAT` - Image loading batch script
5. `MAKEROMS.BAT` - ROM generation for arcade boards

**These tools are not available in modern environments - DO NOT attempt to run them.**

## Navigation and Code Analysis

### Key Areas to Focus On
- **Game Logic**: Start with `BB.ASM` for main game loop and rules
- **Player System**: `PLYR.ASM`, `PLYR2.ASM`, `PLYR3.ASM` for player mechanics
- **Animation**: `PLYR*SEQ.ASM` files contain animation sequences and moves
- **Special Moves**: `DUNK.ASM`, `PLYRDSEQ.ASM` for dunk animations and special moves
- **Team Data**: `HEY.DOC` contains all NBA team rosters and player statistics
- **Graphics**: `IMG/` directory contains all visual assets
- **Audio**: `SOUNDS.ASM` and `SPEECH.ASM` for game audio

### Understanding the Code
- Assembly syntax uses TI TMS34010 conventions
- `.include` directives reference header files
- `SUBRP`/`SUBR` define subroutines
- `W0`, `WL`, `WLW` are animation timing macros
- Memory addresses use GSP memory mapping
- Graphics use custom Williams/Midway sprite formats

### Common Development Tasks
When working with this codebase:

1. **Reading Game Logic**: Start with `BB.ASM` and follow include chains
2. **Understanding Player Stats**: Check `HEY.DOC` for team/player data structure
3. **Animation Analysis**: Look at `PLYR*SEQ.ASM` files for move sequences
4. **Graphics Research**: Examine `IMG/` directory for asset organization
5. **Documentation Review**: Check `*.DOC` files for development notes

### Validation (Analysis Only)
Since the code cannot be built, validation involves:
- **Code Review**: Analyze assembly syntax and logic flow
- **Data Validation**: Check player stats and team data for accuracy
- **Asset Verification**: Ensure graphics files are properly referenced
- **Documentation Check**: Verify comments match code implementation

Always analyze code structure and logic rather than attempting compilation.

## File Organization Reference

### Core Game Files
```
BB.ASM - Main game engine
BB2.ASM - Game state management  
BB3.ASM - Additional game logic
BB4.ASM - Player attribute handling
MAIN.ASM - System initialization
```

### Player System
```
PLYR.ASM - Basic player mechanics
PLYR2.ASM - Player physics and movement
PLYR3.ASM - Player graphics and rendering
PLYRAT.ASM - Player attributes and stats
PLYRSEQ.ASM - Animation sequences
PLYRDSEQ.ASM - Dunk animations
PLYRSTND.ASM - Standing/idle animations
```

### Game Screens
```
SELECT.ASM - Team selection screen
SELECT2.ASM - Player selection  
SELECT3.ASM - Additional selection screens
SELECT4.ASM - Final selection confirmation
ATTRACT.ASM - Demo/attract mode
MAKEPLR.ASM - Create-a-player system
```

### System Files
```
NDSP1.ASM - Display processor
UTIL.ASM - Utility functions
PAL.ASM - Palette management
MPROC.ASM - Multi-processing system
SOUNDS.ASM - Audio system
SPEECH.ASM - Voice samples
```

## Important Notes

- This is historical arcade game source code from 1996
- The codebase represents significant gaming history and technical achievement
- Original development used specialized hardware and software tools
- Focus on code analysis and understanding rather than compilation
- The assembly language and architecture are specific to TMS34010 processors
- Graphics and audio assets use proprietary Williams/Midway formats

**Remember: This codebase cannot be built or executed in modern environments. Use it for analysis, research, and understanding of classic arcade game development.**