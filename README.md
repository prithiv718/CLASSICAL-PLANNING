# Name: PRITHIV S
# Reg no:212224020043
# ExpNo:10 Implementation of Classical Planning Algorithm
# Algorithm or Steps Involved:
<ol>
  <li>Define the initial state</li>
  <li>Define the goal state</li>
  <li>Define the actions</li>
  <li>Find a <b>plan</b> to reach the goal state</li>
  <li>Print the plan</li>
</ol>

# Example - 1
```
initial_state = {'A': 'Table', 'B': 'Table'}
goal_state = {'A': 'B', 'B': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_Table': {'precondition': {'A': 'Table', 'B': 'B'}, 'effect': {'B': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```
# Output:
```
['move_A_to_B']
```
# Example - 2
```
initial_state = {'A': 'Table', 'B': 'Table', 'C': 'Table'}
goal_state = {'A': 'B', 'B': 'C', 'C': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_C': {'precondition': {'A': 'B', 'B': 'Table', 'C': 'Table'}, 'effect': {'B': 'C'}},
    'move_C_to_Table': {'precondition': {'A': 'B', 'B': 'C', 'C': 'C'}, 'effect': {'C': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```
# Output:
```
['move_A_to_B', 'move_B_to_C']
```

# Please Prepare Solution or Definition For the method find_plan(initial_state, goal_state, actions)
<h3>You Can use any of the searching Strategies for planning and executing a sequence of actions.<br> You can also look in to the Code given in the Repository.</h3>
# Check if current state is the goal state
def is_goal_state(current_state, goal_state):
    return current_state == goal_state


# Apply action effect to current state
def apply_action(current_state, action_effect):
    new_state = current_state.copy()
    new_state.update(action_effect)
    return new_state


# Check if action can be applied
def is_applicable(current_state, precondition):
    return all(current_state.get(key) == value for key, value in precondition.items())


# Find plan using BFS
def find_plan(initial_state, goal_state, actions):

    # Queue stores (state, plan)
    queue = [(initial_state, [])]

    visited_states = set()

    while queue:

        current_state, partial_plan = queue.pop(0)

        # Check goal
        if is_goal_state(current_state, goal_state):
            return partial_plan

        # Avoid revisiting states
        state_tuple = tuple(sorted(current_state.items()))
        if state_tuple in visited_states:
            continue

        visited_states.add(state_tuple)

        # Try all actions
        for action in actions:

            precondition = actions[action]['precondition']
            effect = actions[action]['effect']

            if is_applicable(current_state, precondition):

                next_state = apply_action(current_state, effect)

                queue.append((next_state, partial_plan + [action]))

    print("No plan exists.")
    return None


# ------------------ Example 1 ------------------

initial_state = {'A': 'Table', 'B': 'Table'}
goal_state = {'A': 'B', 'B': 'Table'}

actions = {

    'move_A_to_B': {
        'precondition': {'A': 'Table', 'B': 'Table'},
        'effect': {'A': 'B'}
    },

    'move_B_to_Table': {
        'precondition': {'A': 'Table', 'B': 'B'},
        'effect': {'B': 'Table'}
    }
}

plan = find_plan(initial_state, goal_state, actions)
print("Plan:", plan)


# ------------------ Example 2 ------------------

initial_state = {'A': 'Table', 'B': 'Table', 'C': 'Table'}
goal_state = {'A': 'B', 'B': 'C', 'C': 'Table'}

actions = {

    'move_A_to_B': {
        'precondition': {'A': 'Table', 'B': 'Table'},
        'effect': {'A': 'B'}
    },

    'move_B_to_C': {
        'precondition': {'A': 'B', 'B': 'Table', 'C': 'Table'},
        'effect': {'B': 'C'}
    },

    'move_C_to_Table': {
        'precondition': {'A': 'B', 'B': 'C', 'C': 'C'},
        'effect': {'C': 'Table'}
    }
}

plan = find_plan(initial_state, goal_state, actions)
print("Plan:", plan)
