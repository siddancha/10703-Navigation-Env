# 10-703 Project: Navigation Environment

This is based on the NEL (Never Ending Learning) Project (https://github.com/eaplatanios/nel_framework) developed at CMU.

## Requirements

- GCC 5+, Clang 5+, or Visual C++ 14+
- Python 2.7 or 3.5+
- Numpy
- OpenAI Gym (https://github.com/openai/gym).

## Installation Instructions

Assuming that you have Python installed in your system and
that you are located in the root directory of this
repository, run the following commands:

```bash
git submodule update --init --recursive
cd api/python
python setup.py install
```

Note that you should also install OpenAI Gym using `pip install gym`.

Make sure that the environment has successfully installed by running `python test/simulator_test.py` from the [api/python](api/python) directory. You should be able to see a visualizer of the grid-world environment along with the agent's visual input. If run successfullly, you should see the agent navigating through the environment.

## The NEL Environement
The environment (also called Jelly Bean World) consists of a 2-D grid world that an agent (blue triangle) can navigate in. The world contains three types of items (will be picked up by the agent by stepping on them) -
- `Jelly Beans` (<span style="color:blue">Blue</span>): Get `+20` reward by picking up a jelly bean.
- `Tongs` (<span style="color:red">Red</span>): Do not get any reward by picking up a pair of tongs, but tongs can be used to pick up a diamond in the future. Once a pair of tongs is used to pick up a diamond, it is no longer empty and cannot be used anymore.
- `Diamonds` (<span style="color:green">Green</span>): Get `+100` reward by picking up a diamond. The agent can only pick a diamond if it has an empty pair of tongs.

## Using the OpenAI Gym Interface

The action space consists of three actions:
  - `0`: Move forward.
  - `1`: Turn left.
  - `2`: Turn right.

The observation space consists of a dictionary:
  - `scent`: Vector of shape `[3]`. Each component corresponds to the scent of an item.
  - `vision`: Matrix with shape `[11, 11, 3]`. This is the RGB image that the agent sees.
  - `moved`: Binary value indicating whether the last 
    action resulted in the agent moving.

After installing the `nel` framework and `gym`, the 
provided environment can be used as follows:

```python
import gym
import nel

# Use 'NEL-render-v0' to include rendering support.
# Otherwise, use 'NEL-v0', which should be much faster.
env = gym.make('NEL-render-v0')

# The created environment can then be used as any other 
# OpenAI gym environment. For example:
for t in range(10000):
  # Render the current environment.
  env.render()
  # Sample a random action.
  action = env.action_space.sample()
  # Run a simulation step using the sampled action.
  observation, reward, _, _ = env.step(action)
```

NOTE: Please use `env = gym.make('NEL-v0')` while training your agent. This switches the renderer off and significantly speeds up simulation.

## Troubleshooting

### Repository initialization, publickey
If you get the message `Permission denied (publickey).` when initializing the
repository by calling `git submodule update --init --recursive` make sure you
have your public key set correctly in https://github.com/settings/keys. You can
see this example http://zeeelog.blogspot.com/2017/08/the-authenticity-of-host-githubcom.html
to generate a new one.
