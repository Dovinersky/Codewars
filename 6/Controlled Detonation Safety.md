[Link](https://www.codewars.com/kata/63838c67bffec2000e951130) #Arrays #Algorithms #Puzzles

# Context

The Explosive Ordinance Disposal Unit of your country has found a small mine field near your town, and is planning to perform a **controlled detonation** of all of the mines. They have tasked you to write an algorithm that helps them find **all safe spots in the area**, as they want to build a temporal base in the area where all workers can rest safely.

All mines they found are of a special kind, as they only release explosive charge in **four straight lines**, each pointing at a different cardinal point **(north, south, east and west)**. However, the charge stops spreading when it crosses a tree in its path because the charge is not strong enough to burn them.

In the following diagram, `M` represents a mine, `C` represents the explosive charge released after its detonation, and `T` are the trees in the area:

```
. . . . . . .    . . . . . . .
. . . T . . .    . . . T . . .
. . . . . . .    . . . C . . .
. . T M . . . => . . T M C C C
. . . . . . .    . . . C . . .
. . . . . . .    . . . C . . .
. . . T . . .    . . . T . . .
```

# Task

Write an algorithm that, given a mine field represented as an **array of arrays** of size `M * N`, returns an **array of all safe positions** where workers can build their temporal base. As in the previous model, `'M'` represents mines, `'T'` represents trees, and `'.'` represents empty spaces where explosive charge can spread. The positions in the array **should be in "reading order"** (from left to right, and from up to down).

For example:

```
[
  ['.', '.', '.', '.', 'M'],                      . . . . M                           C C C C M
  ['.', 'M', '.', 'T', '.'],                      . M . T .                           C M C T C
  ['.', '.', 'T', '.', '.'],   ==[represents]=>   . . T . .   ==[after explosion]=>   . C T . C
  ['.', '.', 'M', '.', 'T'],                      . . M . T                           C C M C T
  ['.', '.', '.', '.', '.']                       . . . . .                           . C C . .
]
```

This should return one of the two following arrays, depending on the language. Check sample test cases for more information. Also, **trees don't count as safe positions**.

-   `[(2,0), (2,3), (4,0), (4,3), (4,4)] //For Python`
-   `[[2,0], [2,3], [4,0], [4,3], [4,4]] //For JS and other languages`

Return an empty array if there are no safe positions.

# Note

Mines will not stop explosive charge from spreading as trees do, and explosive charge won't erase mines it finds in its path. **Make sure you never override any mines in the field.**

***
# Solutions
#js 
```js
function safe_mine_field(mine_field) {
    let rows = Array.from(mine_field);
    let result = [];

    let explode = (rowIndex, columnIndex) => {
        let checkCell = (cell) => cell == "T" || cell == "M" || !cell;

        // explode north
        for (let index = rowIndex; index > 0; index--) {
            if (checkCell(rows[index - 1][columnIndex])) break;
            rows[index - 1][columnIndex] = "C";
        }

        //explode south
        for (let index = rowIndex; index < rows.length - 1; index++) {
            if (checkCell(rows[index + 1][columnIndex])) break;
            rows[index + 1][columnIndex] = "C";
        }

        // explode west
        for (let index = columnIndex; index > 0; index--) {
            if (checkCell(rows[rowIndex][index - 1])) break;
            rows[rowIndex][index - 1] = "C";
        }

        // explode east
        for (let index = columnIndex; index < rows.length - 1; index++) {
            if (checkCell(rows[rowIndex][index + 1])) break;
            rows[rowIndex][index + 1] = "C";
        }
    };

    // explode all mines
    rows.forEach((row, rowIndex) => {
        row.forEach((cell, columnIndex) => {
            if (cell == "M") explode(rowIndex, columnIndex);
        });
    });

    // get safe spaces
    rows.forEach((row, rowIndex) => {
        row.forEach((cell, columnIndex) => {
            if (cell == ".") result.push([rowIndex, columnIndex]);
        });
    });

    return result;
}
```