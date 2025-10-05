# Minesweeper System
**Course:** EECS 581 Software Engineering II, Fall 2025  
**Professor:** Hossein Saiedian  
**Project 2:** Minesweeper System Extension

## Project Overview
A single-player puzzle game implementation of Minesweeper featuring a 10x10 grid where players uncover cells to reveal numbers indicating adjacent mines while avoiding detonation. Players can flag suspected mine locations and must uncover all non-mine cells to win.

## Team Information
- **Team Members:** Ted Athon, Alec Slavik, Dylan O’Brien, Divit Kannan, Nick Reinig, Amrit Sian
- **Development Platform:** HTML/CSS/JavaScript

## Features
### Board Configuration
- 10x10 grid with columns labeled A–J and rows numbered 1–10
- User-specified mine count (10-20 mines)
- Random mine placement with guaranteed safe first click
- All cells start in covered state

### Gameplay Mechanics
- Left-click to uncover cells
- Right-click to toggle flags on covered cells
- Recursive uncovering for cells with zero adjacent mines
- Number display showing adjacent mine count
- Flagged cells cannot be uncovered until unflagged

### User Interface
- Visual grid showing all cell states (covered, flagged, uncovered)
- Real-time mine count display
- Remaining flag counter
- Game status indicator (Not Started, Playing, Game Over, Victory)
- Interactive controls for mine count selection

### Game Conclusion
- **Loss:** Triggered by uncovering a mine, reveals all mine locations
- **Win:** Achieved by uncovering all non-mine cells without detonation

## Installation and Setup

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- No additional software installation required

### Running the Game
1. Clone the repository to your local machine
2. Open `minesweeper.html` in any modern web browser
3. Click "Start Game" to begin playing

## How to Play
1. **Start Game:** Click "Start Game" button to initialize a new game
2. **Set Mines:** Use the slider or number input to set mine count (10-20)
3. **Reveal Cells:** Left-click on covered cells to uncover them
4. **Flag Mines:** Right-click on covered cells to place/remove flags
5. **Win Condition:** Uncover all non-mine cells
6. **Lose Condition:** Uncover a cell containing a mine

### Game Controls
- **Left Click:** Reveal cell content
- **Right Click:** Toggle flag on covered cell
- **Mine Counter:** Shows total mines on board
- **Flag Counter:** Shows remaining flags available
- **Reset Button:** Start new game with same settings



## System Architecture

### Purpose
This project delivers a self-contained Minesweeper game for a 10x10 grid with a user-selectable mine count. It focuses on clarity, simple DOM manipulation, and a small set of core data structures that make the logic easy to follow. The design keeps rendering and game state close together so that user actions map directly to visual updates.

### High Level Description
The application runs fully in the browser, no backend required. The HTML defines controls, the status bar, and an empty board container. The JavaScript bootstraps the UI, wires events, and manages all game state in memory. CSS provides a simple grid layout and visual states for cells such as covered, revealed, flagged, and mine.

### Core Subsystems
1. UI Controls and Status: Buttons and inputs for starting, resetting, and selecting the mine count. Spans show current state, total mines, and remaining flags.
2. Board Builder: Functions that render column labels, row labels, and the 10x10 grid. Each cell is a div with data attributes for its row and column.
3. Game State Manager: A small set of variables track gameplay, including the mine set, flags placed, revealed count, and whether the game is over.
4. Game Logic: Helper functions for bounds checks, adjacency counts, mine placement that avoids the first click, flood-fill reveal, win detection, and global mine reveal on loss.
5. Event Layer: Click reveals a cell, contextmenu toggles a flag, inputs keep the slider and number in sync. Events read and update state, then update the DOM.

### Data Flow
1. Start Game: The user selects a mine count, clicks Start, and the system resets state, updates counters, shows Playing, and builds the board with fresh cell elements. No mines are placed yet.
2. First Reveal: On the first left click, the system places mines randomly while excluding the clicked cell. The cell is resolved as safe, the flood-fill runs, and the DOM updates covered and revealed classes along with adjacency numbers.
3. Subsequent Actions
   - Left click on a covered cell triggers either a mine reveal that ends the game or a safe reveal with possible flood-fill.
   - Right click toggles a flag if the game is active. The remaining flags counter updates immediately.
4. Win or Loss
   - Loss sets the state to Game over, reveals all mines, and disables further moves.
   - Win occurs when the count of revealed safe cells equals total safe cells, shows You won, and reveals mines for confirmation.

### Key Data Structures

Constants
- BOARD_SIZE: number,
  Fixed size of the grid, 10 in this implementation.
- MAX_MINES: number,
  Safety cap that ensures at least one safe cell exists.

DOM Handles
- board, columnLabels, minesEl, flagsEl, stateEl, startBtn, resetBtn, mineRange, mineNumber
- Cached references to avoid repeated lookups and to centralize updates.

Game State
- minesTotal: number,
  Total number of mines selected by the user.
- flagsPlaced: number,
  Count of flags currently on the board.
- firstClick: boolean,
  Ensures the first reveal is always safe and triggers mine placement.
- minesPlaced: boolean,
  Tracks whether random placement has occurred.
- gameOver: boolean,
  Gates all interactions once set.
- revealedCount: number,
  Number of safe cells revealed, used for win detection.
- minePositions: Set<string>,
  Stores coordinates as "r,c" strings for O(1) membership checks.

Helpers and Algorithms
- isInBounds(r, c): boolean,
  Validates coordinates before reads or writes.
- getKey(r, c): string,
  Normalizes coordinates to the "r,c" key used in the set.
- isMine(r, c): boolean,
  Checks the set for a mine at a given coordinate.
- getCell(r, c): HTMLElement | null,
  Resolves a cell element with a query selector that uses data attributes.
- countAdjacentMines(r, c): number,
  Iterates neighbors to compute the number display.
- placeMines(excludeRow, excludeCol): void,
  Randomly fills minePositions while avoiding the first clicked cell.
- floodReveal(startR, startC): number,
  Iterative depth-first style expansion using a stack and a visited set. Reveals blanks and their neighbors, returns the number of newly revealed cells.
- checkWin(): void,
  compares revealedCount to total safe cells for victory.
- revealMines(): void,
  Marks all mines in the UI when the game ends.
