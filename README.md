import pygame
import sys
import time
import random
import numpy as np

# --- Configuration ---
WIDTH, HEIGHT = 800, 600
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (200, 0, 0)
BLUE = (0, 100, 255)
GREEN = (0, 180, 0)
TIME_LIMIT = 15 

# Initialize Pygame
pygame.init()
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Accuracy Drawing Game - Improved!")
clock = pygame.time.Clock()
font = pygame.font.Font(None, 40)
title_font = pygame.font.Font(None, 60)

# --- Target Generators ---
# These functions draw thick templates for the player to trace inside of.
def draw_car(surf):
    pygame.draw.rect(surf, BLACK, (200, 300, 400, 100), 30) 
    pygame.draw.rect(surf, BLACK, (300, 200, 200, 100), 30) 
    pygame.draw.circle(surf, BLACK, (250, 400), 40, 30)     
    pygame.draw.circle(surf, BLACK, (550, 400), 40, 30)

def draw_house(surf):
    pygame.draw.rect(surf, BLACK, (250, 300, 300, 250), 30) # Base
    pygame.draw.polygon(surf, BLACK, [(200, 300), (400, 100), (600, 300)], 30) # Roof

def draw_tree(surf):
    pygame.draw.rect(surf, BLACK, (350, 300, 100, 250), 30) # Trunk
    pygame.draw.circle(surf, BLACK, (400, 200), 150, 30)    # Leaves

def draw_sword(surf):
    pygame.draw.line(surf, BLACK, (200, 500), (600, 100), 40) # Blade
    pygame.draw.line(surf, BLACK, (150, 450), (250, 550), 40) # Crossguard

targets = {
    "CAR": draw_car,
    "HOUSE": draw_house,
    "TREE": draw_tree,
    "SWORD": draw_sword
}

def get_new_target():
    name = random.choice(list(targets.keys()))
    surf = pygame.Surface((WIDTH, HEIGHT))
    surf.fill(WHITE)
    targets[name](surf)
    return name, surf

# --- Game Variables ---
game_state = "START" 
start_time = 0
time_used = 0
final_score = 0
accuracy_pct = 0

canvas = pygame.Surface((WIDTH, HEIGHT))
canvas.fill(WHITE)
drawing = False
last_pos = None

# Get the first target
current_target_name, target_img = get_new_target()

def calculate_score(user_canvas, target_surface, time_taken):
    """Calculates a fairer score based on precision and a forgiving coverage curve."""
    user_arr = pygame.surfarray.array2d(user_canvas)
    target_arr = pygame.surfarray.array2d(target_surface)

    white_int = user_canvas.map_rgb(WHITE)

    user_drawn = (user_arr != white_int)
    target_drawn = (target_arr != white_int)

    overlap = np.logical_and(user_drawn, target_drawn).sum()
    user_total = user_drawn.sum()
    target_total = target_drawn.sum()

    if user_total == 0:
        return 0, 0 # They drew nothing

    # Precision: Did they stay inside the lines? (Penalty for scribbling outside)
    precision = overlap / user_total 

    # Recall: Did they draw enough of the shape? 
    # Since they draw thin lines inside thick target boxes, they will never cover 100%.
    # We multiply by 5 so that covering just 20% of the thick target pixels counts as a "full" drawing.
    recall = min(1.0, (overlap / target_total) * 5.0)

    # Average them out for a fair accuracy score
    acc_score = ((precision * 0.6) + (recall * 0.4)) * 100

    # Time score: up to 15 bonus points for finishing fast
    time_score = max(0, ((TIME_LIMIT - time_taken) / TIME_LIMIT) * 15)
    
    # Weight accuracy out of 85, time out of 15 to get score out of 100
    total_score = (acc_score * 0.85) + time_score
    
    return min(100, round(total_score)), round(acc_score)

def draw_text(text, font, color, surface, x, y, center=True):
    textobj = font.render(text, True, color)
    textrect = textobj.get_rect()
    if center:
        textrect.center = (x, y)
    else:
        textrect.topleft = (x, y)
    surface.blit(textobj, textrect)

# --- Main Game Loop ---
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                if game_state == "START":
                    game_state = "DRAWING"
                    canvas.fill(WHITE) 
                    start_time = time.time()
                
                elif game_state == "DRAWING":
                    time_used = time.time() - start_time
                    final_score, accuracy_pct = calculate_score(canvas, target_img, time_used)
                    game_state = "SCORE"
                
                elif game_state == "SCORE":
                    # Pick a new random target for the next round
                    current_target_name, target_img = get_new_target()
                    game_state = "START" 

        # Drawing mechanics
        if game_state == "DRAWING":
            if event.type == pygame.MOUSEBUTTONDOWN:
                if event.button == 1: 
                    drawing = True
                    last_pos = event.pos
            elif event.type == pygame.MOUSEBUTTONUP:
                if event.button == 1:
                    drawing = False
            elif event.type == pygame.MOUSEMOTION:
                if drawing:
                    current_pos = event.pos
                    # Draw thicker lines (radius 10) so the user has an easier time covering area
                    pygame.draw.line(canvas, BLACK, last_pos, current_pos, 10)
                    last_pos = current_pos

    # --- Screen Updates ---
    screen.fill(WHITE)

    if game_state == "START":
        target_img.set_alpha(100) # Make it faint
        screen.blit(target_img, (0, 0))
        target_img.set_alpha(255) 
        
        draw_text(f"Target: A {current_target_name}", title_font, BLUE, screen, WIDTH/2, HEIGHT/2 - 50)
        draw_text(f"You will have {TIME_LIMIT} seconds.", font, BLACK, screen, WIDTH/2, HEIGHT/2)
        draw_text("Press SPACE to start drawing!", font, GREEN, screen, WIDTH/2, HEIGHT/2 + 50)

    elif game_state == "DRAWING":
        screen.blit(canvas, (0, 0))
        
        time_elapsed = time.time() - start_time
        time_left = max(0, TIME_LIMIT - time_elapsed)
        
        draw_text(f"Time: {time_left:.1f}", font, RED, screen, 80, 30, center=False)
        draw_text("Press SPACE when finished", font, BLUE, screen, WIDTH - 350, 30, center=False)

        if time_left <= 0:
            time_used = TIME_LIMIT
            final_score, accuracy_pct = calculate_score(canvas, target_img, time_used)
            game_state = "SCORE"

    elif game_state == "SCORE":
        screen.blit(canvas, (0, 0)) 
        
        overlay = pygame.Surface((WIDTH, HEIGHT))
        overlay.set_alpha(220)
        overlay.fill(WHITE)
        screen.blit(overlay, (0, 0))

        # Show the target faintly behind their drawing so they can see how they did
        target_img.set_alpha(50)
        screen.blit(target_img, (0, 0))
        target_img.set_alpha(255)

        draw_text("TIME'S UP!" if time_used >= TIME_LIMIT else "FINISHED!", title_font, RED, screen, WIDTH/2, HEIGHT/2 - 100)
        draw_text(f"Accuracy: {accuracy_pct}%", font, BLACK, screen, WIDTH/2, HEIGHT/2 - 20)
        draw_text(f"Time Used: {time_used:.1f}s", font, BLACK, screen, WIDTH/2, HEIGHT/2 + 20)
        draw_text(f"TOTAL SCORE: {final_score} / 100", title_font, GREEN, screen, WIDTH/2, HEIGHT/2 + 80)
        draw_text("Press SPACE to play again", font, BLUE, screen, WIDTH/2, HEIGHT/2 + 150)

    pygame.display.flip()
    clock.tick(60)

pygame.quit()
sys.exit()
