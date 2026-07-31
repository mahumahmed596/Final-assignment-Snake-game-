import pygame
import random

# Initialize Pygame
pygame.init()

# Screen Settings
WIDTH = 800
HEIGHT = 600
CELL_SIZE = 20

screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Python Snake Game")

# Colors
BACKGROUND = (30, 30, 30)
SNAKE = (50, 205, 50)
FOOD = (255, 0, 0)
TEXT = (255, 255, 0)

# Clock
clock = pygame.time.Clock()
FPS = 15

# Font
font = pygame.font.SysFont("Verdana", 28)


def show_score(score):
    text = font.render("Score: " + str(score), True, TEXT)
    screen.blit(text, (10, 10))


def create_food():
    x = random.randint(0, (WIDTH // CELL_SIZE) - 1) * CELL_SIZE
    y = random.randint(0, (HEIGHT // CELL_SIZE) - 1) * CELL_SIZE
    return (x, y)


def game():

    snake = [(200, 200)]
    direction = "RIGHT"

    food = create_food()

    score = 0
    running = True

    while running:

        for event in pygame.event.get():

            if event.type == pygame.QUIT:
                running = False

            if event.type == pygame.KEYDOWN:

                if event.key == pygame.K_UP and direction != "DOWN":
                    direction = "UP"

                elif event.key == pygame.K_DOWN and direction != "UP":
                    direction = "DOWN"

                elif event.key == pygame.K_LEFT and direction != "RIGHT":
                    direction = "LEFT"

                elif event.key == pygame.K_RIGHT and direction != "LEFT":
                    direction = "RIGHT"

        x, y = snake[0]

        if direction == "UP":
            y -= CELL_SIZE

        elif direction == "DOWN":
            y += CELL_SIZE

        elif direction == "LEFT":
            x -= CELL_SIZE

        elif direction == "RIGHT":
            x += CELL_SIZE

        head = (x, y)

        # Wall Collision
        if x < 0 or x >= WIDTH or y < 0 or y >= HEIGHT:
            break

        # Self Collision
        if head in snake:
            break

        snake.insert(0, head)

        # Food Collision
        if head == food:

            score += 1

            while True:
                food = create_food()
                if food not in snake:
                    break
        else:
            snake.pop()

        # Draw Background
        screen.fill(BACKGROUND)

        # Draw Food
        pygame.draw.rect(screen, FOOD, (food[0], food[1], CELL_SIZE, CELL_SIZE))

        # Draw Snake
        for part in snake:
            pygame.draw.rect(screen, SNAKE, (part[0], part[1], CELL_SIZE, CELL_SIZE))

        show_score(score)

        pygame.display.update()

        clock.tick(FPS)

    pygame.quit()


if __name__ == "__main__":
    game()
