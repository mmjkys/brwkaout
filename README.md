
import pygame
import random
pygame.init()

#======================



screen = pygame.display.set_mode((700,500))
pygame.display.set_caption("Pong")


#====================== Variable ============================
doExit = False
 
 
#variable to hold paddle pos\\
p2x = 350
p2y = 400
p2Score = 0

ball_size = 10
ball_x = 350 #xpos
ball_y = 250 #ypos
bVx = 5 #x vel (hor)
bVy = 5 #y vel (ver)

class brick:
    def __init__(self, xpos, ypos):
        self.xpos = xpos
        self.ypos = ypos
        self.color = (random.randrange(100,250), random.randrange(100,250), random.randrange(100,250))
        self.isDead = False
    def draw(self):
        if not self.isDead:
            pygame.draw.rect(screen, self.color, (self.xpos, self.ypos, 100, 50))
    #BOX COLL
    def collide(self, ball_x, ball_y):
        if not self.isDead:
            if (ball_x + ball_size > self.xpos and
                ball_x < self.xpos + 100 and #width of brick is 100
                ball_y + ball_size > self.ypos and
                ball_y < self.ypos + 50): #heaight of brick is 50
                self.isDead = True
                return True
            return False



#========== clock control screen updates ====================
clock = pygame.time.Clock()

b1 = brick(50,50)

while not doExit: #Game Loop

    #ball movement
    ball_x += bVx
    ball_y += bVy

       
    #right paddle
    if ball_x > p2x and ball_x < p2x + 100 and ball_y > p2y and ball_y < p2y + 20:
        bVy *= -1




    #reflect ball
    if ball_x < 0 or ball_x + 20 > 700: #bounce against left and right wall
        bVx *= -1
    if ball_y < 0 or ball_y + 20 > 500: #bounce against top or bottom
        bVy *= -1
        
        
    # Ball collision with brick
    if b1.collide(ball_x, ball_y):
        bVy *= -1



    #event queue
    clock.tick(60) #FPS
   
    for event in pygame.event.get(): #check if done something
        if event.type == pygame.QUIT: #Check if clicked
            doExit = True #Flag
           
    #game logic==============================================
    keys = pygame.key.get_pressed()
    if keys[pygame.K_w]:
        p1y-=5
    if keys[pygame.K_s]:
        p1y+=5
       
    if keys[pygame.K_p]:
        p2x+=5
    if keys[pygame.K_o]:
        p2x-=5
       
    if ball_x < 0:
        bVx *= 1
        p2Score += 1
    if ball_x > 680:
        bVx *= 1

           
           
    #render==================================================
    screen.fill((0,0,0)) #wipe screen
   
    #Display Scores
   
    #reflect ball means add point

    #background
    pygame.draw.rect(screen, (86, 209, 94), (0, 0, 2000, 2000))  
       
    #scores
    font = pygame.font.Font(None, 74) 
    text = font.render(str(p2Score), 1, (255, 59, 219))
    screen.blit(text, (420, 10))
   
    #draw rec
    b1.draw()
    pygame.draw.rect(screen, (209, 92, 255), (p2x, p2y, 100, 20), 4)
    
    #draw cir
    pygame.draw.circle(screen, (42, 71, 50), (ball_x, ball_y), 20)
   
    #update screen
    pygame.display.flip()
   

 

 
 
#END GAME LOOP===========================================
