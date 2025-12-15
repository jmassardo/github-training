---
layout: training-module
title: "Prompt Crafting"
permalink: /advanced/module-13/prompts/
module_number: 13
module_title: "GitHub Copilot Fundamentals"
section_number: 4
total_sections: 10
phase: advanced
estimated_time: "30 min"
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
  url: /advanced/module-13/setup/
  title: "IDE Setup"
next_section:
  url: /advanced/module-13/chat/
  title: "Copilot Chat"
---

## The Art of Prompting

Getting great suggestions from Copilot is like giving instructions to a skilled but literal-minded assistant. The more specific and clear you are, the better the results.

### The Prompting Mindset

Think of prompts as communication:
- **Vague prompt** → Generic suggestion
- **Specific prompt** → Targeted suggestion
- **Contextual prompt** → Brilliant suggestion

> **💡 Key Insight:** Copilot doesn't read your mind—it reads your code, comments, and context. Make that information count.

---

## Comment-Driven Development

Comments are your primary tool for guiding Copilot. Write comments that describe what you want, then let Copilot implement it.

### Basic Comment Prompting

```python
# Calculate the factorial of a number recursively
def factorial(n):
    # Copilot will suggest the implementation

```

### Detailed Comment Prompting

```python
# Calculate the factorial of a number
# - Use recursion
# - Handle negative numbers by returning None
# - Handle 0 and 1 as base cases
# - Include type hints
def factorial(n: int) -> int | None:
    # Much better suggestion coming!

```

### Multi-Line Documentation

```python
def process_user_data(users: list[dict]) -> dict:
    """
    Process a list of user dictionaries and return aggregated statistics.
    
    Args:
        users: List of user dicts with keys: 'name', 'age', 'city', 'active'
    
    Returns:
        Dictionary with:
        - total_users: int
        - average_age: float
        - users_by_city: dict mapping city names to counts
        - active_percentage: float (0-100)
    
    Example:
        >>> process_user_data([{'name': 'Alice', 'age': 30, 'city': 'NYC', 'active': True}])
        {'total_users': 1, 'average_age': 30.0, ...}
    """
    # Copilot now has excellent context for implementation

```

---

## Naming as Context

Variable and function names provide semantic context. Meaningful names lead to meaningful suggestions.

### Poor Names → Poor Suggestions

```javascript
// Bad: Copilot has no idea what this does
function fn(x, y) {
  // ??? 
}

```

### Descriptive Names → Better Suggestions

```javascript
// Good: Names tell Copilot what to do
function calculateShippingCost(orderWeight, destinationZone) {
  // Copilot will suggest relevant shipping logic
}

```

### Domain-Specific Names

```python
# Financial domain hints
def calculate_compound_interest(principal, annual_rate, years, compounds_per_year):
    # Copilot recognizes this is finance-related

# Healthcare domain hints  
def calculate_bmi(weight_kg, height_m):
    # Copilot knows this is medical calculation

```

---

## Examples as Prompts

Showing Copilot what you want through examples is incredibly powerful.

### Input/Output Examples

```python
# Convert camelCase to snake_case
# Examples:
#   "helloWorld" -> "hello_world"
#   "getUserById" -> "get_user_by_id"
#   "HTTPServer" -> "http_server"
def camel_to_snake(s: str) -> str:
    # Copilot now understands edge cases like HTTPServer

```

### Pattern Examples

```javascript
// API endpoints following REST conventions:
// GET /users -> getAllUsers()
// GET /users/:id -> getUserById(id)
// POST /users -> createUser(data)
// PUT /users/:id -> updateUser(id, data)
// DELETE /users/:id -> deleteUser(id)

async function getAllUsers() {
  // Copilot will follow the pattern for subsequent functions
}

```

---

## Structuring Complex Requests

For complex functionality, break down your prompt into clear steps.

### Step-by-Step Comments

```python
def sync_inventory(source_db, target_db):
    """
    Synchronize inventory between two databases.
    
    Steps:
    1. Fetch all items from source database
    2. Fetch existing items from target database
    3. Identify new, updated, and deleted items
    4. Apply changes to target in a transaction
    5. Log sync results
    6. Return summary of changes
    """
    # Copilot will follow this structure

```

### Constraint Specification

```python
# Parse a date string into a datetime object
# Constraints:
# - Support formats: "YYYY-MM-DD", "MM/DD/YYYY", "DD-Mon-YYYY"
# - Return None for invalid dates
# - Don't use dateutil (use only stdlib)
# - Timezone-naive output
def parse_date(date_string: str) -> datetime | None:

```

---

## Language-Specific Techniques

### Python: Type Hints

```python
# Type hints dramatically improve suggestions
def process_data(
    items: list[dict[str, Any]],
    filter_fn: Callable[[dict], bool],
    transform_fn: Callable[[dict], T]
) -> list[T]:

```

### JavaScript/TypeScript: JSDoc and Types

```typescript
/**
 * Fetches user data with retry logic
 * @param userId - The user's unique identifier
 * @param options - Fetch options including retries and timeout
 * @returns Promise resolving to user data or null if not found
 */
async function fetchUserWithRetry(
  userId: string,
  options: { retries?: number; timeout?: number }
): Promise<User | null> {

```

### SQL: Schema Context

```sql
-- Table: orders (id, user_id, total, status, created_at)
-- Table: users (id, name, email, tier)
-- Table: products (id, name, price, category)

-- Get top 10 customers by total order value this month
SELECT 

```

---

## Prompting Patterns

### The "Similar To" Pattern

```python
# Similar to requests.get() but with automatic retry and exponential backoff
def resilient_get(url, max_retries=3, base_delay=1):

```

### The "Like X but Y" Pattern

```javascript
// Like Array.prototype.map but runs promises in parallel with concurrency limit
async function mapWithConcurrency(items, asyncFn, limit = 5) {

```

### The "Given/When/Then" Pattern

```python
# Given: a list of transactions with 'amount' and 'category' fields
# When: calculating monthly summaries
# Then: return dict with category totals and percentage of total spend
def calculate_monthly_summary(transactions: list[dict]) -> dict:

```

---

## What NOT to Do

### Avoid Overly Vague Prompts

```python
# Bad: What does "process" mean?
def process(data):

# Good: Be specific
def normalize_and_validate_user_input(raw_form_data: dict) -> ValidatedUser:

```

### Avoid Conflicting Instructions

```python
# Bad: Contradictory requirements
# Parse JSON without using json module and return parsed JSON
def parse_json(s):  # Confusing!

# Good: Clear single approach
# Parse JSON manually for simple objects (no nested structures)
# Return dict with string keys and string/int values
def parse_simple_json(s: str) -> dict[str, str | int]:

```

### Avoid Assuming External Knowledge

```python
# Bad: Assumes Copilot knows your internal API
def call_internal_api(endpoint):

# Good: Provide context about the API
# Call internal REST API at BASE_URL
# - Add Authorization header with API_KEY env var
# - Parse JSON response
# - Raise on non-200 status
def call_internal_api(endpoint: str) -> dict:

```

---

## Prompt Templates

### API Function Template

```python
def <action>_<resource>(
    <required_params>,
    <optional_params>=<default>
) -> <return_type>:
    """
    <Brief description of what this does>
    
    Args:
        <param>: <description>
    
    Returns:
        <description of return value>
    
    Raises:
        <ExceptionType>: <when this is raised>
    
    Example:
        >>> <example usage>
        <expected output>
    """

```

### Test Function Template

```python
def test_<function_name>_<scenario>():
    """Test that <function> correctly handles <scenario>."""
    # Arrange
    <setup test data>
    
    # Act
    <call function under test>
    
    # Assert
    <verify expected outcome>

```

---

## Summary

| Technique | Purpose | Example |
|-----------|---------|---------|
| **Comments** | Describe intent | `# Calculate using recursion` |
| **Naming** | Provide semantic context | `calculateTaxWithDeductions` |
| **Examples** | Show expected behavior | `# "hello" -> "HELLO"` |
| **Type hints** | Clarify data shapes | `list[dict[str, int]]` |
| **Docstrings** | Full specification | Args, Returns, Examples |
| **Constraints** | Limit solution space | `# Don't use external libs` |

---

## Next Steps

Now that you can craft effective prompts, let's explore Copilot Chat for interactive AI assistance.
