# Task Management App

This project is a Task Management Application that allows users to manage their tasks efficiently. It includes features such as adding, removing, listing tasks, and recommending tasks based on their priority using a machine learning model.

## Features

- **Add Task**: Add a new task with a description and priority.
- **Remove Task**: Remove a task by its description.
- **List Tasks**: Display all the tasks in the list.
- **Recommend Task**: Recommend a high-priority task using a simple machine learning model.

## Requirements

To run this application, ensure you have the following Python libraries installed:
- pandas
- scikit-learn

You can install these dependencies using pip:
```bash
pip install pandas scikit-learn
```

## Usage

1. **Clone the repository to your local machine:**
    ```bash
    git clone https://github.com/your_username/task-management-app.git
    ```

2. **Navigate to the project directory:**
    ```bash
    cd task-management-app
    ```

3. **Run the application:**
    ```bash
    python task_management_app.py
    ```

## Code Explanation

### Initialization

```python
import pandas as pd
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.naive_bayes import MultinomialNB
from sklearn.pipeline import make_pipeline
import random

# Initialize an empty task list
tasks = pd.DataFrame(columns=['description', 'priority'])

# Load pre-existing tasks from a CSV file (if any)
try:
    tasks = pd.read_csv('tasks.csv')
except FileNotFoundError:
    pass
```

### Save Tasks to CSV

```python
# Function to save tasks to a CSV file
def save_tasks():
    tasks.to_csv('tasks.csv', index=False)
```

### Train the Task Priority Classifier

```python
# Train the task priority classifier
vectorizer = CountVectorizer()
clf = MultinomialNB()
model = make_pipeline(vectorizer, clf)
model.fit(tasks['description'], tasks['priority'])
```

### Add Task

```python
# Function to add a task to the list
def add_task(description, priority):
    global tasks  # Declare tasks as a global variable
    new_task = pd.DataFrame({'description': [description], 'priority': [priority]})
    tasks = pd.concat([tasks, new_task], ignore_index=True)
    save_tasks()
```

### Remove Task

```python
# Function to remove a task by description
def remove_task(description):
    global tasks  # Declare tasks as a global variable
    tasks = tasks[tasks['description'] != description]
    save_tasks()
```

### List Tasks

```python
# Function to list all tasks
def list_tasks():
    if tasks.empty:
        print("No tasks available.")
    else:
        print(tasks)
```

### Recommend Task

```python
# Function to recommend a task based on machine learning
def recommend_task():
    if not tasks.empty:
        # Get high-priority tasks
        high_priority_tasks = tasks[tasks['priority'] == 'High']
        
        if not high_priority_tasks.empty:
            # Choose a random high-priority task
            random_task = random.choice(high_priority_tasks['description'])
            print(f"Recommended task: {random_task} - Priority: High")
        else:
            print("No high-priority tasks available for recommendation.")
    else:
        print("No tasks available for recommendations.")
```

### Main Menu

```python
# Main menu
while True:
    print("\nTask Management App")
    print("1. Add Task")
    print("2. Remove Task")
    print("3. List Tasks")
    print("4. Recommend Task")
    print("5. Exit")

    choice = input("Select an option: ")

    if choice == "1":
        description = input("Enter task description: ")
        priority = input("Enter task priority (Low/Medium/High): ").capitalize()
        add_task(description, priority)
        print("Task added successfully.")

    elif choice == "2":
        description = input("Enter task description to remove: ")
        remove_task(description)
        print("Task removed successfully.")

    elif choice == "3":
        list_tasks()

    elif choice == "4":
        recommend_task()

    elif choice == "5":
        print("Goodbye!")
        break

    else:
        print("Invalid option. Please select a valid option.")
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

Contributions are welcome! If you would like to contribute, please fork the repository and submit a pull request. For major changes, it's recommended to open an issue first to discuss the changes. If you find this project useful, consider giving it a star!