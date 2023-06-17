# Godot State Machine

Simple code only state machine that's is originally based off [GDQuest's implementation](https://www.gdquest.com/tutorial/godot/design-patterns/finite-state-machine/).

## Features (or problems)

- States are nodes
- Nested states, but only 1 level (parent and child)
- Support for deferred state transitions
- Bulk of the logic is in one file (under 200 lines)

## Usage

### Transition to a state

```gd
# Transition to a state using the name of the node.
state_machine.transition_to("Jump")

# Additional parameters can be passed along to the next state, which helps avoids globals.
state_machine.transition_to("Jump", {
  "height": 100
})
```

### Deffered transition

```gd
# This will queue a state change, waiting for one physics frame to have been completed before transitioning.
state_machine.deffered_transition_to("Jump")

# Parameters are passed as a callback to ensure that when the transition occurs, any variable references in the params are the latest values.
state_machine.deffered_transition_to("Jump", func():
  return {
    "velocity": velocity
  }
)
```
