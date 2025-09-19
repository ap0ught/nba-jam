# NBA Jam - Classic Arcade Basketball

This repository contains the source code for **NBA Jam**, the legendary basketball arcade game originally developed by Williams Electronics (later Midway) in the early 1990s. NBA Jam revolutionized arcade basketball with its fast-paced, over-the-top gameplay featuring real NBA players and teams.

## About NBA Jam

NBA Jam was one of the most successful arcade basketball games ever created, known for its:
- **"On Fire" gameplay mechanics** - Players could catch fire after consecutive shots
- **Exaggerated slam dunks** and athletic moves
- **Real NBA players and teams** with official licensing
- **Fast-paced 2-on-2 action** with simplified rules
- **Memorable announcer calls** like "He's on fire!" and "Boomshakalaka!"

The game was powered by Williams' custom hardware featuring the **TMS34010 Graphics System Processor (GSP)** and represented a significant technical achievement in arcade gaming.

## Technical Overview

This codebase is written primarily in **TMS34010 assembly language** for the Graphics System Processor, which was Williams' arcade platform of choice during this era. The architecture includes:

- **TMS34010 GSP** - Texas Instruments graphics processor handling display and game logic
- **Custom sound system** with speech synthesis
- **High-resolution graphics** with palette-based sprite systems
- **Real-time 3D-style player scaling and rotation**

## Repository Structure

The source code is organized into several key components:

### Core Game Files
- **MAIN.ASM** - System initialization and interrupt handlers
- **BB.ASM, BB2.ASM, BB3.ASM, BB4.ASM** - Main basketball game logic
- **PLYR*.ASM** - Player control, movement, and behavior systems
- **PLYRAT*.ASM** - Player attributes and statistics

### Game Systems
- **ATTRACT.ASM** - Attract mode sequences
- **SELECT*.ASM** - Team and player selection screens
- **MAKEPLR*.ASM** - Create-a-player functionality
- **SCORE*.ASM** - Scoring and statistics tracking
- **AUDIT.ASM** - Game auditing and coin-op management

### Graphics and Audio
- **IMGPAL*.ASM** - Image palettes and color data
- **BGND*.ASM** - Background graphics systems
- **SPEECH.ASM** - Announcer speech and sound effects
- **SOUNDS.ASM** - Audio system management

### Build System
- **MAKEFILE** - Build configuration
- **BB.CMD** - Linker command file
- **BBL.BAT** - Primary build script
- Various **\*.BAT** files for different build configurations

## Historical Significance

NBA Jam was groundbreaking for several reasons:

1. **First major sports game with real player likenesses** and official NBA licensing
2. **Technical innovation** in real-time sprite scaling and rotation
3. **Cultural impact** - introduced basketball video games to mainstream audiences
4. **Arcade revenue success** - one of the highest-grossing arcade games of the 1990s

The game featured real NBA rosters from the 1993-94 season, with notable exclusions like Michael Jordan due to licensing restrictions.

## Legacy

NBA Jam spawned numerous sequels and ports across various platforms, influencing sports video game design for decades. Its emphasis on fun, accessible gameplay over simulation became a template for "arcade-style" sports games.

This source code represents a piece of video game history, showcasing the technical craftsmanship and creative vision that made NBA Jam a timeless classic.

## Copyright and Legal

**COPYRIGHT (C) 1992-1995 WILLIAMS ELECTRONICS GAMES, INC.**

This source code is historical in nature. NBA and team names are trademarks of the National Basketball Association and respective teams. All rights to the NBA Jam franchise belong to their respective copyright holders.
