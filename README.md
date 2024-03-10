import pygame
import random

# Initialize Pygame
pygame.init()

# Set the screen dimensions
screen_width = 600
screen_height = 400

# Set the colors
white = (255, 255, 255)
black = (0, 0, 0)
red = (255, 0, 0)

# Set the clock
clock = pygame.time.Clock()

# Set the font
font_style = pygame.font.SysFont(None, 30)


# Function to draw the snake
def draw_snake(snake_block, snake_list):
    for x in snake_list:
        pygame.draw.rect(screen, black, [x[0], x[1], snake_block, snake_block])


# Function to display message
def message(msg, color):
    mesg = font_style.render(msg, True, color)
    screen.blit(mesg, [screen_width / 6, screen_height / 3])


# Set the game screen
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Snake Game")

# Set the snake parameters
snake_block = 10
snake_speed = 15

# Set the initial position of snake
x1 = screen_width / 2
y1 = screen_height / 2
x1_change = 0
y1_change = 0

# Initialize the snake list
snake_list = []
snake_length = 1

# Set the initial position of food
foodx = round(random.randrange(0, screen_width - snake_block) / 10.0) * 10.0
foody = round(random.randrange(0, screen_height - snake_block) / 10.0) * 10.0

# Set the game over condition
game_over = False

# Set the game loop
while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                x1_change = -snake_block
                y1_change = 0
            elif event.key == pygame.K_RIGHT:
                x1_change = snake_block
                y1_change = 0
            elif event.key == pygame.K_UP:
                y1_change = -snake_block
                x1_change = 0
            elif event.key == pygame.K_DOWN:
                y1_change = snake_block
                x1_change = 0

    # Update the position of snake
    x1 += x1_change
    y1 += y1_change

    # Check if snake hits the boundary
    if x1 >= screen_width or x1 < 0 or y1 >= screen_height or y1 < 0:
        game_over = True

    screen.fill(white)
    pygame.draw.rect(screen, red, [foodx, foody, snake_block, snake_block])

    snake_head = []
    snake_head.append(x1)
    snake_head.append(y1)
    snake_list.append(snake_head)
    if len(snake_list) > snake_length:
        del snake_list[0]

    for x in snake_list[:-1]:
        if x == snake_head:
            game_over = True

    draw_snake(snake_block, snake_list)

    pygame.display.update()

    if x1 == foodx and y1 == foody:
        foodx = round(random.randrange(0, screen_width - snake_block) / 10.0) * 10.0
        foody = round(random.randrange(0, screen_height - snake_block) / 10.0) * 10.0
        snake_length += 1

    clock.tick(snake_speed)

message("You lost! Press Q to Quit or C to Play Again", red)
pygame.display.update()
pygame.quit()
quit()
