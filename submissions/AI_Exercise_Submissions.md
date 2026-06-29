 Exercise: Code Comprehension – Knowing Where to Start
Codebase: Task Manager (Python)
** Name:** Patience Ramaboe


PART 1: Understanding Project Structure
Initial Observations (Before AI Prompt)

By looking at the files, I noticed:

cli.py imports from task_manager, models, and storage, so there are at least 3 modules
task_manager.py imports from models and storage
The project uses only Python standard library (argparse, datetime)
Tasks are stored in a JSON file (tasks.json)

My initial guess at the structure was:

cli.py → handles user commands
task_manager.py → business logic
models.py → Task, TaskStatus, TaskPriority classes
storage.py → reads/writes tasks.json
AI Prompt Used

"I have just joined a team maintaining a Python Task Manager CLI app. It has these files: cli.py (entry point using argparse), task_manager.py (business logic), models.py (Task, TaskStatus, TaskPriority), and storage.py (JSON file storage). No external dependencies. Can you explain the architectural pattern, each file's responsibility, the main entry points, and how data flows when a user runs a command?"

AI Analysis Result

The project follows a 3-layer architecture:

USER INPUT
→ cli.py (presentation layer: parses commands, formats output)
→ task_manager.py (business logic: validation and coordination)
→ storage.py (data layer: reads/writes tasks.json)
→ models.py (domain layer: Task, enums, data structures)

Each file’s responsibility
cli.py: Handles command-line arguments using argparse and displays output
task_manager.py: Contains core logic like creating tasks, filtering, updating status
models.py: Defines Task, TaskStatus, and TaskPriority
storage.py: Handles saving/loading tasks from tasks.json
Key entry point

cli.py → main() function

Data flow example

When running:
python cli.py create "Fix bug"

cli.py parses the command
Calls task_manager.create_task()
Task object is created from models.py
storage.py saves it to tasks.json
What I misunderstood initially
I thought everything was inside task_manager.py
I didn’t realise there was a separate storage.py and models.py
I also assumed TaskManager handled storage directly, but it delegates that responsibility
PART 2: Task Export to CSV Feature
What I found in the codebase
storage.py already handles JSON file operations
task_manager.py already has methods that collect task data
cli.py already follows a consistent command pattern

No CSV export feature exists yet.

AI Prompt Used

"I need to add a Task Export to CSV feature. The app has cli.py, task_manager.py, models.py, and storage.py. Where should I implement it and what standard library tools should I use?"

Where the feature goes

task_manager.py
Add a method:

def export_to_csv(self, filename="tasks_export.csv"):
    import csv
    tasks = self.storage.get_all_tasks()

    with open(filename, "w", newline="") as f:
        writer = csv.writer(f)

        writer.writerow(["ID", "Title", "Description", "Status",
                         "Priority", "Due Date", "Tags",
                         "Created At", "Completed At"])

        for task in tasks:
            writer.writerow([
                task.id,
                task.title,
                task.description,
                task.status.value,
                task.priority.name,
                task.due_date.strftime("%Y-%m-%d") if task.due_date else "",
                ", ".join(task.tags),
                task.created_at.strftime("%Y-%m-%d %H:%M"),
                task.completed_at.strftime("%Y-%m-%d %H:%M") if task.completed_at else ""
            ])

    return filename

cli.py
Add a new command:

export_parser = subparsers.add_parser("export", help="Export tasks to CSV")
export_parser.add_argument("-f", "--file", default="tasks_export.csv")

elif args.command == "export":
    filename = task_manager.export_to_csv(args.file)
    print(f"Tasks exported to {filename}")

Only task_manager.py and cli.py are affected.

PART 3: Domain Model Understanding
Task entity
id: unique identifier
title: short name
description: details
status: current workflow state
priority: urgency level
due_date: deadline
tags: labels
created_at: creation timestamp
completed_at: completion timestamp
TaskStatus

Workflow:
TODO → IN_PROGRESS → REVIEW → DONE
Values: todo, in_progress, review, done

TaskPriority

LOW (1) → MEDIUM (2) → HIGH (3) → URGENT (4)

Key behaviour

When a task becomes DONE, mark_as_done() sets completed_at.

AI reflection questions

1. due_date vs completed_at
Due date is when it should be done, completed_at is when it was actually finished.

2. Why use mark_as_done()?
Because setting DONE triggers extra logic (saving timestamp), not just a status change.

3. What is overdue?
A task past its due date and not completed.

PART 4: New Business Rule

Rule:
Tasks overdue by more than 7 days should be marked as abandoned unless they are high priority.

Changes needed
models.py → add ABANDONED status
task_manager.py → add rule logic
cli.py → add command trigger
Logic (task_manager.py)
def mark_abandoned_overdue_tasks(self):
    from datetime import datetime, timedelta

    cutoff = datetime.now() - timedelta(days=7)
    count = 0

    tasks = self.storage.get_all_tasks()

    for task in tasks:
        is_old = task.due_date and task.due_date < cutoff
        is_active = task.status not in (TaskStatus.DONE, TaskStatus.ABANDONED)
        is_low_priority = task.priority not in (TaskPriority.HIGH, TaskPriority.URGENT)

        if is_old and is_active and is_low_priority:
            task.status = TaskStatus.ABANDONED
            count += 1

    if count:
        self.storage.save()

    return count
CLI command
subparsers.add_parser("abandon", help="Auto-abandon overdue low priority tasks")

elif args.command == "abandon":
    count = task_manager.mark_abandoned_overdue_tasks()
    print(f"{count} tasks marked as abandoned.")
Questions before implementation
Should abandoned tasks show in default lists?
Should this run automatically or manually?
Is HIGH only high priority, or does it include URGENT?
Should we log abandoned tasks?
Final Reflection
Before vs After
Thought there were 2 files → actually 4 modules
Assumed storage was inside TaskManager → it’s separated
Didn’t understand enums → now clear workflow states
Didn’t see side effects → DONE triggers timestamp logic
Most valuable insight

Understanding the architecture first made everything else easier.

Best strategy for new codebases
Look at file structure first
Identify architecture pattern
Trace one full user action
Understand domain model before coding
Use AI to confirm, not replace understanding
