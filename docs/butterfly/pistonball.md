---
action-type: "either"
title: "Pistonball"
actions: Either
agents: "20"
manual-control: "Yes"
action-shape: "(1,)"
action-values: "discrete (2)"
observation-shape: "(200, 120, 3)"
observation-values: "(0, 255)"
average-total-reward: "-1053.72"
import: "from pettingzoo.butterfly import pistonball_v1"
agent-labels: "agents= ['piston_0', 'piston_1', ..., 'piston_19']"
---

{% include info_box.md %}



This is a simple physics based cooperative game where the goal is to move the ball to the left wall of the game border by activating any of the twenty vertically moving pistons. Each piston agents observation is an RGB image of the area the, the two pistons next to them can move in (or the wall for pistons against the wall). Every piston can be acted on in any given time, the action space in discrete mode is 0 for down, 1 for staying still, and 2 for up. In continuous mode, the value is proportional to the amount the pistons are raised or lowered by.

Accordingly, pistons must learn highly coordinated emergent behavior to achieve an optimal policy for the environment. Each agent gets a reward that is a combination of how much the ball moved left overall and how much the ball moved left if it was close to the piston (i.e. movement it contributed to). Balancing the ratio between these appears to be critical to learning this environment, and as such is an environment parameter. The local reward applied is 0.5 times the change in the ball's x-position. Additionally, the global reward is change in x-position divided by the starting position, times 100, plus the `time_penalty` (default -0.1). For each piston, the reward is `local_ratio` * local_reward + (1-`local_ratio`) * global_reward. The local reward is applied to pistons surrounding the ball while the global reward is provided to all pistons.

Pistonball uses the chipmunk physics engine, and are thus the physics are about as realistic as in the game Angry Birds.

Keys *a* and *d* control which piston is selected to move (initially the rightmost piston is selected) and keys *w* and *s* move the piston in the vertical direction.


### Arguments

```
pistonball.env(local_ratio=.2, time_penalty=-0.1, continuous=False, random_drop=True,
starting_angular_momentum=True, ball_mass = .75, ball_friction=.3,
ball_elasticity=1.5, max_cycles=900)
```

`local_ratio`:  Weight applied to local reward and global reward. Global reward weight will always be 1 - local reward weight.

`time_penalty`: Amount of reward added to each piston each timestep. Higher values mean higher weight towards getting the ball across the screen to terminate the game.

`continuous`:  If true, piston action is a real value between -1 and 1 which is added to the piston height. If False, then action is a discrete value to move a unit up or down.

`random_drop`:  If True, ball will initially spawn in a random x value. If False, ball will always spawn at x=800

`starting_angular_moment`:  If True, ball will spawn with a random angular momentum

`ball_mass`:  Sets the mass of the ball physics object

`ball_friction`:  Sets the friction of the ball physics object

`ball_elasticity`:  Sets the elasticity of the ball physics object
