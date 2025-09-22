# Minesweeper System
**Course:** EECS 581 Software Engineering II, Fall 2025  
**Professor:** Hossein Saiedian  
**Project 1:** Minesweeper System Development

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
