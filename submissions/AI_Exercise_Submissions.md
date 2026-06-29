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
