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
- **Do not cancel search or analysis operations**: Any build attempts will fail quickly, but search and analysis operations should complete normally.

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

## TMS34010 Assembly Syntax Guide

### Assembly File Structure
Every assembly file follows this standard structure:
```assembly
**************************************************************************
* File comment header with version and copyright info
**************************************************************************
	.file	"filename.asm"
	.title	"Module Description"
	.width	132
	.option	b,d,l,t
	.mnolist

	.include	gsp.equ       ; Hardware register definitions
	.include	sys.equ       ; System constants
	.include	macros.hdr    ; Macro definitions

; External references
	.ref	external_function
	.ref	external_data

; Constants and equates
CONSTANT_NAME	.equ	123

; Code sections
SUBR	function_name
	; function body
	rets

	.end
```

### Common Assembly Directives

#### File Header Directives
- `.file "name.asm"` - Specifies source filename for debugging
- `.title "description"` - Sets module title for listings  
- `.width 132` - Sets listing line width to 132 characters
- `.option b,d,l,t` - Assembler options (binary, decimal, list, title)
- `.mnolist` - Suppress macro expansion in listings

#### Include and Reference Directives  
- `.include filename` - Include another source file
- `.ref symbol` - Reference external symbol
- `.def symbol` - Define global symbol for export
- `.global symbol` - Make symbol globally visible

#### Conditional Assembly
```assembly
	.if	DEBUG
	; Debug-only code
	.ref	SLDEBUG
	.endif
```

### TMS34010 Instructions

#### Data Movement
```assembly
	move	src,dst,size		; Move data between registers/memory
	movi	#immediate,reg		; Move immediate value to register
	movk	#constant,reg		; Move small constant (1-32) to register
```

Examples from the codebase:
```assembly
	move	*a0(ODATA_p),a1,L	; Load long word from memory
	movi	>1000100,b5		; Load immediate hex value  
	movk	1,a14			; Load constant 1
	move	b9,b12			; Copy register to register
```

#### Arithmetic and Logic
```assembly
	add	src,dst			; Addition
	sub	src,dst			; Subtraction
	and	src,dst			; Bitwise AND
	or	src,dst			; Bitwise OR
	xor	src,dst			; Bitwise XOR
```

#### Control Flow
```assembly
	calla	subroutine		; Call subroutine (saves return address)
	callr	relative_addr		; Call relative address
	rets				; Return from subroutine
	jmp	address			; Unconditional jump
	jrz	label			; Jump if zero
	jrnz	label			; Jump if not zero
```

#### Graphics-Specific Instructions
```assembly
	setf	field,1,0		; Set graphics field register
	pixblt	B,L			; Pixel block transfer
```

Example from NDSP1.ASM:
```assembly
	setf	1,0,0			; Disable DMA interrupt
	move	sp,@INTENB+1		; Clear X1E interrupt enable
	setf	16,1,0			; Set 16-bit mode
```

### Register Usage Conventions

#### A-File Registers (Address/General Purpose)
- `a0-a14` - General purpose address registers
- `a13` - Frame pointer (fp)
- `a14` - Parameter stack pointer (pstk)
- `sp` - System stack pointer

#### B-File Registers (Graphics Operations)
- `b0` - SADDR (source address)
- `b1` - SPTCH (source pitch)  
- `b2` - DADDR (destination address)
- `b3` - DPTCH (destination pitch)
- `b4` - OFFSET
- `b5` - WSTART (window start)
- `b6` - WEND (window end)
- `b7` - DYDX (delta Y/delta X)
- `b8` - COLOR0
- `b9` - COLOR1
- `b10` - COUNT
- `b11` - INC1
- `b12` - INC2
- `b13` - PATTRN (pattern)

### Data Declarations

#### Basic Data Types
```assembly
	.byte	value		; 8-bit byte
	.word	value		; 16-bit word  
	.long	value		; 32-bit long word
	.even			; Align to even address
	.bss	symbol,size	; Reserve uninitialized space
```

Examples from the codebase:
```assembly
	.word	>F9F7,>50,>814A,0	; Sound data structure
	.long	sequence_address	; Pointer to animation sequence
	.byte	"Text string",0		; Null-terminated string
	.even				; Ensure word alignment
```

### Subroutine Definitions

#### SUBR Macro (Basic Subroutine)
```assembly
SUBR	function_name
	; function body
	rets
```

#### SUBRP Macro (Process Subroutine)  
```assembly
SUBRP	process_name
	; process body that may yield control
	rets
```

Examples from NDSP1.ASM:
```assembly
SUBR	dma_irq
	move	-*b14,-*b12,L		; Move long from [--b14] to [--b12]
	; interrupt handler code
	rets

SUBR	char_gen  
	move	*a0(ODATA_p),a1,L	; Get player name pointer
	move	b7,a7			; SCALEY:SCALEX
	; character generation code
	rets
```

### Animation and Game Macros

#### Animation Timing Macros
```assembly
WLW	time,frame,flags	; Wait-Long-Word: timing, image, flags
WL	time,function		; Wait-Long: timing, function call
WLLW	time,func,param,value	; Wait-Long-Long-Word: complex timing
W0				; End marker (wait 0)
```

Examples from DUNK.ASM:
```assembly
	WLW	4,M1SPDU1,F		; Display frame M1SPDU1 for 4 ticks
	WLW	-1,seq_jam_speech,JAM_EASY  ; Call speech function
	WL	-1,seq_call_name	; Call name announcement  
	WLW	260,M1SPDU8,F		; Wait 260 ticks (landing delay)
	W0				; End sequence
```

#### Stack Operations
```assembly
PUSH	reg1,reg2,reg3		; Push registers onto stack
PULL	reg1,reg2,reg3		; Pull registers from stack
```

#### Data Structure Macros
```assembly
LWW	long_val,word1,word2	; Long-Word-Word structure
LWWWWWW	long_val,w1,w2,w3,w4,w5,w6  ; Complex data structure
```

### Memory Addressing

#### Addressing Modes
```assembly
	move	*a0,a1		; Indirect addressing
	move	*a0+,a1		; Post-increment
	move	*a0-,a1		; Post-decrement  
	move	-*a0,a1		; Pre-decrement
	move	*a0(offset),a1	; Indexed addressing
	move	@address,a1	; Absolute addressing
```

#### Memory Organization
- `@INTENB` - Interrupt enable register
- `@CONTROL` - Graphics control register
- Graphics memory uses special GSP addressing
- Object data structures use offset notation: `*a0(ODATA_p)`

### Comments and Labels

#### Comment Styles
```assembly
; Single line comment
* Alternative comment style (legacy)

;--------------------  
; Section divider
;--------------------

********************************
* Major section header  
********************************
```

#### Label Definitions
```assembly
function_name:		; Global label
#local_label		; Local label (scope limited)
lp?			; Loop label with auto-generation
```

### Conditional Compilation

#### Debug Code
```assembly
	.if	DEBUG
	.ref	SLDEBUG
	calla	debug_function
	.endif
```

#### Platform-Specific Code
```assembly
	.if	NBA_VERSION
	; NBA JAM specific code
	.else  
	; Other version code
	.endif
```

This syntax guide covers the essential TMS34010 assembly patterns used throughout the NBA JAM codebase. The examples are taken directly from the source files to ensure accuracy and practical relevance.

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