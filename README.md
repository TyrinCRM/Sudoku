# Sudoku

A web-based Sudoku app built with Asp.Net Core 2.0.

Working Game at https://cedarrivertech.com/sudoku

Done
* Build
* Simple random clues
* User interface (functional, not pretty)
* Pencil toggle
* Keyboard shortcuts
* Scoring
* Decent Layout

To Do
* Save games
* Options (difficulty, hints)
* Make it look even better

## Puzzle Creation

I started out trying to build the algorithm myself, and finally had to go search for some help. So this is based on Daniel Beer's post https://dlbeer.co.nz/articles/sudoku.html.

Models:
Game (81 boxes, indexed by row, column, and square)
* Box (one writing "square")
  * Answer
  * Pencil (List<int> of possible answers)
  * Guess
  * Row
  * Column
  * Square (group of 9 boxes)
  * Index (spot in gameboard, based on row and column)
    
Algorithm:
  We use System.Random to generate possible values, and a randomly-ordered array of integers 1-9 to generate a row, column, or square.
  1. Build the top-left square of 3x3. Any arrangement is possible.
  2. Build the middle and right squares. This takes two passes, first with the "Pencil" attribute, listing possibities that don't conflict, then filling in answers, updating the other boxes "Pencil" lists as each answer is added.
  3. Build the first column on the left. This follows the same algorithm as step 2.
  4. Create a random "Seed" int of 1-9.
  5. Pencil the remaining boxes, then sort by the number of possibilities. Answer starting with the box with the fewest possibilities, using the "Seed number from step 4. As in step 2, recalculate all pencil possibilities after each answer is added.
  6. If the puzzle can't be completed, it loops through again with a different Seed, until all 9 numbers are tried.
  7. If the puzzle doesn't work with any Seed, it reloads back to step one, with a new initial box.
  
## User Interface

The user interface is created with HTML, CSS, and Typescript, one file of each. The main layout is a table, with nine rows and nine columns. Within each cell (<td>), there is another table for "penciling" in answers, and a larger input field, overlaying the small table. The pencil toggle button brings the small table to the forefront (z-index), and there is a numbered button 1-9 for each possibility.
