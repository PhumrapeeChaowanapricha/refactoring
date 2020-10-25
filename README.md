# Refactoring
## Just tower game
From my senior project (Sirawich Direk) : https://github.com/magmagcup/just_tower_game.git

In `just_tower_game/model.py` 

https://github.com/magmagcup/just_tower_game/blob/master/model.py

Inside this code
``` python
class Model(arcade.Sprite):
    def __init__(self, filename, filename2,scale, pointx, pointy):
        super().__init__()
        #PIC
        self.textures.append(arcade.load_texture(filename, mirrored=True, scale=scale))
        self.textures.append(arcade.load_texture(filename2, mirrored=True, scale=scale))
        self.textures.append(arcade.load_texture(filename, scale=scale))
        self.textures.append(arcade.load_texture(filename2, scale=scale))

        self.direction = {'RIGHT':2,'RIGHT_move':3,'LEFT':0,'LEFT_move':1}

        # self.RIGHT = 2
        # self.RIGHT_move = 3
        # self.LEFT = 0
        # self.LEFT_move = 1

        self.center_x = pointx
        self.center_y = pointy

        #time
        self.jump_timer = time()
        self.skin_changer = time()



class Enemy(Model):
    # Constructor and some method.
    def update(self):
        self.center_x += self.change_x
        self.center_y += self.change_y
 
class YellowSlime(Enemy):
    def __init__(self, filename, filename2, scale, pointx, pointy):
        super().__init__(filename, filename2, scale, pointx, pointy)
        self.can_deflect = True
        self.want_to_jump = False

        def update(self):
            self.center_x += self.change_x
            self.center_y += self.change_y


class Player(Model):

    # Constructor and some method.
    
    def update(self):
        self.center_x += self.change_x
        self.center_y += self.change_y

        if time() - self.still >= 0.5:
            motion = None
            if self.motion_status == self.direction['LEFT']:
                self.set_texture(self.direction['LEFT_move'])
                motion = self.direction['LEFT_move']
            elif self.motion_status == self.direction['LEFT_move']:
                self.set_texture(self.direction['LEFT'])
                motion = self.direction['LEFT']
            elif self.motion_status == self.direction['RIGHT']:
                self.set_texture(self.direction['RIGHT_move'])
                motion = self.direction['RIGHT_move']
            elif self.motion_status == self.direction['RIGHT_move']:
                self.set_texture(self.direction['RIGHT'])
                motion = self.direction['RIGHT']

            self.motion_status = motion
            self.still = time()

        if self.change_x < 0:
            self.set_texture(self.direction['LEFT_move'])
            self.facing_status = self.direction['LEFT']
            self.motion_status = self.direction['LEFT_move']

        elif self.change_x > 0:
            self.set_texture(self.direction['RIGHT_move'])
            self.facing_status = self.direction['RIGHT']
            self.motion_status = self.direction['RIGHT_move']

        if self.left < 0:
            self.left = 0
        elif self.right > SCREEN_WIDTH - 1:
            self.right = SCREEN_WIDTH - 1

```
Refactoring Signs:
As you can see that inside this code use Inheritance to create a polymorphism in the class so Model class should has the absatract method that will be implement by every subclass

Refactoring: Create the abstract method inside the Model class
``` python

class Model(arcade.Sprite):
    def __init__(self, filename, filename2,scale, pointx, pointy):
        super().__init__()
        #PIC
        self.textures.append(arcade.load_texture(filename, mirrored=True, scale=scale))
        self.textures.append(arcade.load_texture(filename2, mirrored=True, scale=scale))
        self.textures.append(arcade.load_texture(filename, scale=scale))
        self.textures.append(arcade.load_texture(filename2, scale=scale))

        self.direction = {'RIGHT':2,'RIGHT_move':3,'LEFT':0,'LEFT_move':1}

        # self.RIGHT = 2
        # self.RIGHT_move = 3
        # self.LEFT = 0
        # self.LEFT_move = 1

        self.center_x = pointx
        self.center_y = pointy

        #time
        self.jump_timer = time()
        self.skin_changer = time()
        
    def update(self):
        pass
 ```
 This will make every subclass that inherit the update method.
