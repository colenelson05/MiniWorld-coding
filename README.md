# MiniWorld-coding, FourRooms code 
import numpy as np
import math
from gym import spaces
from ..miniworld import MiniWorldEnv, Room
from ..entity import ImageFrame, Box, MeshEnt, Ball, Key #acts as all needed objects.

class FourRooms(MiniWorldEnv):
    """
    Classic four rooms environment.
    The agent must reach the any object to get a reward
    """

    def __init__(self, **kwargs):
        super().__init__(
            max_episode_steps=1000,
            **kwargs
        )

        # Allow only the movement actions
        self.action_space = spaces.Discrete(self.actions.move_forward+1)

    def _gen_world(self):
        # Top-left room
        room0 = self.add_rect_room(
            min_x=-7, max_x=-1,
            min_z=1 , max_z=7
        )
        # Top-right room
        room1 = self.add_rect_room(
            min_x=1, max_x=7,
            min_z=1, max_z=7
        )
        # Bottom-right room
        room2 = self.add_rect_room(
            min_x=1 , max_x=7,
            min_z=-7, max_z=-1
        )
        # Bottom-left room
        room3 = self.add_rect_room(
            min_x=-7, max_x=-1,
            min_z=-7, max_z=-1
        )

        # Add openings to connect the rooms together
        self.connect_rooms(room0, room1, min_z=3, max_z=5, max_y=2.2)
        self.connect_rooms(room1, room2, min_x=3, max_x=5, max_y=2.2)
        self.connect_rooms(room2, room3, min_z=-5, max_z=-3, max_y=2.2)
        self.connect_rooms(room3, room0, min_x=-5, max_x=-3, max_y=2.2)

        self.box = self.place_entity( MeshEnt(
              mesh_name='duckie', #creates the duckie object
              height=.5)
                )
       
        self.box = self.place_entity(
        Box(color=('green'),
              )
        )
       
        self.ball = self.place_entity(
            Ball(color=('blue')
                   )
           
            )
        self.key = self.place_entity(
            Key(color=('red')
                )
            )
        self.box = self.place_entity(
            Box(color=('purple')
                ))


        self.entities.append(ImageFrame(
            pos=[3, 2, 6],
            dir=math.pi/2,
            width=1.8,
            tex_name='window'
            ))
                           
        self.place_agent()

    def step(self, action):
        obs, reward, done, info = super().step(action)

        if self.near(self.box):
            reward += self._reward()
            done = True # one problem is that only the purple box works, this might be because of the order of code or indents. More research is needed. 
           
     
           
        if self.near(self.ball):
            reward += self._reward()
            done = True
         
        if self.near(self.key):
            reward += self._reward()
            done = True
            print('1') 
