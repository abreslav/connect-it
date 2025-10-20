Connect-it!
===========

This is a game inspired by the tile-matching puzzle mechanics and the "2048" mechanics.

# Game field

The field has 5x5 cells.
Each cell 
- has a number which is a power of 2
- each number is displayed in a colored circle
- colors
  - number are white
  - circles are colored according to the numbers (like in 2048)

Numbers higher than 512 are displayed 1K, 2K, ... 512K, then 1M, 2M, etc
Colors of, e.g. 2, 2K and 2M are the same.
Circles around numbers 1K .. 512K have an extra white rim to distinguish them more from 2 and 2M

# Initial state

The field is filled with random powers of 2 between 2 and 32. If there are no adjacent pairs of the same value, up to 3 cells should be changed to introduce some such pairs.

# Gameplay

Starting with any cell, the user can drag the mouse/their finger through the cells of the same value adjacent to it. While dragging, the affected cells of the same value as the starting cell are connected by lines of the same color as the circles in the cells. A preview above the game field displays the biggest whole power of 2 the sum of all the affected cells has reached. If the user drags the pointer back along the connecting line, it cancels the line (as if the adjacent cell was never affected). When the user releases the pointer, the affected cells all collapse into the last affected one and it gets replaced by the biggest power of 2 their sum has reached. The collapsed cells disappear and the cells above them slide directly downward. the vacant spots are filled with new values generated according to the algorithm below.

## Rules of adjacency

Two cells are adjacent if they share a side or a corner. Example:

NNNNN
NAAAN
NAXAN
NAAAN
NNNNN

The cells marked "A" are adjacent to the cell marked "X", while the cells marked "N" aren't

The field is flat, meaning the the cells at the edges have fewer adjacent cells, e.g.:

NNNNN
AANNN
XANNN
AANNN
NNNNN


## Biggest whole power of 2 calculation examples

The value of the reulting cell after N cells of value X collapse is the biggest power of 2 that is less or equal to N * X, i.e. R = max(2^k | where 2^k <= N * X)

- if the user connects 2 cells of value 4, they collapse into an 8
- if the user connects 3 cells of value 4, they collapse into an 8 (4+4 = 8, is the biggest power of 2 <= 4+4+4=12)
- 4 cells of value 4 collapse into a 16
- if the user only touches one cell, nothing happens, which is equivalent with it "collapsing into itself"

## New cell generation algorithm

When the cells are vacated by some cells collapsing and then the cells above them dropping down, the vacant cells are populated with numbers according to the following rules:
- for each cell, generate random while powers of 2 numbers between 2 and 32
- if the resulting game field has no adjacent cells of the same value (the game can not progress), replace one of the newly generated cells with a value equal to a random cell adjacent to it

## Game reset button

The user can reset the game field by pressing "Reset". In this case, a new initial state is generated (and immediately saved).


# Animations and smoothness

It's important to maintain visual continuity for the user.
The field should always update smoothly: no blinking, and all changes should happen visually:
- when the user is dragging the pointer, the lines between cells appear gradually (without much delay)
- when the cells collapse, they should move along the connecting line from the first touched to the last making room for the ones above them to fall down also visibly
- the newly generated values appear in vacated cells simlutaneously with an animation: the circles started small in the center and grow to full size
- the whole time of the collapse, fall and replace animation should be about 0.5 sec


# Users and data storage

There's only one user, no registration or sign-in is required.

The playing field is saved upon each move so that if the user closes the game at any point, they can then continue from where they left off.

# Tech Stack

- This is a single-page application (single HTML file) written in HTML, CSS and JavaScript
- no extra assets used
- it should work on desktops and mobiles alike
- data is stored in the browser's local storage
