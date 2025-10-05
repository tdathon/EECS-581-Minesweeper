# Minesweeper System
**Course:** EECS 581 Software Engineering II, Fall 2025  
**Professor:** Hossein Saiedian  
**Project 2:** Minesweeper System Extension

## Project Overview
An enhanced version of a single-player puzzle game implementation of Minesweeper featuring a 10x10 grid where players uncover cells to reveal numbers indicating adjacent mines while avoiding detonation. Players can flag suspected mine locations and must uncover all non-mine cells to win.  

This project extends a previous team’s Minesweeper system by introducing an **AI Solver** with three difficulty levels, **sound effects** for user interactions, and general **bug fixes** to improve stability and gameplay consistency.

## Team Information
- **Team Members:** Daniel Butler, Charley Findling, Skylar Franz, Jack Gerety, Beckett Malinowski  
- **Development Platform:** HTML/CSS/JavaScript  
- **Inherited From:** Ted Athon, Alec Slavik, Dylan O’Brien, Divit Kannan, Nick Reinig, Amrit Sian

## Features
### Board Configuration
- 10x10 grid with columns labeled A–J and rows numbered 1–10  
- User-specified mine count (10–20 mines)  
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

---

## New Features (Project 2 Extension)

### AI Solver
The AI Solver automates gameplay based on selected difficulty mode. It follows the same rules and uses the same board as the human player.

#### Easy Mode
- Selects random hidden cells to uncover.  
- Avoids flagged or already revealed cells.

#### Medium Mode
- Implements two deduction rules:
  1. If the number of hidden neighbors of a revealed cell equals that cell’s number, flag all hidden neighbors.  
  2. If the number of flagged neighbors of a revealed cell equals that cell’s number, uncover all other hidden neighbors.  
- If no rules apply, the AI makes a random valid move.

#### Hard Mode
- Extends Medium Mode logic by adding the **1-2-1 pattern rule**:
  - When three adjacent revealed cells show the numbers “1-2-1”, the AI infers that the outer hidden cells are mines and the inner one is safe.  
- Falls back to random moves when no deduction applies.

### Sound Effects
- Added sound effects for the following events:
  - Cell reveal  
  - Flag placement/removal
  - Mine detonation  

---

## Installation and Setup

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)  
- No additional software installation required  

### Running the Game
1. Clone the forked repository to your local machine.  
2. Open `minesweeper.html` in a web browser.  
3. Select mine count and AI difficulty, then click **Start Game** to begin.

---

## How to Play
1. **Set Mines:** Use the slider or number input to set mine count (10–20).  
2. **Select Difficulty:** Choose AI level – Easy, Medium, Hard – or Player Only (N/A).  
3. **Select AI Mode:** Choose AI mode - Interactive or Automatic.
4. **Start Game:** Click “Start Game” to initialize a new game.  
5. **Reveal Cells:** Left-click to uncover cells.  
6. **Flag Mines:** Right-click to flag suspected mines.  
7. **Win Condition:** Uncover all non-mine cells.  
8. **Lose Condition:** Uncover a cell containing a mine.  

### Game Controls
- **Left Click:** Reveal cell content  
- **Right Click:** Toggle flag on covered cell  
- **Reset Button:** Start new game with same settings  
- **AI Difficulty:** Dropdown selector for Easy / Medium / Hard / Player  

---

## System Architecture

The Minesweeper system is built around a modular structure that separates the game logic, user interface, and supporting features. Core game functionality—such as board generation, mine placement, and cell revealing—is handled by the main logic module. The interface uses event-driven updates to visually represent cell states and to respond to user interactions.

In Project 2, the inherited structure was maintained and refined. Existing bugs in board initialization and cell adjacency calculations were corrected, and the architecture was extended to incorporate AI-based gameplay and sound functionality without altering the system’s primary interface.

---

### Data Flow
1. **Initialization**
   - When the user starts a new game, the DOM is populated with cell elements representing the grid. Each cell div is tagged with `data-row` and `data-col` attributes corresponding to its coordinates.
   - The system resets global state variables, including mine positions, flags placed, revealed count, and game status. No mines are placed at this stage to ensure the first click is always safe.

2. **First Click**
   - On the first left-click, mines are placed randomly using `placeMines()`, which avoids the clicked cell by excluding its coordinates. The system updates the `minePositions` set with string keys of the form `"r,c"` for constant-time lookups.
   - After placement, the clicked cell is resolved as safe, and `floodReveal()` executes to recursively uncover neighboring empty cells and their adjacent numbered cells.

3. **Gameplay Progression**
   - **Left-click:** Reveals a cell if it is covered and unflagged. If the cell contains a mine, the game ends and all mines are revealed using `revealMines()`.
   - **Right-click:** Toggles a flag on a covered cell, updating the flag counter immediately.
   - The system continually checks win conditions after every reveal to determine if all safe cells are uncovered.

4. **AI Solver Integration**
   - The system introduces an **AI Solver** that can operate in three difficulty modes:
     - **Easy:** Randomly selects and reveals any hidden, unflagged cell.
     - **Medium:** Applies logical deductions:  
       - If a revealed number’s hidden neighbors equal that number, all such cells are flagged.  
       - If the number of flagged neighbors equals the revealed number, all other hidden neighbors are revealed.  
       - Falls back to random choice if no deduction applies.
     - **Hard:** Builds upon Medium logic, adding a **1-2-1 pattern rule**, allowing the AI to deduce additional mine and safe cell placements. The Hard AI can solve boards efficiently without detonating mines.
   - The AI operates on the same DOM-based cell structure as human players, using helper methods (`getCell`, `isMine`, `countAdjacentMines`, etc.) to simulate interaction.

5. **Sound and Feedback**
   - Audio feedback enhances immersion, including sounds for cell reveals, flag placements, and mine detonations. Sounds are triggered by event handlers tied to specific game actions.
   - These effects provide real-time feedback and improve user engagement without affecting core game logic.

6. **Game Conclusion**
   - **Loss:** Triggered by uncovering a mine; the board reveals all mine locations, plays a “loss” sound, and disables further input.
   - **Win:** Occurs when all non-mine cells are revealed. The system updates the game status, plays a “victory” sound, and displays a winning message.

### Key Data Structures

**DOM-Based Representation**
- Each cell on the board is a DOM element (`<div class="cell">`) with attributes `data-row` and `data-col`.  
- The system does not maintain a traditional 2D array. Instead, all logic references cells dynamically using helper functions that query the DOM directly.

**State Variables**
- `minePositions`: `Set<string>` — Stores all mine locations as `"r,c"` keys for O(1) lookups.
- `minesTotal`: `number` — Total number of mines for the current game.
- `flagsPlaced`: `number` — Number of flags currently on the board.
- `firstClick`: `boolean` — Indicates whether the player has made the first click.
- `minesPlaced`: `boolean` — Tracks whether mines have been distributed.
- `gameOver`: `boolean` — Prevents interaction after loss or victory.
- `revealedCount`: `number` — Counts how many safe cells have been revealed.

**Helper Methods**
- `isInBounds(r, c)`: Validates coordinates before operations.
- `getKey(r, c)`: Converts coordinates into `"r,c"` format for key-based access.
- `isMine(r, c)`: Checks the mine set for a given cell.
- `getCell(r, c)`: Retrieves the DOM element for a cell.
- `countAdjacentMines(r, c)`: Counts neighboring mines for number display.
- `placeMines(excludeRow, excludeCol)`: Randomly places mines, excluding the first-clicked cell.
- `floodReveal(startR, startC)`: Reveals cells recursively using a stack-based algorithm.
- `revealMines()`: Reveals all mines when the game ends.

### Enhancements and Improvements
1. **AI Solver**
   - Introduced multi-tiered AI capable of simulating player decisions through rule-based logic.
   - Difficulty levels (Easy, Medium, Hard) mirror real player intuition, offering interactive or automated solving modes.

2. **Audio Feedback**
   - Integrated sound effects for major game events: reveal, flag, win, and loss.
   - Enhances accessibility and engagement without increasing visual clutter.

3. **Refactoring and Bug Fixes**
   - Improved event handling to prevent race conditions between clicks and state updates.
   - Fixed edge cases involving first-click safety and flag counter synchronization.
   - Optimized DOM updates to minimize unnecessary reflows and improve performance.

4. **Code Maintainability**
   - Consolidated state management and clarified variable naming conventions.
   - Added clear comment headers to all major code blocks for future teams.
   - Ensured that all external sources and generated materials are properly attributed.