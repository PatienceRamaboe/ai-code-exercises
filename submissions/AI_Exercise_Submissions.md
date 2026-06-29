# AI Exercise Submissions
**  Name:** Patience Ramaboe

---

# Exercise 1: Knowing Where to Start

## Initial Understanding
I selected the Python Task Manager implementation. From the project structure, I understood that it is a command-line application for creating, updating, deleting, and managing tasks. The application is organized into separate files for the command-line interface, business logic, models, storage, and testing.

## Technologies Used
- Python 3.11+
- JSON for data storage
- argparse
- unittest

## Main Components
- cli.py – Handles user commands.
- task_manager.py – Contains the main business logic.
- models.py – Defines Task, TaskStatus, and TaskPriority.
- storage.py – Reads and writes tasks to JSON.
- tests/ – Contains unit tests.

## Feature Location
For the "Export to CSV" feature, I would modify the storage layer because it already manages reading and writing task data. I would also add a new command in cli.py to allow users to export their tasks.

## Domain Model
The main entity is Task. Each task has a title, description, priority, status, due date, and tags. Status represents the current progress of the task, while priority determines its importance.

## New Business Rule
To automatically mark tasks as abandoned after being overdue for more than seven days, I would add a new status called ABANDONED and implement logic in task_manager.py that checks due dates before updating the task status. High-priority tasks would be excluded from this rule.

## Reflection
This exercise helped me understand how the project is organized and how AI can assist in quickly understanding an unfamiliar codebase. I also learned how different files work together to implement the application's functionality.

---


# Exercise 2: Codebase Exploration Challenge

## Part 1: Understanding a Specific Feature

### Task Creation and Status Updates

The files involved in task creation and status updates are:

- `cli.py` – Receives user commands.
- `task_manager.py` – Creates and updates tasks.
- `models.py` – Defines the Task model and task status.
- `storage.py` – Saves and loads tasks.

### Execution Flow

1. The user enters a command in the terminal.
2. `cli.py` processes the command.
3. `task_manager.py` performs the requested operation.
4. The task object is created or updated.
5. `storage.py` saves the updated tasks to the JSON file.

### Data Storage

Task information is stored in a JSON file. Whenever a task is added or updated, the storage layer writes the latest task list to disk.

### Design Pattern

The project separates responsibilities into different modules:
- CLI handles user interaction.
- Models represent the data.
- Task Manager contains business logic.
- Storage manages persistence.

---

## Part 2: Understanding Task Priorities

### Initial Understanding

I initially believed task priority was only used for displaying tasks.

### Final Understanding

After exploring the code, I discovered that priority is part of the Task model and is used throughout the application when managing tasks.

### Insights

- Priority is stored with every task.
- It can be updated.
- It helps organize work according to importance.

---

## Part 3: Data Flow

### Completing a Task

Data Flow:

User Command
↓
cli.py
↓
task_manager.py
↓
Task status updated
↓
storage.py
↓
tasks.json saved

### State Changes

- Status changes from TODO to DONE.
- Completion date is recorded.
- Updated task is saved.

### Possible Failure Points

- Invalid task ID.
- Missing JSON file.
- File write errors.
- Invalid status value.

---

## Part 4: Reflection

This exercise improved my understanding of how data flows through the application. Using AI prompts helped me identify the important files more quickly and understand how they work together.


# Exercise 3: Algorithm Deconstruction Challenge

## Algorithm Selected

I chose the task prioritization algorithm used in the Python Task Manager.

## Initial Understanding

At first, I understood that the algorithm calculates a score for each task based on different attributes and then sorts tasks by importance.

## How the Algorithm Works

1. Read all task information.
2. Calculate a priority score using factors such as priority level, due date, and task status.
3. Compare the scores.
4. Sort tasks from highest score to lowest score.
5. Return the ordered list of tasks.

## Inputs

- Task priority
- Due date
- Status
- Task metadata

## Outputs

- A sorted list of tasks from highest to lowest priority.

## Time Complexity

The scoring process runs once for each task, while sorting the list takes approximately O(n log n).

## Advantages

- Separates scoring from sorting.
- Easy to extend with new business rules.
- Makes important tasks appear first.

## Possible Improvements

- Allow users to customize scoring weights.
- Add more factors such as task owner or category.
- Cache scores for better performance when the task list is large.

## Reflection

Breaking the algorithm into smaller steps made it much easier to understand. AI helped explain both the overall logic and the purpose of each step without needing to read every line of code first.
