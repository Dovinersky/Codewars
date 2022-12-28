[Link](https://www.codewars.com/kata/5c230f017f74a2e1c300004f) #Algorithms

# Story

POTUS thinks he is all alone in the White House on Christmas Eve.

But is he?

The White House has a wall-penetrating radar security system that sees everything.

# Kata Task

Process the radar image.

Return `true` if POTUS really is home alone.

## Legend

-   `#` walls
-   `X` POTUS
-   `o` elves

# Notes

-   The house corners are square only
-   The radar can also see into nearby buildings

# Examples

## ex1

```
   o                o        #######
 ###############             #     #
 #             #        o    #     #
 #  X          ###############     #
 #                                 #
 ###################################
```

All alone --> `true`

## ex2

```
#################
#     o         #   o
#          ######        o
####       #                
   #       ###################
   #                         #
   #                  X      #
   ###########################
```

All alone --> `false`

***
# Solutions
#js
```js
function allAlone(house) {
    let rows = Array.from(house);
    let coordsForCheckAround = [];

    let getCoordObject = (rowIndex, columnIndex) =>
        new Object({
            rowIndex: rowIndex ? rowIndex : 0,
            columnIndex: columnIndex ? columnIndex : 0,
        });

    // if cell is "X" or "#" then check result is false
    // if cell is "o" then the result is true
    // if cell is empty space then set this cell as "X" and add the cell coords for new checks around; check result is false
    let checkCellForElf = (rowIndex, columnIndex) => {
        let cell = rows[rowIndex]?.[columnIndex];
        if (cell == "X" || cell == "#" || !cell) return false;
        if (cell == "o") return true;

        rows[rowIndex][columnIndex] = "X";
        coordsForCheckAround.push(getCoordObject(rowIndex, columnIndex));
        return false;
    };

    // find Potus coords
    let breakRequired = false;
    for (let rowIndex = 0; rowIndex < rows.length; rowIndex++) {
        if (breakRequired) break;

        for (let colIndex = 0; colIndex < rows[rowIndex].length; colIndex++) {
            
            if (rows[rowIndex][colIndex] == "X") {
                coordsForCheckAround.push(
                    getCoordObject(rowIndex, colIndex)
                );
                breakRequired = true;
                break;
            }
        }
    }

    let cell;
    while (coordsForCheckAround.length > 0) {
        // check for elves around and add new cells for checks
        cell = coordsForCheckAround.shift();
        // north
        if (checkCellForElf(cell.rowIndex - 1, cell.columnIndex))
            return false;

        // north - east
        if (checkCellForElf(cell.rowIndex - 1, cell.columnIndex + 1))
            return false;

        // east
        if (checkCellForElf(cell.rowIndex, cell.columnIndex + 1))
            return false;

        // south - east
        if (checkCellForElf(cell.rowIndex + 1, cell.columnIndex + 1))
            return false;

        // south
        if (checkCellForElf(cell.rowIndex + 1, cell.columnIndex))
            return false;

        // south - west
        if (checkCellForElf(cell.rowIndex + 1, cell.columnIndex - 1))
            return false;

        // west
        if (checkCellForElf(cell.rowIndex, cell.columnIndex - 1))
            return false;

        // north - west
        if (checkCellForElf(cell.rowIndex - 1, cell.columnIndex - 1))
            return false;
    }
    return true;
}
```
