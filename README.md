# Game of Life (pygame learning project)

A simple implementation of **Conway’s Game of Life** written in Python using **pygame**.  
Inspired by this amazing video where people have built entire computers with this logic:  
https://www.youtube.com/watch?v=Kk2MH9O4pXY

This project is a **learning exercise**, focused on understanding:
- game loops  
- rendering grids  
- separating simulation logic from rendering  
- basic performance optimizations using spatial partitioning (chunks)

---

## Game rules 

The Game of Life is, briefly, a two-dimensional cellular automata universe governed by a simple set of birth, death and survival rules.

- survival: if a live cell has two or three live neighbors, it survives.
- death: if a live cell has less than two or more than three live neighbors, it dies.
- birth: if a dead cell has exactly three live neighbors, it is born. 

---

## Usage

### Manual editing mode (default)

When started **without** the `--random` flag, the program opens in **editing mode**:

- Click on cells to toggle them alive or dead
- Grid lines are shown to make placement easier
- Press **SPACE** to start the simulation
- Press **N** to advance **one generation** and enter pause mode

This mode is useful for manually constructing patterns and stepping through their evolution.

---

### Random start

If the `--random` flag is provided, the grid is filled randomly with the given density (a float between `0` and `1`) and the simulation starts in paused state (press SPACE to start):

```bash
python3 gameoflife.py --random 0.15
```
---

## Controls

### Keyboard

- **SPACE**  
  Toggle between:
  - Edit → Run  
  - Run → Pause  
  - Pause → Run

- **N**  
  Advance the simulation by **one generation**  
  - From **Edit mode**: enters Pause mode and advances once  
  - From **Pause mode**: advances one generation

- **Arrow keys**  
  Pan the view when the grid is larger than the window:
  - ← → move left / right  
  - ↑ ↓ move up / down

- **ESC**  
  Quit the application

---

## Mouse

### Left click (Edit mode only)
- Toggle a cell **alive / dead**

---

## Display behavior

- The application opens in **borderless fullscreen**
- Grid size automatically adapts to the screen resolution
- You can **pan the camera** using the arrow keys
- The simulation continues running even when cells are outside the visible area

---

## Chunk-based optimization

To improve performance on large grids, the simulation uses **chunk-based evaluation**.

### Concept

- The grid is divided into square chunks (e.g. `32 × 32` cells)
- Only chunks that contain **live cells** are processed
- Neighboring chunks are also evaluated to handle edge interactions
- After each step, active chunks are recalculated

This significantly reduces unnecessary computation when most of the grid is empty.

---

## Internal structure

### Important functions

| Function | Purpose |
|--------|---------|
| `make_grid()` | Create an empty grid |
| `populate_grid_random()` | Fill grid with random live cells |
| `count_live_neighbor_cells()` | Apply Conway’s rules |
| `active_chunks_from_grid()` | Find which chunks contain live cells |
| `expand_active_chunks()` | Include neighboring chunks |
| `iterate_chunk_cells()` | Iterate safely over chunk cells |
| `step()` | Advance the simulation by one generation |

---

## Simulation flow

1. User edits the grid or generates a random state  
2. Active chunks are detected  
3. Each frame:
   - Only relevant chunks are evaluated  
   - A new grid is produced  
   - Active chunks are recalculated  
4. The grid is rendered to the screen  

---

## Design philosophy

- Readable over clever  
- Predictable behavior  
- Explicit state transitions  
- Suitable for learning and experimentation  

This is **not** meant to be the fastest possible implementation.

## Requirements

- Python **3.8+**
- `pygame`

Install pygame with:

```bash
pip install pygame
```
or
```bash
apt install python3-pygame
```
---

## Nota bene

John Conway, the creator of the Game of Life, explaining the algorithm himself:  
https://www.youtube.com/watch?v=R9Plq-D1gEk
