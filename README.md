# Sand Simulator

A pixel art sand simulation game created with Python and Pygame.

## Features

- 500x500 pixel grid for sand simulation
- Sand falls with realistic physics
- Sand color changes every 1.5 seconds when the mouse is pressed
- Simple controls: click and hold to create sand
- Save your sand creations as PNG images

## Requirements

- Python 3.6+
- Pygame 2.0+

## Installation

1. Make sure you have Python installed
2. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

## How to Run

```
python sand_simulator.py
```

## Controls

- **Click and hold**: Create sand at mouse position
- **C key**: Clear the canvas
- **S key**: Save your creation as a PNG image

## How it works

The simulator uses a simple grid-based physics system where:
- Sand falls downward due to gravity
- If blocked, sand tries to slide diagonally
- Sand particles interact with each other to create natural-looking pile formations
- Color changes every 1.5 seconds while the mouse is pressed

Enjoy creating beautiful sand art!
