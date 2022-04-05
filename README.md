
import pygame
import math
import sys
import random

pygame.font.init()

sw = 1150
sh = 1000


playerShip = pygame.image.load("PLAYER_SHIP.png")
asteroids = pygame.image.load("ASTEROID.png")
enemy = pygame.image.load("E_BOSS.png")
missile = pygame.transform.scale(pygame.image.load("F_WEAPON.png"),(10,10))
bg = pygame.image.load("BACKGROUND.png")
click = pygame.mixer.load("click.mp3")


screen = pygame.display.set_mode((sw, sh))
pygame.display.set_caption("Hamza's: Space game")

clock = pygame.time.Clock()

gameover = False

class Player():
    def __init__(self,x ,y, health = 5):
        self.ship_img = playerShip
        self.bullet_img = missile
        self.x = x
        self.y = y
        self.w = playerShip.get_width()
        self.h = playerShip.get_height()
        self.boost = 100
        self.initial_health = health
        self.shots = []
        self.shots_cooldown = 200
        self.rect = self.ship_img.get_rect()
        self.mask = pygame.mask.from_surface(self.ship_img)
 
    def draw(self, screen):
        screen.blit(playerShip, (self.x, self.y))
        #if self.remaining_health > 0:
         #   pygame.draw.rect(screen, (0,255,0), (self.rect.x, (self.rect.bottom + 10), (self.w * (self.remaining_health / self.initial_health)), 9))            

    def forward(self, velocity):
        self.y -= velocity
    
    def backwards(self, velocity):
        self.y += velocity
    
    def left(self, velocity):
        self.x -= velocity
    
    def right(self, velocity):
        self.x += velocity
        
    def accelerate(self, keys, velocity, boost):
        a = 1.3
        if keys[pygame.K_w]:
            while boost > 0:
                if keys[pygame.K_LEFT] or keys[pygame.K_RIGHT]:
                    self.x = velocity * a
                    boost -= 1
                elif keys[pygame.K_UP] or keys[pygame.K_DOWN]: 
                    self.y = velocity * a
                    boost -= 1 

    def move(self,keys):
        velocity = 3
        if keys[pygame.K_LEFT] and self.x - velocity > 0:
            left(velocity)
        elif keys[pygame.K_RIGHT] and self.x + self. w + velocity < sw:
            right(velocity)
        elif keys[pygame.K_DOWN] and self.y + self.h + velocity > 0:
            backward(velocity)
        elif keys[pygame.K_UP] and self.y - velocity > sh:
            forward(velocity)
        elif keys[pygame.K_w]:
            Player.accelerate(velocity, self.boost)   
            
            
class Enemy(Player):
    def __init__(self, x, y, health = 10):
        super().__init__(x, y, health)
        self.ship_img = enemy
        self.mask = pygame.mask.from_surface(self.ship_img)
        self.shots_cooldown = 100        
    
    def move(self, velocity):
        self.y +=   velocity + random.randint(0,4)
        
class Asteroid():
    def __init__(self):
        self.asteroid_img = asteroids
        self.size = random.randint(1,3)
        self.mask = pygame.mask.from_surface(self.asteroid_img)
        self.hit = False
        
    def big(self, size, asteroids, hit):
        if size == 1:
            asteroids = pygame.transform.scale(asteroids, random.randint(40,100))
        elif size == 2:
            asteroids = pygame.transform.scale(asteroids, random.randint(120,150))
            if hit == True:
                asteroids = random.randint(1,3)
        elif size == 3:
            if hit == True:
                asteroids = random.randint(1,2)
    
    
            
'''
class Bullet(Player):
    def __init__(self, x, y):
        self.missile_img = missile
        self.maxAmmo = 10
        self.shots= [0]
        self.num = 1
        self.long = len(self.shots)-1
        self.start = x, y
        self.mask = pygame.mask.from_surface(self.missile_img)

    def draw(self, screen):
        pygame.draw.rect(bg, (255,255,255), [self.x, self.y, self.w, self.h])

    def shoot(self, keys):
        if Bullet.CanShoot == True: 
            self.pos = Player.pos
            if self.y >= sh + 100:
                self.kill(10)
                self.shots = self.shots.pop(self.num)
                self.num -= 1
 
            Bullet.move()
        else:
            Bullet.blank()

    def move(self):
        self.y += self.yv

    def UpdateBullet(self, num, shots):
        self.pos = Player.pos
        if self.missile_img.y >= sh + 100:
            self.kill()
            self.shots = shots.pop(self.num)
            self.num -= 1
            
    def CanShoot(self, shots, num):
        while confirm == " ":
            self.shots.append(self.num)
            self.num += 1
            self.long = len(self.shots) - 1
            if self.num > 4:
                CanShoot = False
            else:
                CanShoot = True
        
    def IsItShot(self,CanShoot):
        while CanShoot == True:
            while confirm == " ":
                self.shots.append(self.num)
                self.num += 1
                self.long = len(self.shots)-1
                print(self.shots)
                if self.num > 4:
                    return False
                else:
                    confirm = str(input("Another input"))
                
OBJECT1 = Bullet()

print(OBJECT1.shots)
while True:
    ans = input("Enter a value")
    OBJECT1.IsItShot(ans)
    if OBJECT1.num > 4:
        pass
                
    def blank(self):
        while Bullet.CanShoot == False and keys[pygame.K_w]:
            pygame.mixer.Sound.play(click)
'''

#bullet = Bullet()
playerBullets=[]

#Objects = pygame.sprite.Group()
#Objects.add(player)
#Objects.add(bullet)



def game():
    run = True
    score = 0
    health = 5
    
    player = Player(sw//2, sh//2, 5)
    
    def redraw_screen():
        main_font = pygame.font.SysFont("comicsans", 50)
        screen.blit(bg, (0, 0))
        #Objects.draw(screen)
        health_view = main_font.render(f"Health: {health}", 1,(0,255,0))
        score_view = main_font.render(f"Health: {score}", 1,(0,255,0))
        screen.blit(health_view, (20, sw-20))
        screen.blit(score_view, (20, 20))
        player.draw(screen)
        #bullet.draw(screen)
        pygame.display.update()
    
    while run:
        clock.tick(60)
        if len(Enemy) == 0:
            score += 20
        #if not gameover:
            #for b in playerBullets:
            # #b.move()

        keys = pygame.key.get_pressed()
        player.move(keys)


        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                run = False
                pygame.quit()
        
        keys = pygame.key.get_pressed()
        player.move(keys)

        redraw_screen()





screen = pygame.display.set_mode((sw,sh))
main_font = pygame.font.SysFont("comicsans", 50)
title_font = pygame.font.SysFont("comicsans", 150)
playImg = pygame.transform.scale(pygame.image.load("menu.png"),(400, 75))

def title_main(x, y, display_text):
    x = x
    y = y
    display_text = display_text
    text = title_font.render(display_text, True, "white")
    text_rect = text.get_rect(center=(x, y))
    screen.blit(text, text_rect)

class Button():
    def __init__(self, image, x, y, display_text):
        self.x = x
        self.y = y
        self.image = image
        self.rect = self.image.get_rect(center=(self.x, self.y))
        self.display_text = display_text
        self.text = main_font.render(self.display_text, True, "black")
        self.title = title_font.render(self.display_text, True, "black")
        self.text_rect = self.text.get_rect(center=(self.x, self.y))

    def update(self):
        screen.blit(self.image, self.rect)
        screen.blit(self.text, self.text_rect)

    

    def ChangeColour(self, position):
        if position[0] in range(self.rect.left, self.rect.right) and position[1] in range(self.rect.top, self.rect.bottom):
            self.text = main_font.render(self.display_text,True,"green")
        else:
            self.text = main_font.render(self.display_text,True,"white")


playGame = Button(playImg, 575, 300, "Play Game!")
instructionGame = Button(playImg, 575, 400, "Instructions")
quitGame = Button(playImg, 575, 500, "Quit game" )

picture_bg = pygame.image.load("BACKGROUND.png")

    

def Inputs(event):
    if event.type == pygame.MOUSEBUTTONDOWN:
        pos = pygame.mouse.get_pos()
        if pos[0] in range(playGame.rect.left, playGame.rect.right) and pos[1] in range(playGame.rect.top, playGame.rect.bottom):
            game()
        elif pos[0] in range(quitGame.rect.left, quitGame.rect.right) and pos[1] in range(quitGame.rect.top, quitGame.rect.bottom):
            pygame.quit()

while True:
    mou = pygame.mouse.get_pos()
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        else:
            Inputs(event)
            
        #if event.type == pygame.MOUSEBUTTONDOWN:
         #playGame.CheckForInput(mou)
          #if event.type == playGame:
           #playGame.update


    screen.blit(picture_bg, (0, 0))
    title_main(575, 150, "Hamza's game")

    playGame.update()
    playGame.ChangeColour(mou)
    instructionGame.update()
    instructionGame.ChangeColour(mou)
    quitGame.update()
    quitGame.ChangeColour(mou)
    
    
    pygame.display.update()





'''
        else:
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    bullet.shoot()
                    if not gameover:
                        playerBullets.append(Bullet())
    '''



'''
    FPS=60 
    hits=5
    score=0
    main_font= pygame.font.SysFont("comicsans", 50)
    lost_font= pygame.font.SysFont("comicsans", 70)

    velocity = 10

    enemies = []
    enemyVel = 5

    lost = False

    player = Player(300,650)

    clock = pygame.time.Clock()

    gameover = False
'''

'''


        if gameover :
            lost_label = lost_font.render("You have lost!", 1, (0,255,0))
            screen.blit(lost_label, (sw/2 - lost_label.get_width()/2, 350))



        pygame.display.update()

'''

'''
if len(asteroids) < 5 and asteroidsTimer == 0:
    asteroids.appends(asteroids(radnom.radnint(20, 430), 0, asteroids.w, asteroids.h))
    asteroidsTimer = 1
if timer >= 1:
    timer += 1
    if timer == 4:
        timer= 0
        
        
if asteroidsTimer >= 1:
    asteroidsTimer += 1
    if asteroidsTimer == 100:
        asteroidsTimer = 0
    
    if keys[pygame.K_Space]:
        if len(bullets) < 3 and timer == 0:
            bullets.append(projectiles(round(player.x + player.w // 2), player.y, player.w, player.h))



'''
