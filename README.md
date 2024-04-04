import pygame
pygame.init()

win=pygame.display.set_mode((500,500))
pygame.display.set_caption("The dark knight")

walk_right=[pygame.image.load('r1.png'),pygame.image.load('r2.png'),pygame.image.load('r3.png'),pygame.image.load('r4.png'),pygame.image.load('r5.png'),pygame.image.load('r6.png'),pygame.image.load('r7.png'),pygame.image.load('r8.png'),pygame.image.load('r9.png')]
walk_left=[pygame.image.load('l1.png'),pygame.image.load('l2.png'),pygame.image.load('l3.png'),pygame.image.load('l4.png'),pygame.image.load('l5.png'),pygame.image.load('l6.png'),pygame.image.load('l7.png'),pygame.image.load('l8.png'),pygame.image.load('l9.png')]
bg=pygame.image.load('bg.png')
char=pygame.image.load('s.png')
jump=pygame.image.load('j.png')

bulletsound=pygame.mixer.Sound('bullet.mp3')
hitsound=pygame.mixer.Sound('hit.mp3')
victorysoud=pygame.mixer.Sound('iambatman.mp3')
defetsound=pygame.mixer.Sound('batmanhitharley.mp3')
harleysound=pygame.mixer.Sound('harleylaugh.mp3')
gamesound=pygame.mixer.Sound('something.mp3')
gamesound.play(-1)
#defetsound.play(-1)
#opps
class player(object):
    def __init__(self,x,y,width,height):
        self.x=x
        self.y=y
        self.width=70
        self.height=70
        self.vel=5.5
        self.is_jump=False
        self.jump_count=10
        self.right=False
        self.left=False
        self.walk_count=0
        self.standing=True
        self.hitbox=(self.x,self.y+10,65,50)
        self.health=10
        self.visible=True
    def draw_char(self,win):
        if self.visible:
            if self.walk_count+1>=27:
                    self.walk_count=0
            if not (self.standing):
                if self.left:
                        win.blit(walk_left[self.walk_count//3],(self.x,self.y))
                        self.walk_count+=1
                elif self.right:
                        win.blit(walk_right[self.walk_count//3],(self.x,self.y))
                        self.walk_count+=1
            else:
                if self.left:
                    win.blit(walk_left[0],(self.x,self.y))
                else:
                    win.blit(walk_right[0],(self.x,self.y))
            self.hitbox=(self.x,self.y+10,65,50)
            pygame.draw.rect(win,'GREEN',(self.hitbox[0],self.hitbox[1]-20,60-5*(10-self.health),10))
            #pygame.draw.rect(win,'GREEN',(self.hitbox[0],self.hitbox[1]-20,50,10))
            #pygame.draw.rect(win,"YELLOW",self.hitbox,2)
        else:
            harleysound.play(-1)
    def hit(self):
            if self.health<0:
                self.visible=False
            if harley.visible:
                self.x=0
                self.y=440
                font1=pygame.font.SysFont('comicsan',100)
                text=font1.render('-5',1,'YELLOW')
                win.blit(text,(250-(text.get_width()/2),250))
                pygame.display.update()
                i=0
                while i<300:
                    pygame.time.delay(8)
                    i+=1
                    for event in pygame.event.get():
                        if event.type==pygame.QUIT:
                            i=301
                            pygame.quit()

class projectile(object):
    def __init__(self,x,y,radius,color,facing):
        self.x=x
        self.y=y
        self.color=color
        self.radius=radius
        self.facing=facing
        self.vel=8*facing
    def draw_char(self,win):
        pygame.draw.circle(win,self.color,(self.x,self.y),self.radius,)
class enemy(object):
    walk_right=[pygame.image.load('re1.png'),pygame.image.load('re2.png'),pygame.image.load('re3.png'),pygame.image.load('re4.png'),pygame.image.load('re5.png'),pygame.image.load('re6.png'),pygame.image.load('re7.png'),pygame.image.load('re8.png'),pygame.image.load('re9.png')]
    walk_left=[pygame.image.load('le1.png'),pygame.image.load('le2.png'),pygame.image.load('le3.png'),pygame.image.load('le4.png'),pygame.image.load('le5.png'),pygame.image.load('le6.png'),pygame.image.load('le7.png'),pygame.image.load('le8.png'),pygame.image.load('le9.png')]
    def __init__(self,x,y,width,height,end):
        self.x=x
        self.y=y
        self.width=width
        self.height=height
        self.end=end
        self.path=[self.x,self.end]
        self.walk_count=0
        self.vel=3
        self.hitbox=(self.x,self.y,30,58)
        self.health=10
        self.visible=True
        self.collide=False

    def draw_char(self,win):
        self.move()
        if self.visible:
            if self.walk_count+1>27:
                self.walk_count=0
            if self.vel>0:
                win.blit(self.walk_right[self.walk_count // 3],(self.x,self.y))
                self.walk_count+=1
            else:
                win.blit(self.walk_left[self.walk_count // 3],(self.x,self.y))
                self.walk_count+=1
        self.hitbox=(self.x,self.y,30,58)
        pygame.draw.rect(win,'YELLOW',(self.hitbox[0],self.hitbox[1]-20,50-5*(10-self.health),10))
        #pygame.draw.rect(win,'RED',(self.hitbox[0],self.hitbox[1]),20,50)
        #pygame.draw.rect(win,"YELLOW",self.hitbox,2)

    def move(self):
        if self.vel>0:
            if self.x+self.vel<self.path[1]:
                self.x+=self.vel
            else:
                self.vel=self.vel*-1
                self.x+=self.vel
        else:
            if self.x-self.vel>self.path[0]:
                self.x+=self.vel
            else:
                self.vel=self.vel*-1
                self.x+=self.vel
    def hit(self):
        if self.health>0:
            self.health-=1
        else:
            victorysoud.play()
            self.visible=False


screen_width=500
clock=pygame.time.Clock()
score=0




def redraw_char():
    win.blit(bg,(0,0))
    batman.draw_char(win)
    text=font.render('SCORE:'+str(score),win,'YELLOW',1)
    win.blit(text,(320,10))
    for bullet in bullets:
        bullet.draw_char(win)
    harley.draw_char(win)
    if not harley.visible:
        font2=pygame.font.SysFont('comicsan',50)
        text1=font2.render('I AM BATMAN',win,'YELLOW',1)
        win.blit(text1,(150,150))
    if not batman.visible:
        font3=pygame.font.SysFont('comicsan',50)
        text2=font3.render('YOU LOST',win,'YELLOW',1)
        win.blit(text2,(150,150))
    pygame.display.update()

run=True
batman=player(0,440,70,70)
harley=enemy(100,440,50,50,450)
shootloop=0
font=pygame.font.SysFont('comicsans',30,True,True)
bullets=[]


while run:
    clock.tick(27)
    if harley.collide:
        defetsound.play()
        harley.collide=False
    if harley.visible:
        if batman.hitbox[1]  < harley.hitbox[1] + harley.hitbox[3] and batman.hitbox[1] > harley.hitbox[1]:
            if batman.hitbox[0]  + batman.hitbox[2]  > harley.hitbox[0] and batman.hitbox[0]  < harley.hitbox[0] + harley.hitbox[2]:
                if batman.visible:
                    batman.hit()
                    batman.health-=5
                    score-=5
                    harley.collide=True
    if shootloop>0:
        shootloop+=1
    if shootloop>3:
        shootloop=0
    for events in pygame.event.get():
        if events.type==pygame.QUIT:
            run=False
    for bullet in bullets:
        if harley.visible:
            if bullet.y - bullet.radius < harley.hitbox[1] + harley.hitbox[3] and bullet.y + bullet.radius > harley.hitbox[1]:
                if bullet.x + bullet.radius > harley.hitbox[0] and bullet.x - bullet.radius < harley.hitbox[0] + harley.hitbox[2]:
                    hitsound.play()
                    harley.hit()
                    score+=1
                    bullets.pop(bullets.index(bullet))
        if bullet.x<500 and bullet.x>0:
            bullet.x=bullet.x+bullet.vel
        else:
            bullets.pop(bullets.index(bullet))
    keys=pygame.key.get_pressed()
    if keys[pygame.K_SPACE] and shootloop==0:
        bulletsound.play()
        if batman.left:
            facing=-1
        else:
            facing=1
        if len(bullets)<5:
            bullets.append(projectile(round(batman.x+batman.width // 2),round(batman.y+batman.height // 2),6,'RED',facing))
        shootloop=1
    if keys[pygame.K_LEFT] and batman.x>0:
        batman.x=batman.x-batman.vel
        batman.left=True
        batman.right=False
        batman.standing=False
    elif keys[pygame.K_RIGHT] and batman.x<screen_width-batman.width-batman.vel:
        batman.x=batman.x+batman.vel
        batman.right=True
        batman.left=False
        batman.standing=False
    else:
        batman.standing=True
    if not batman.is_jump:
        if keys[pygame.K_UP]:
            batman.is_jump=True
    else:
        if batman.jump_count>=-10:
            neg=1
            if batman.jump_count<0:
                neg=-1
            batman.y-=(batman.jump_count**2)*0.5*neg
            batman.jump_count-=1
        else:
            batman.is_jump=False
            batman.jump_count=10
    redraw_char()
pygame.quit()
