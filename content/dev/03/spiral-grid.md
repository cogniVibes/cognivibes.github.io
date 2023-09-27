---
author: Ayush Saha
title: "Printing an Outward Opening Spiral Grid"
cover:
  image: "/dev/03/spiral-grid-cover.png"
date: 2023-08-15
summary: "Learn to print an outward opening spiral grid of numbers based on user input."
tags:
    - python
    - programming
    - printing patterns
math: true
---

Let's create an outward opening spiral grid of numbers using Python. This pattern involves arranging numbers in a grid format that spirals outwards. We will do this by assigning coordinates to each number.

{{< figure
  src="/dev/03/spiral-grid.png"
  caption="Fig 1. Outward Opening Spiral Grid"
  height="300"
  width="300"
  align="center"
>}}

## The Code

```python
def print_spiral_grid(n):
    # Initialize the grid with zeros
    grid = [[0] * n for _ in range(n)]

    num = n * n
    coords = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # Directions: right, down, left, up
    direction_index = 0
    row, col = 0, 0

    for _ in range(n * n):
        grid[row][col] = num
        num -= 1

        # Calculate the next position
        next_row = row + coords[direction_index][0]
        next_col = col + coords[direction_index][1]

        # Check if the next position is within bounds and not yet filled
        if 0 <= next_row < n and 0 <= next_col < n and grid[next_row][next_col] == 0:
            row, col = next_row, next_col
        else:
            direction_index = (direction_index + 1) % 4
            row += coords[direction_index][0]
            col += coords[direction_index][1]

    # Print the grid
    for row in grid:
        print(" ".join(str(num).zfill(2) for num in row))

# Example usage
n = int(input("Enter the value of n: "))
print_spiral_grid(n)
```

The code defines a Python function `print_spiral_grid(n)` that generates and prints a spiral grid of numbers from $n^2$ down to 1, following a clockwise pattern. Let's break down the code step by step:

## Initialization

```python
grid = [[0] * n for _ in range(n)]
```

This line initializes a 2D grid (list of lists) with dimensions $n$ by $n$ and fills it with zeros. This grid will hold the spiral pattern of numbers.

```python
num = n * n
```

This initializes a variable `num` with the value $n^2$. This will be used to populate the grid with decreasing numbers in a spiral manner.

```python
coords = [(0, 1), (1, 0), (0, -1), (-1, 0)]
```

This defines a list `coords` that contains four tuples representing directional changes. These tuples represent moving right, down, left, and up respectively.

```python
direction_index = 0
row, col = 0, 0
```

`direction_index` keeps track of the current direction (initially set to 0 for moving right). `row` and `col` store the current position in the grid.

## Assigning Coordinates

The loop iterates from 1 to $n^2$, filling the grid in a spiral pattern with decreasing numbers. Here's what happens inside the loop:

```python
grid[row][col] = num
num -= 1
```

The current position in the grid is set to the current value of `num`, and `num` is decremented by 1 for the next iteration.

```python
next_row = row + coords[direction_index][0]
next_col = col + coords[direction_index][1]
```

Calculate the next position based on the current direction.

```python
if 0 <= next_row < n and 0 <= next_col < n and grid[next_row][next_col] == 0:
    row, col = next_row, next_col
else:
    direction_index = (direction_index + 1) % 4
    row += coords[direction_index][0]
    col += coords[direction_index][1]
```

Check if the next position is within bounds of the grid and hasn't been filled already. If both conditions are met, move to the next position. Otherwise, change the direction (clockwise) and update the current position accordingly.


## Printing the Grid

After filling the grid with the spiral pattern, the code prints the grid using nested loops:

```python
for row in grid:
    print(" ".join(str(num).zfill(2) for num in row))
```

It iterates through each row in the grid, formatting the numbers to have leading zeros if needed, and printing them with spaces in between.

## Tre Program in Action

Here is sample output that we get upon executing the program.

```console
Enter the value of n: 5
25 24 23 22 21
10 09 08 07 20
11 02 01 06 19
12 03 04 05 18
13 14 15 16 17
```