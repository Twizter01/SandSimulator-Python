import pygame
import sys
import random
import time
import math
from pygame.locals import *

# Initialize pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 500, 500
PIXEL_SIZE = 2
GRID_WIDTH = WIDTH // PIXEL_SIZE
GRID_HEIGHT = HEIGHT // PIXEL_SIZE
FPS = 60

# Colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
SAND_COLORS = [
    (255, 204, 102),  # Light sand
    (232, 158, 57),   # Orange sand
    (204, 142, 53),   # Dark orange sand
    (176, 127, 51),   # Brown sand
    (240, 211, 165),  # Pale sand
    (225, 173, 80),   # Golden sand
]

# Setup display
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption('Sand Simulator')
clock = pygame.time.Clock()

# Grid setup
grid = [[0 for _ in range(GRID_HEIGHT)] for _ in range(GRID_WIDTH)]

# Sand color management
current_color_index = 0
color_change_interval = 1.5  # seconds
last_color_change_time = time.time()
mouse_pressed_time = 0

def get_current_sand_color():
    return SAND_COLORS[current_color_index]

def update_sand_color():
    global current_color_index, last_color_change_time
    current_time = time.time()
    if current_time - last_color_change_time >= color_change_interval:
        current_color_index = (current_color_index + 1) % len(SAND_COLORS)
        last_color_change_time = current_time

def add_sand(x, y):
    grid_x = x // PIXEL_SIZE
    grid_y = y // PIXEL_SIZE
    
    # Ensure coordinates are within grid bounds
    if 0 <= grid_x < GRID_WIDTH and 0 <= grid_y < GRID_HEIGHT:
        # Add sand with current color index
        grid[grid_x][grid_y] = current_color_index + 1  # +1 because 0 means empty

def update_sand():
    # Process from bottom to top to prevent sand from "teleporting" down
    for y in range(GRID_HEIGHT - 2, -1, -1):
        for x in range(GRID_WIDTH):
            if grid[x][y] > 0:  # If there's sand at this position
                # Check if the space below is empty
                if y + 1 < GRID_HEIGHT and grid[x][y + 1] == 0:
                    grid[x][y + 1] = grid[x][y]  # Move sand down
                    grid[x][y] = 0  # Clear current position
                
                # If blocked below, try to move diagonally down
                elif y + 1 < GRID_HEIGHT:
                    # Try to move randomly to left or right diagonal
                    random_direction = random.choice([-1, 1])
                    try_x = x + random_direction
                    
                    # Check if the diagonal position is valid and empty
                    if 0 <= try_x < GRID_WIDTH and grid[try_x][y + 1] == 0:
                        grid[try_x][y + 1] = grid[x][y]  # Move sand diagonally
                        grid[x][y] = 0  # Clear current position
                    # Try the other diagonal if the first one is blocked
                    elif 0 <= x - random_direction < GRID_WIDTH and grid[x - random_direction][y + 1] == 0:
                        grid[x - random_direction][y + 1] = grid[x][y]
                        grid[x][y] = 0

def draw_grid():
    screen.fill(BLACK)
    for y in range(GRID_HEIGHT):
        for x in range(GRID_WIDTH):
            if grid[x][y] > 0:
                # Get the appropriate color for this sand grain
                color_index = grid[x][y] - 1  # -1 because we added +1 when storing
                sand_color = SAND_COLORS[color_index % len(SAND_COLORS)]
                
                # Draw the sand pixel
                pygame.draw.rect(
                    screen, 
                    sand_color, 
                    (x * PIXEL_SIZE, y * PIXEL_SIZE, PIXEL_SIZE, PIXEL_SIZE)
                )

def clear_canvas():
    global grid
    grid = [[0 for _ in range(GRID_HEIGHT)] for _ in range(GRID_WIDTH)]

def save_canvas():
    timestamp = int(time.time())
    filename = f"sand_creation_{timestamp}.png"
    pygame.image.save(screen, filename)
    print(f"Creation saved as {filename}")

def main():
    global mouse_pressed_time
    
    running = True
    mouse_pressed = False
    
    while running:
        for event in pygame.event.get():
            if event.type == QUIT:
                running = False
            elif event.type == KEYDOWN:
                if event.key == K_c:
                    clear_canvas()
                elif event.key == K_s:
                    save_canvas()
            elif event.type == MOUSEBUTTONDOWN:
                if event.button == 1:  # Left mouse button
                    mouse_pressed = True
                    mouse_pressed_time = time.time()
            elif event.type == MOUSEBUTTONUP:
                if event.button == 1:  # Left mouse button
                    mouse_pressed = False
        
        # Add sand at mouse position if pressed
        if mouse_pressed:
            mouse_x, mouse_y = pygame.mouse.get_pos()
            
            # Create a small cluster of sand around the mouse position for better control
            add_sand(mouse_x, mouse_y)
            add_sand(mouse_x + random.randint(-2, 2), mouse_y + random.randint(-2, 2))
            add_sand(mouse_x + random.randint(-2, 2), mouse_y + random.randint(-2, 2))
            
            # Check if it's time to change the sand color
            if mouse_pressed:
                current_time = time.time()
                if current_time - mouse_pressed_time >= color_change_interval:
                    update_sand_color()
                    mouse_pressed_time = current_time
        
        # Update sand positions
        update_sand()
        
        # Draw everything
        draw_grid()
        
        # Display controls at the bottom
        font = pygame.font.SysFont('Arial', 14)
        controls_text = "Controls: Click and hold to create sand | C to clear | S to save"
        text_surface = font.render(controls_text, True, WHITE)
        text_rect = text_surface.get_rect(center=(WIDTH // 2, HEIGHT - 15))
        screen.blit(text_surface, text_rect)
        
        pygame.display.flip()
        clock.tick(FPS)
    
    pygame.quit()
    sys.exit()

if __name__ == "__main__":
    main()
