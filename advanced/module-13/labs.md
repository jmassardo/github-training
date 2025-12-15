---
layout: training-module
title: "Hands-On Labs"
permalink: /advanced/module-13/labs/
module_number: 13
module_title: "GitHub Copilot Fundamentals"
section_number: 8
total_sections: 10
phase: advanced
estimated_time: "45 min"
module_index: /advanced/module-13/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-13/overview/"
    short_title: "Overview"
    icon: "📋"
  - title: "Core Concepts"
    url: "/advanced/module-13/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "IDE Setup"
    url: "/advanced/module-13/setup/"
    short_title: "Setup"
    icon: "⚙️"
  - title: "Prompt Crafting"
    url: "/advanced/module-13/prompts/"
    short_title: "Prompts"
    icon: "✍️"
  - title: "Copilot Chat"
    url: "/advanced/module-13/chat/"
    short_title: "Chat"
    icon: "💬"
  - title: "IDE Differences"
    url: "/advanced/module-13/ide-differences/"
    short_title: "IDE Differences"
    icon: "🔀"
  - title: "Best Practices"
    url: "/advanced/module-13/best-practices/"
    short_title: "Best Practices"
    icon: "✅"
  - title: "Hands-On Labs"
    url: "/advanced/module-13/labs/"
    short_title: "Labs"
    icon: "🧪"
  - title: "Knowledge Check"
    url: "/advanced/module-13/quiz/"
    short_title: "Quiz"
    icon: "📝"
  - title: "Resources"
    url: "/advanced/module-13/resources/"
    short_title: "Resources"
    icon: "📖"
toc: true
prev_section:
  url: /advanced/module-13/best-practices/
  title: "Best Practices"
next_section:
  url: /advanced/module-13/quiz/
  title: "Knowledge Check"
---

## Lab Overview

These hands-on exercises will help you practice using GitHub Copilot effectively. Complete them in order, as each builds on skills from the previous lab.

### Prerequisites

- GitHub Copilot enabled in your IDE (VS Code recommended)
- Python 3.8+ or Node.js 18+ installed
- Basic programming knowledge

---

## Lab 1: First Steps with Completions

**Objective:** Get comfortable with inline completions and cycling through suggestions.

### Task 1.1: Comment-Driven Development

1. Create a new Python file called `string_utils.py`
2. Type the following comment and let Copilot complete the function:

```python
# Function to reverse a string without using built-in reverse methods
def reverse_string(s: str) -> str:

```

3. Press `Tab` to accept or `Alt+]` to see alternative suggestions
4. Test your function:

```python
print(reverse_string("hello"))  # Should print: olleh

```

### Task 1.2: Pattern Completion

1. Add another function using the same pattern:

```python
# Function to check if a string is a palindrome (reads same forwards and backwards)
def is_palindrome(s: str) -> bool:

```

2. Notice how Copilot uses context from your previous function

### Task 1.3: Multiple Suggestions

1. Start a new function:

```python
# Function to count vowels in a string
def count_vowels(s: str) -> int:

```

2. Use `Alt+]` and `Alt+[` to cycle through different implementation options
3. Choose the one you think is clearest

**✅ Success Criteria:** All three functions work correctly with various inputs.

---

## Lab 2: Copilot Chat Exploration

**Objective:** Use Copilot Chat to understand, explain, and modify code.

### Task 2.1: Code Explanation

1. Paste this code into a new file:

```python
def mystery(n):
    return n & (n - 1) == 0 and n != 0

```

2. Select the function and open Copilot Chat
3. Ask: "What does this function do? Explain the bitwise operation."
4. Based on the explanation, rename the function to something meaningful

### Task 2.2: Test Generation

1. Create a simple function:

```python
def calculate_discount(price: float, discount_percent: float) -> float:
    """Calculate the final price after applying a discount."""
    if discount_percent < 0 or discount_percent > 100:
        raise ValueError("Discount must be between 0 and 100")
    return price * (1 - discount_percent / 100)

```

2. Select the function and ask Chat: "/tests generate comprehensive tests for this function"
3. Review the generated tests—do they cover edge cases?

### Task 2.3: Code Refactoring

1. Ask Chat to refactor this code:

```python
def process_users(users):
    result = []
    for user in users:
        if user['active'] == True:
            if user['age'] >= 18:
                if user['email'] != None:
                    result.append(user)
    return result

```

2. Prompt: "Refactor this to use early returns and more Pythonic style"
3. Compare the original and refactored versions

**✅ Success Criteria:** You've successfully used Chat for explanation, test generation, and refactoring.

---

## Lab 3: Building a Complete Feature

**Objective:** Use Copilot to build a complete feature from scratch.

### Task 3.1: Define the Feature

You'll build a simple task manager with these capabilities:
- Add tasks with title and priority
- List all tasks
- Mark tasks as complete
- Filter by priority

### Task 3.2: Create the Data Model

1. Create `task_manager.py`
2. Start with this comment block:

```python
"""
Simple Task Manager
Features:
- Add tasks with title, priority (low, medium, high), and due date
- List tasks with optional filtering
- Mark tasks complete
- Persist to JSON file
"""

from dataclasses import dataclass
from datetime import datetime
from enum import Enum
from typing import Optional
import json

class Priority(Enum):
    LOW = 1
    MEDIUM = 2
    HIGH = 3

@dataclass
class Task:
    # Let Copilot suggest the fields

```

3. Let Copilot complete the dataclass with appropriate fields

### Task 3.3: Implement the Manager

```python
class TaskManager:
    def __init__(self, storage_file: str = "tasks.json"):
        # Copilot will suggest initialization
    
    def add_task(self, title: str, priority: Priority, due_date: Optional[datetime] = None) -> Task:
        """Add a new task and return it."""
        # Let Copilot implement
    
    def list_tasks(self, priority_filter: Optional[Priority] = None, show_completed: bool = True) -> list[Task]:
        """List tasks with optional filtering."""
        # Let Copilot implement
    
    def complete_task(self, task_id: int) -> bool:
        """Mark a task as complete. Returns True if successful."""
        # Let Copilot implement
    
    def _save(self) -> None:
        """Persist tasks to JSON file."""
        # Let Copilot implement
    
    def _load(self) -> None:
        """Load tasks from JSON file."""
        # Let Copilot implement

```

### Task 3.4: Test Your Implementation

Create a test script:

```python
# Test the task manager
manager = TaskManager()

# Add some tasks
task1 = manager.add_task("Review PRs", Priority.HIGH)
task2 = manager.add_task("Update documentation", Priority.MEDIUM)
task3 = manager.add_task("Refactor tests", Priority.LOW)

# List all tasks
print("All tasks:")
for task in manager.list_tasks():
    print(f"  {task}")

# Complete a task
manager.complete_task(task1.id)

# List high priority incomplete tasks
print("\nHigh priority incomplete:")
for task in manager.list_tasks(priority_filter=Priority.HIGH, show_completed=False):
    print(f"  {task}")

```

**✅ Success Criteria:** Task manager works with all operations and persists data correctly.

---

## Lab 4: Real-World Scenario

**Objective:** Simulate helping a customer demonstrate Copilot value.

### Scenario

A customer is skeptical about Copilot. They want to see a quick demonstration of how it can help their team.

### Task 4.1: Quick Demo Setup

Create a demo that shows:

1. **Speed** - Generate a REST API endpoint in under 2 minutes
2. **Quality** - Generate tests automatically
3. **Learning** - Use Chat to explain unfamiliar code

### Demo Script

```python
# Demo: Create a Flask API endpoint for user management
# Show: Comment-driven development, test generation, code explanation

from flask import Flask, request, jsonify
from typing import Optional

app = Flask(__name__)

# In-memory database for demo
users_db = {}

# Create a new user
# Accepts JSON with: name, email, role (optional, defaults to "user")
# Returns: created user with generated ID
@app.route('/users', methods=['POST'])
def create_user():
    # Let Copilot generate

```

### Task 4.2: Generate Tests

Ask Chat: "Generate pytest tests for this Flask API including happy path and error cases"

### Task 4.3: Explain to "Customer"

Select a complex part of the generated code and use Chat's `/explain` to prepare talking points.

**✅ Success Criteria:** You can confidently demonstrate Copilot's value in a simulated customer meeting.

---

## Lab 5: Advanced Chat Features

**Objective:** Master advanced Chat capabilities.

### Task 5.1: @workspace Queries

If using VS Code, try these queries on a project with multiple files:

```
@workspace What testing framework does this project use?

@workspace Find all functions that make HTTP requests

@workspace How is error handling implemented in this project?

```

### Task 5.2: Multi-Turn Conversation

Have a progressive conversation:

1. "I need to implement rate limiting for an API"
2. "Make it configurable per endpoint"
3. "Add support for different time windows (minute, hour, day)"
4. "Generate tests for all scenarios"

### Task 5.3: Code Review Simulation

Paste this code and ask Chat to review it:

```python
def get_user_data(user_id):
    import requests
    url = "http://api.example.com/users/" + str(user_id)
    response = requests.get(url, timeout=30)
    data = eval(response.text)
    password = data['password']
    return data

```

Prompt: "Review this code for security issues, performance problems, and best practices violations"

**✅ Success Criteria:** You've successfully used @workspace and multi-turn conversations.

---

## Lab Reflection

After completing these labs, consider:

1. **What worked well?** Which prompting techniques gave the best results?
2. **What surprised you?** Any unexpected capabilities or limitations?
3. **What would you show customers?** Which demos are most compelling?
4. **What would you change?** How would you improve your workflow?

---

## Bonus Challenges

If you finish early:

1. **Language Swap** - Redo Lab 3 in JavaScript/TypeScript
2. **Framework Integration** - Add a SQLAlchemy backend to the task manager
3. **CLI Interface** - Add a command-line interface using Click or argparse
4. **Documentation** - Use Copilot to generate comprehensive README for your code

---

## Next Steps

Test your knowledge with the quiz in the next section.
