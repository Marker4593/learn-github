# learn-github
import random

import pygame
from random import randint
import pygame.time

# กำหนดขนาดหน้าต่าง
WINDOW_WIDTH = 1200
WINDOW_HEIGHT = 800

pygame.init()
display_surface = pygame.display.set_mode((WINDOW_WIDTH, WINDOW_HEIGHT))

pygame.display.set_caption("Jumping")
Icon = pygame.image.load("Keroro-3 (1).jpg")
pygame.display.set_icon(Icon)

spaceship = pygame.image.load("spaceship2.png")
meteorite = pygame.image.load("meteorite.png")
Quit_button = pygame.image.load("Quit_button.png")
Play_button = pygame.image.load("play_button.png")
Retry_button = pygame.image.load("Retry_button.png")
Home_background = pygame.image.load("background2.png")
Space_slug_right = pygame.image.load("slug.png")
Space_slug_left = pygame.image.load("slug.png")
Enemy_spaceship = pygame.image.load("Enemy_spaceship.png")

# ปุ่มออก
quit_button_x = 600
quit_button_y = 400
quit_button_width = 200
quit_button_height = 100
# ปุ่มเริ่มต้น
play_button_x = 500
play_button_y = 400
play_button_width = 200
play_button_height = 100

# ปุ่มเริ่มใหม่
retry_button_x = 350
retry_button_y = 400
retry_button_width = 200
retry_button_height = 100

# พื้นหลังหน้าหลัก
home_background_x = 400
home_background_y = 80

# ยานอวกาศ
spaceship_x = 550
spaceship_y = 550
spaceship_width = 150
spaceship_height = 100
spaceship_speed = 4

# อุกกาบาต
num_meteorites = 4
meteorite_width = 25
meteorite_height = 25
Min_speeds = 5
Max_speeds = 12
meteorites_speeds = [randint(Min_speeds, Max_speeds) for _ in range(num_meteorites)]
meteorites = [(randint(1,WINDOW_WIDTH),0) for _ in range(num_meteorites)]

status_color = pygame.Color("gray")
status_x = 0
status_y = 0
status_width = 200
status_height = 50

hp_color = pygame.Color("green")
hp_x = 10
hp_y = 10
hp_width = 180
hp_height = 10
reduce_hp = False
damage = 0.5

Bullets_right = []
Bullets_left = []


enemy_spaceship_height = 100
enemy_spaceship_width = 200
enemy_spaceship_speed = 1
num_enemy = 1









Game_Home = True
running = False
Lost = False

while Game_Home:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            Game_Home = False
            break
        if event.type == pygame.MOUSEBUTTONDOWN:
            mouse_pos = pygame.mouse.get_pos()
            if play_button_x <= mouse_pos[0] <= play_button_x + play_button_width and play_button_y <= mouse_pos[1] <= play_button_y + play_button_height:
                running = True
                Game_Home = False


    display_surface.fill((0,0,0))
    display_surface.blit(Home_background,(home_background_x,home_background_y))
    display_surface.blit(Play_button,(play_button_x,play_button_y))
    pygame.display.update()



    while running:
        pygame.time.delay(10)
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            if event.type == pygame.KEYDOWN and event.key == pygame.K_j:
                print("shoot")
                new_bullet_right = {'x': spaceship_x + spaceship_width - 43, 'y': spaceship_y,'speed': 1.5}
                new_bullet_left = {'x': spaceship_x + spaceship_width - 125, 'y': spaceship_y,'speed': 1.5}
                Bullets_left.append(new_bullet_left)
                Bullets_right.append(new_bullet_right)


        keys = pygame.key.get_pressed()
        if keys[pygame.K_a] and spaceship_x > 0:
            spaceship_x -= spaceship_speed
        if keys[pygame.K_d] and spaceship_x < WINDOW_WIDTH - spaceship_width:
            spaceship_x += spaceship_speed
        if keys[pygame.K_w] and spaceship_y > 0:
            spaceship_y -= spaceship_speed
        if keys[pygame.K_s] and spaceship_y < WINDOW_HEIGHT - spaceship_height:
            spaceship_y += spaceship_speed
        display_surface.fill((0, 0, 0))


        for i in range(num_meteorites):
            meteorites[i] = (meteorites[i][0],meteorites[i][1] + meteorites_speeds[i])
            display_surface.blit(meteorite,meteorites[i])


            if (spaceship_y <= meteorites[i][1] + meteorite_height and
                spaceship_x + spaceship_width >= meteorites[i][0] and
                spaceship_x <= meteorites[i][0] + meteorite_width and
                spaceship_y + spaceship_width >= meteorites[i][1]):
                hp_width -= damage
                if hp_width == 0:
                    Lost = True
                while Lost:
                    for event in pygame.event.get():
                        if event.type == pygame.QUIT:
                            running = False
                            break
                        if event.type == pygame.KEYDOWN and event.key == pygame.K_RETURN:
                            Lost = False
                            spaceship_x = 550
                            spaceship_y = 550
                            for i in range(num_meteorites):
                                meteorites[i] = (randint(1, WINDOW_WIDTH), 0)
                        if event.type == pygame.MOUSEBUTTONDOWN:
                            mouse_pos = pygame.mouse.get_pos()
                            if retry_button_x <= mouse_pos[0] <= retry_button_x + retry_button_width and retry_button_y <= mouse_pos[1] <= retry_button_y + retry_button_height:
                                spaceship_x = 530
                                spaceship_y = 550
                                for i in range(num_meteorites):
                                    meteorites[i] = (randint(1, WINDOW_WIDTH), 0)
                                hp_width = 180
                                Lost = False
                        if event.type == pygame.MOUSEBUTTONDOWN:
                            mouse_pos = pygame.mouse.get_pos()
                            if quit_button_x <= mouse_pos[0] <= quit_button_x + quit_button_width and  quit_button_y <= mouse_pos[1] <= quit_button_y + quit_button_height:
                                spaceship_x = 550
                                spaceship_y = 550
                                for i in range(num_meteorites):
                                    meteorites[i] = (randint(1, WINDOW_WIDTH), 0)
                                Game_Home = True
                                running = False


                    display_surface.blit(Quit_button, (quit_button_x, quit_button_y))
                    display_surface.blit(Retry_button, (retry_button_x, retry_button_y))
                    pygame.display.update()
                    if not running:
                        break

            if meteorites[i][1] + meteorite_height >= WINDOW_HEIGHT:
                meteorites_speeds[i] = randint(Min_speeds, Max_speeds)
                meteorites[i] = (randint(1,WINDOW_WIDTH),0)

            for bullet1 in Bullets_left:
                display_surface.blit(Space_slug_left, (bullet1['x'], bullet1['y']))
                bullet1['y'] -= bullet1['speed']

            for bullet2 in Bullets_right:
                display_surface.blit(Space_slug_right,(bullet2['x'],bullet2['y']))
                bullet2['y'] -= bullet2['speed']




            Bullets_left = [bullet1 for bullet1 in Bullets_left if bullet1['y'] > 0]
            Bullets_right = [bullet2 for bullet2 in Bullets_right if bullet2['y'] > 0]

            enemy_random = randint(0,400)
            enemy_speed = 1
            enemy_y = 100
            enemy_x = 550









        display_surface.blit(spaceship,(spaceship_x,spaceship_y))
        pygame.draw.rect(display_surface,status_color,(status_x,status_y,status_width,status_height))
        pygame.draw.rect(display_surface,hp_color,(hp_x,hp_y,hp_width,hp_height))
        pygame.display.update()

