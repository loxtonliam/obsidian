At first glance, the formulas in Advanced Tables look like absolute gibberish. But once you learn the "secret code," it is actually just a simple game of Battleship using coordinates.

Here is the straightforward guide to writing your own formulas from scratch.

## 1. The Container
Every formula must live inside a specific HTML comment block, placed **directly** below your table with zero empty lines in between. 
`<!-- TBLFM: [your formula goes here] -->`

## 2. The Coordinates
The plugin finds cells using a coordinate system based on Rows (`@`) and Columns (`$`).

| Symbol | Meaning | Example |
| :--- | :--- | :--- |
| **`$`** | **Column** | `$1` is the 1st column, `$3` is the 3rd column. |
| **`@`** | **Row** | `@1` is the 1st row (usually your header). |

To target a specific single cell, combine them: **`@3$2`** means "Row 3, Column 2".

## 3. The "Magic" Shortcuts
Counting rows manually is annoying, especially when you keep adding new assignments. Instead, the plugin has shortcuts that automatically find the top and bottom of your table:

| Shortcut | What it targets |
| :--- | :--- |
| **`@>`** | The **very last row** of the table. |
| **`@I`** | The **first row of actual data** (skipping your header). |
| **`@-1`** | The row **directly above** wherever the formula is acting. |

## 4. Ranges and Math
To select a chunk of data, use two periods (`..`) between coordinates. 
For example, **`@I..@-1`** translates to: *"Start at the top of my data (`@I`), and go all the way down to the row just above the bottom (`@-1`)."*

You can use standard math symbols (`+`, `-`, `*`, `/`) or built-in functions:
*   `sum(range)` - Adds everything up.
*   `mean(range)` - Calculates the average.

---

## Putting It All Together: A Translation

Let's look at a simple table and translate a formula into plain English:

| Item | Cost | Tax | Total |
| ---- | ---- | --- | ----- |
| Book | 10   | 2   |       |
| Pen  | 5    | 1   |       |
| Sum  |      |     |       |
`<!-- TBLFM: $4=($2+$3) :: @>$4=sum(@I..@-1) -->`

Here is exactly how to read that formula, piece by piece:

### Formula 1: `$4=($2+$3)`
*   **Translation:** "Make Column 4 equal to Column 2 plus Column 3."
*   **Action:** The plugin goes down every single row and adds the Cost and the Tax to fill in the Total.

### The Separator: `::`
*   **Translation:** "Wait, I have another math problem for you to do!"
*   **Action:** This simply separates multiple formulas so you can chain them together on one line.

### Formula 2: `@>$4=sum(@I..@-1)`
*   **Translation:** "Go to the last row (`@>`), specifically Column 4 (`$4`). Make that cell the sum of everything from the top of the data (`@I`) down to the row right above this one (`@-1`)."
*   **Action:** The plugin calculates your grand total at the bottom.1