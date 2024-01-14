# A 2 player pong game
# Each player will control a paddle. 'a' and 'q' will control the left paddle
# 'l' and 'p' will control the right paddle
# The ball will move continuously across the screen, each time it hits the 
# left or right edge, the player on the opposite side will gain a point


# import module
import pygame

def main():
   # initialize pygame modules
   pygame.init()
   
   # create a pygame display window
   pygame.display.set_mode((700, 500))
   
   # set the title of the display window
   pygame.display.set_caption('Pong')   
   
   # get the display surface
   w_surface = pygame.display.get_surface() 
   
   # create a game object
   game = Game(w_surface)
   
   # call the play method on the game object to start the game
   game.play() 
   
   # quit pygame and clean up the pygame window
   pygame.quit() 


# User-defined classes
class Game:
   # This class will represent what is happening in the game

   def __init__(self, surface):
      # Initialize game
      # - self is the Game to initialize
      # - surface is the display window surface object

      # create the surface and the color of the background of the game
      self.surface = surface
      self.bg_color = pygame.Color('black')
      
      # set up default FPS, game clock
      self.FPS = 60
      self.game_Clock = pygame.time.Clock()
      
      # set up status of game window and status of continue game      
      self.close_clicked = False
      self.continue_game = True
      
      # create game specific objects
      # create 1 ball, 2 paddles, and a score for each player
      self.ball = Ball('white', 10, [50, 50], [4, 3], self.surface)
      self.paddle_increment = 10
      self.left_paddle = Paddle(100,50,10,50,'white',self.surface)
      self.right_paddle = Paddle(600,50,10,50, 'white', self.surface)
      self.right_score = 0
      self.left_score = 0      

   def play(self):
      # Play the game until the player presses the close box of the window
      # - self is the Game 
      
      # loop until the user clicks the close box
      while not self.close_clicked: 
         # check events and draw objects
         self.handle_events()
         self.draw()            
         if self.continue_game:
            # continue to update game if continue_game remains true
            self.update()
            self.decide_continue()
         # run at most with FPS Frames Per Second 
         self.game_Clock.tick(self.FPS) 

   def handle_events(self):
      # Handle each user event by changing the game state appropriately.
      # - self is the Game whose events will be handled
      events = pygame.event.get()
      for event in events:
         # if game is quit, close game window
         if event.type == pygame.QUIT:
            self.close_clicked = True
            
         # if a key is pressed, carry out event
         elif event.type == pygame.KEYDOWN:
            self.handle_key_down(event)
            
         # if a key is no longer pressed, carry out event
         elif event.type == pygame.KEYUP:
            self.handle_key_up(event)        
               
   def handle_key_down(self,event):
      # reponds to KEYDOWN event
      # - self is the Game object
      
      # if 'a', 'q', 'l', or 'p' are pressed, move respective paddle
      if event.key == pygame.K_a:
         self.left_paddle.set_vertical_velocity(self.paddle_increment)
      elif event.key == pygame.K_q:
         self.left_paddle.set_vertical_velocity(-self.paddle_increment)
      elif event.key == pygame.K_l:
         self.right_paddle.set_vertical_velocity(self.paddle_increment)
      elif event.key == pygame.K_p:
         self.right_paddle.set_vertical_velocity(-self.paddle_increment)         
   
   def handle_key_up(self,event):
      # responds to KEYUP event
      # - self is the Game object
      
      # if 'a', 'q', 'l' or 'p' are no longer pressed, stop moving respective paddle
      if event.key == pygame.K_a:
         self.left_paddle.set_vertical_velocity(0)
      elif event.key == pygame.K_q:
         self.left_paddle.set_vertical_velocity(0)     
      elif event.key == pygame.K_l:
         self.right_paddle.set_vertical_velocity(0)    
      elif event.key == pygame.K_p:
         self.right_paddle.set_vertical_velocity(0)         

   def draw(self):
      # Draw all game objects.
      # - self is the Game to draw
      
      # draw in background, and game objects
      self.surface.fill(self.bg_color)
      self.left_paddle.draw()
      self.right_paddle.draw()
      self.draw_score()
      self.ball.draw()
      
      # update display
      pygame.display.update() 
   
   def draw_score(self):
      # draw scores for each side
      # - self will be the score to draw
      self.draw_left_score()
      self.draw_right_score()   
   
   def draw_left_score(self):
      # - self will be the score to draw
      
      # create font object
      font = pygame.font.SysFont('', 70)
      
      # render font to create a text_image
      text_image = font.render(str(self.left_score), True, pygame.Color('white'), self.bg_color)
      
      # place score in top left corner
      location = (0,0)
      
      # blit the text_image
      self.surface.blit(text_image, location)
      
   def draw_right_score(self):
      # - self will be the score to draw
      
      # create font object
      font = pygame.font.SysFont('', 70)
      
      # render the font to create a text_image
      text_image = font.render(str(self.right_score), True, pygame.Color('white'), self.bg_color)
      
      # place score in top right corner
      location = (self.surface.get_width() - text_image.get_width(), 0)
      
      # blit the text_image
      self.surface.blit(text_image, location)         

   def update(self):
      # Update the game objects for the next frame.
      # - self is the Game to update
      
      # update game paddles to move
      self.left_paddle.move()
      self.right_paddle.move()
      
      # calculate if the ball will hit a paddle
      center = self.ball.get_center()
      left_collide = self.left_paddle.collide(center)
      right_collide = self.right_paddle.collide(center)
      
      # if the ball hits a paddle, ball will bounce off in the opposite direction
      # ball will not bounce if the ball is behind the paddle
      ball_velocity = self.ball.get_velocity()
      if left_collide == True and ball_velocity[0] < 0:
         self.ball.bounce()
      if right_collide == True and ball_velocity[0] > 0:
         self.ball.bounce()
         
      
      # if the ball hits right edge of screen, left player score will go up by 1
      # if the ball hits left edge of screen, right player score will go up by 1
      edge = self.ball.move()     
      if edge == 'left':
         self.right_score = self.right_score + 1
      elif edge == 'right':
         self.left_score = self.left_score + 1      
         
      
   def decide_continue(self):
      # if either player score reaches 11, continue_game will be false
      # - self is the Game to check      
      if self.left_score == 11 or self.right_score == 11:
         self.continue_game = False
      
   

class Ball:
   # An object in this class will represent a ball
   
   def __init__(self, dot_color, dot_radius, dot_center, dot_velocity, surface):
      # Initialize the Ball
      # - self is the ball to initialize
      # - color is the pygame.Color of the ball
      # - center is a list containing the x and y int coords of the center of the ball
      # - radius is the int pixel radius of the ball
      # - velocity is a list containing the x and y components
      # - surface is the window's pygame.Surface object

      self.color = pygame.Color(dot_color)
      self.radius = dot_radius
      self.center = dot_center
      self.velocity = dot_velocity
      self.surface = surface
      
   def move(self):
      # Change the location of the ball by adding the corresponding 
      # speed values to the x and y coordinate of its center
      # - self is the Ball
      
      # get size for the surface
      size = self.surface.get_size() 
      
      # if the ball hits the edges of the screen, bounce off in opposite direction
      for i in range(0,2):
         self.center[i] = (self.center[i] + self.velocity[i])
         if self.center[i] < self.radius : 
            self.velocity[i] = - self.velocity[i]
         if self.center[i] + self.radius > size[i]: 
            self.velocity[i] = - self.velocity[i]
         
      # if the ball hits the left or right edge, set edge to 'left' or 'right'
      edge = ''
      if self.center[0] - self.radius < 0:
         edge = 'left'
      if self.center[0] + self.radius > size[0]:
         edge = 'right'
      return edge             
      
   def draw(self):
      # Draw the dot on the surface
      # - self is the Ball
      pygame.draw.circle(self.surface, self.color, self.center, self.radius)
      
   def get_center(self):
      # get the center of the dot
      # - self is the Ball
      return self.center
   
   def get_velocity(self):
      # get the velocity of the dot
      # - self is the Ball
      return self.velocity
   
   def bounce(self):
      # dot will move in the opposite direction by switching velocity
      # - self is the Ball
      self.velocity[0] = - self.velocity[0]
      return self.velocity
      
      
class Paddle:
   # An object in this class represents a Paddle that moves
   
   def __init__(self,x,y,width,height,color,surface):
      # - self is the Paddle object
      # - x, y are the top left corner coordinates of the rectangle of type int
      # - width is the width of the rectangle of type int
      # - height is the heightof the rectangle of type int
      # - surface is the pygame.Surface object on which the rectangle is drawn
      
      self.rect = pygame.Rect(x,y,width,height)
      self.color = pygame.Color(color)
      self.surface = surface
      self.vertical_velocity = 0
      
   def draw(self):
      # draw the paddle
      # -self is the Paddle object to draw
      pygame.draw.rect(self.surface,self.color,self.rect)
      
   def set_vertical_velocity(self, vertical_distance):
      # set the vertical velocity of the Paddle object
      # -self is the Paddle object
      # -vertical_distance is the int increment by which the paddle moves vertically
      self.vertical_velocity = vertical_distance
      
   def move(self):
      # make sure the paddle does not move outside the window
      # - self is the Paddle object
      self.rect.move_ip(0,self.vertical_velocity)
      if self.rect.top <= 0:
         self.rect.top = 0
      elif self.rect.bottom >= self.surface.get_height():
         self.rect.bottom = self.surface.get_height()
         
   def collide(self, center):
      # if ball is in the paddle return true, otherwise return false
      # - self is the Paddle object
      if self.rect.collidepoint(center):
         return True
      return False
main()
