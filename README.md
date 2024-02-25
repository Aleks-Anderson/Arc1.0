import pygame
import sys

# Инициализация Pygame
pygame.init()

# Параметры окна
WIDTH, HEIGHT = 800, 600
FPS = 60

# Цвета
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Создание окна
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Арканоид")

# Создание платформы
platform_width, platform_height = 100, 20
platform = pygame.Rect(WIDTH // 2 - platform_width // 2, HEIGHT - 50, platform_width, platform_height)

# Создание мяча
ball_radius = 10
ball = pygame.Rect(WIDTH // 2 - ball_radius, HEIGHT // 2 - ball_radius, ball_radius * 2, ball_radius * 2)
ball_speed = [5, -5]

# Основной цикл игры
clock = pygame.time.Clock()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Обработка движения платформы
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and platform.left > 0:
        platform.x -= 5
    if keys[pygame.K_RIGHT] and platform.right < WIDTH:
        platform.x += 5

    # Обновление положения мяча
    ball.x += ball_speed[0]
    ball.y += ball_speed[1]

    # Обработка столкновения мяча с границами экрана
    if ball.left <= 0 or ball.right >= WIDTH:
        ball_speed[0] = -ball_speed[0]
    if ball.top <= 0:
        ball_speed[1] = -ball_speed[1]

    # Обработка столкновения мяча с платформой
    if ball.colliderect(platform):
        ball_speed[1] = -ball_speed[1]

    # Отрисовка элементов
    screen.fill(BLACK)
    pygame.draw.rect(screen, WHITE, platform)
    pygame.draw.ellipse(screen, WHITE, ball)

    # Обновление экрана
    pygame.display.flip()

    # Установка FPS
    clock.tick(FPS)
