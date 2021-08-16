# Better-Tic-Tac-Toe
Tic-Tac-Toe with Initial screen and Match result
```Python
import pygame,sys
import time
import random

FPS=60
WIDTH=500
HEIGHT=500
blockw=20
blockh=20
Time=0
judge=False
check=-1
arr=[[0,0,0],[0,0,0],[0,0,0]]
emergency=0

pygame.init()
pygame.display.set_caption('Small Game')# name of the window
screen=pygame.display.set_mode([WIDTH,HEIGHT])
clock=pygame.time.Clock()

class Player(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image=pygame.Surface((blockw,blockh))
        self.image.fill([0,0,255])
        self.rect=self.image.get_rect()
        self.rect.x=(WIDTH-blockw)//2;
        self.rect.y=(HEIGHT-blockh)//2; 
    def update(self):
        key_pressed=pygame.key.get_pressed()
        if key_pressed[pygame.K_RIGHT]:
            self.rect.x=self.rect.x+20
        if key_pressed[pygame.K_LEFT]:
            self.rect.x=self.rect.x-20
        if key_pressed[pygame.K_UP]:
            self.rect.y=self.rect.y-20
        if key_pressed[pygame.K_DOWN]:
            self.rect.y=self.rect.y+20
        if(self.rect.x>WIDTH-100-blockw):
            self.rect.x=100
        if(self.rect.x<100):
            self.rect.x=WIDTH-100
        if(self.rect.y>HEIGHT-100-blockh):
            self.rect.y=100
        if(self.rect.y<100):
            self.rect.y=HEIGHT-100
    def Paint(self):
        paint=color(self.rect.x,self.rect.y)
        block=Block(self.rect.x,self.rect.y)
        all_sprites.add(paint)
        all_sprites.add(block)
    
        
          
class color(pygame.sprite.Sprite):
    def __init__(self,x,y):
        global Time,judge,check,arr,emergency
        pygame.sprite.Sprite.__init__(self)
        self.image=pygame.Surface((98,98))
        if(Time%2==0 and arr[x//100-1][y//100-1]==0):
            self.image.fill([255,0,0])
            Time+=1
            arr[x//100-1][y//100-1]=2
        elif arr[x//100-1][y//100-1]==0:
            self.image.fill([0,255,0])
            Time+=1
            arr[x//100-1][y//100-1]=1
        elif arr[x//100-1][y//100-1]==2:
            self.image.fill([255,0,0])
        else:
            self.image.fill([0,255,0])
        self.rect=self.image.get_rect()  
        self.rect.x=x//100*100+1  
        self.rect.y=y//100*100+1 
        #To Check if there is a winner
        for i in range (3):
            if(arr[0][i]!=0 and arr[0][i]==arr[1][i] and arr[1][i]==arr[2][i]):
                judge=True
                check=arr[0][i]
                break
            elif(arr[i][0]!=0 and arr[i][0]==arr[i][1] and arr[i][1]==arr[i][2]):
                judge=True
                check=arr[i][0]
                break
        if(arr[0][0]!=0 and arr[0][0]==arr[1][1] and arr[1][1]==arr[2][2] and judge==False):
            judge=True
            check=arr[0][0]
        elif(arr[0][2]!=0 and arr[0][2]==arr[1][1] and arr[1][1]==arr[2][0] and judge==False):
            judge=True
            check=arr[0][2]

        if Time==9:
            if check==-1:
                screen.fill((0,0,0))
                draw_text(screen,'Tie!!',30,WIDTH/2,HEIGHT/3+20)
                draw_text(screen,'Press enter to restart',30,WIDTH/2,HEIGHT/3-20)
                pygame.display.update()
            emergency=1

        if check==1:    
            screen.fill((0,0,0))
            draw_text(screen,'BOB Win!!!',30,WIDTH/2,HEIGHT/3+20)
            draw_text(screen,'Press enter to restart',30,WIDTH/2,HEIGHT/3-20)
            pygame.display.update()
            waiting=True
            while waiting:
                clock.tick(FPS)
                for event in pygame.event.get():
                    if event.type==pygame.QUIT:
                        sys.exit()
                    elif event.type==pygame.KEYDOWN:
                        if event.key==pygame.K_KP_ENTER:
                            waiting=False
                        if waiting==False:
                            emergency=1
                
        elif check==2:
            screen.fill((0,0,0))
            draw_text(screen,'Alice Win!!!',30,WIDTH/2,HEIGHT/3+20)
            draw_text(screen,'Press enter to restart',30,WIDTH/2,HEIGHT/3-20)
            pygame.display.update()
            waiting=True
            while waiting:
                clock.tick(FPS)
                for event in pygame.event.get():
                    if event.type==pygame.QUIT:
                        sys.exit()
                    elif event.type==pygame.KEYDOWN:
                        if event.key==pygame.K_KP_ENTER:
                            waiting=False
                        if waiting==False:
                            emergency=1
                 

class Block(pygame.sprite.Sprite):
    def __init__(self,x,y):
        pygame.sprite.Sprite.__init__(self)
        self.image=pygame.Surface((blockw,blockh))
        self.image.fill([0,0,255])
        self.rect=self.image.get_rect()
        self.rect.x=x;
        self.rect.y=y; 
    def update(self):
        key_pressed=pygame.key.get_pressed()
        if key_pressed[pygame.K_RIGHT]:
            self.rect.x=self.rect.x+20
        if key_pressed[pygame.K_LEFT]:
            self.rect.x=self.rect.x-20
        if key_pressed[pygame.K_UP]:
            self.rect.y=self.rect.y-20
        if key_pressed[pygame.K_DOWN]:
            self.rect.y=self.rect.y+20
        if(self.rect.x>WIDTH-100-blockw):
            self.rect.x=100
        if(self.rect.x<100):
            self.rect.x=WIDTH-100
        if(self.rect.y>HEIGHT-100-blockh):
            self.rect.y=100
        if(self.rect.y<100):
            self.rect.y=HEIGHT-100

font_name=pygame.font.match_font('arial')
def draw_text(surf,text,size,x,y):
    font=pygame.font.Font(font_name,size)
    text_surface=font.render(text,True,[255,255,255])
    text_rect=text_surface.get_rect()
    text_rect.centerx=x
    text_rect.top=y
    surf.blit(text_surface,text_rect)

def draw_init():
    screen.fill((0,0,0))
    draw_text(screen,'Click any bottom to start',50,WIDTH/2,HEIGHT/3+20)
    pygame.display.update()
    waiting=True
    while waiting:
        clock.tick(FPS)
        for event in pygame.event.get():
            if event.type==pygame.QUIT:
                sys.exit()
            elif event.type==pygame.KEYDOWN:
                waiting=False


all_sprites=pygame.sprite.Group()
player=Player()
all_sprites.add(player)

show_init=True
while True:
    if show_init:
        draw_init()
        show_init=False
    clock.tick(FPS)
    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            sys.exit()
        elif event.type==pygame.KEYDOWN:
            if event.key==pygame.K_SPACE:
                player.Paint()
                if(emergency==1):
                    #player.Paint()
                    Time=0
                    show_init=True
                    check=-1
                    all_sprites=pygame.sprite.Group()
                    player=Player()
                    all_sprites.add(player)
                    arr=[[0,0,0],[0,0,0],[0,0,0]]
                    judge=False
                    emergency=0

            elif event.key==pygame.K_KP_ENTER:
                #player.Paint()
                Time=0
                show_init=True
                check=-1
                all_sprites=pygame.sprite.Group()
                player=Player()
                all_sprites.add(player)
                arr=[[0,0,0],[0,0,0],[0,0,0]]
                judge=False
                emergency=0
        
    screen.fill((255,255,255))
    pygame.draw.rect(screen,[0,0,0],[100,100,100,100],1)
    pygame.draw.rect(screen,[0,0,0],[100,200,100,100],1)
    pygame.draw.rect(screen,[0,0,0],[100,300,100,100],1)
    pygame.draw.rect(screen,[0,0,0],[200,100,100,100],1)
    pygame.draw.rect(screen,[0,0,0],[200,200,100,100],1)
    pygame.draw.rect(screen,[0,0,0],[200,300,100,100],1)
    pygame.draw.rect(screen,[0,0,0],[300,100,100,100],1)
    pygame.draw.rect(screen,[0,0,0],[300,200,100,100],1)
    pygame.draw.rect(screen,[0,0,0],[300,300,100,100],1)
    all_sprites.update()
    all_sprites.draw(screen) 
    pygame.display.update()
    
```
