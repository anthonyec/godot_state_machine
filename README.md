# Godot State Machine

Simple code only state machine that's is originally based off [GDQuest's implementation](https://www.gdquest.com/tutorial/godot/design-patterns/finite-state-machine/).

## Features (or problems)

- States are nodes
- Nested states, but only 1 level (parent and child)
- Support for deferred state transitions
- Bulk of the logic is in one file (under 200 lines)

## Usage

### State node
```gd
# Invoked when transitioning to this state. Used for setup and
# handling any paramaters passed from the previous state.
func enter(params: Dictionary) -> void:
	pass

# Invoked when transitioning to another state. Generally used
# for cleanup, it also will yield if `await` is used inside of it.
func exit() -> void:
	pass

# Equivalent to `_ready`.
func awake() -> void:
	pass

# Equivalent to `_process`.
func update(delta: float) -> void:
	pass

# Equivalent to `_physics_process`.
func physics_update(delta: float) -> void:
	pass

# Equivalent to `_input`.
func handle_input(event: InputEvent) -> void:
	pass

# Handle messages that have been sent to the state machine.
#
# This is usefull for when something external wants to change
# the state, but avoids forefully transitioning by using
# `transition_to` directly.
func handle_message(title: String, _params: Dictionary) -> void:
	pass

```

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

### Transition to the previous state

```gs
# Transition to whatever previous state invoked the current state.
state_machine.transition_to_previous_state()
```
