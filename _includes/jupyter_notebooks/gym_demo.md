

```python
import gym
```


```python
env = gym.make('FrozenLake-v0') # initialize the Frozen Lake environment
```

    [2017-10-25 14:08:16,944] Making new env: FrozenLake-v0



```python
env.render() # check out what the initial state of the environment looks like
```

    [41mS[0mFFF
    FHFH
    FFFH
    HFFG
    





    <ipykernel.iostream.OutStream at 0x7f44924d5750>



What we see here is a layout of a 'frozen lake'. The agent can move in this world. Some tiles of the lake are walkable (frozen, 'F'), and others lead to the agent falling into the water (hole, 'H'). Additionally, the movement direction of the agent is uncertain and only partially depends on the chosen direction. The agent is rewarded for finding a walkable path to a goal tile. The available actions are walking one step in any of the four directions (up, down, left and right). The current state is highlighted in red.


```python
n_states = env.observation_space.n # number of states
n_actions = env.action_space.n # number of actions
print("This environment has %s states and %s actions." % (n_states, n_actions))
```

    This environment has 16 states and 4 actions.


Let us now try to move around in this environment a little bit.


```python
s0 = env.reset()
print("Initially the agent is in state %s." % s0)

s1 = env.step(env.action_space.sample()) # take a random step
print('''
    The agent is now in state %s and has recieved a reward %s. 
    Episode done? %s.
    Additional information (transition probability of landing in this state): %s.
    '''
    % s1)
env.render() # render current state
```

    Initially the agent is in state 0.
    
        The agent is now in state 0 and has recieved a reward 0.0. 
        Episode done? False.
        Additional information (transition probability of landing in this state): {'prob': 0.3333333333333333}.
        
    [41mS[0mFFF
    FHFH
    FFFH
    HFFG
      (Left)





    <ipykernel.iostream.OutStream at 0x7f44924d5750>


