# MVC Pattern

Model: The model holds all the data, state, and application logic. The model is oblivious to the view and controller, although it provides an interface to manipulate and retrieve its state and it can send notifications of state changes to observers.

View: Gives you a presentation of the model. The view usually gets the state and data it needs to display directly from the model.

Controller: The controller separates the logic of control from the view and decouples the view from the model. Basically, it takes user input and figures out what it means to the model.

MVC as a set of patterns:

- The model uses Observer to keep the views and controllers updated on the latest state changes.
- The view and the controller implement the Strategy Pattern. The controller is the strategy of the view, and it can be easily exchanged with another controller if you want different behavior.
- The view itself also uses the Composite Pattern internally to manage the windows, buttons, and other components of the display.
- The Adapter Pattern can be used to adapt a new model to an existing view and controller.

## Diagram

## Example

```python
class TaskModel:
    def __init__(self):
        self.tasks = []
    def add_task(self, task):
        self.tasks.append(task)
    def get_tasks(self):
        return self.tasks

class TaskView:
    def display_tasks(self, tasks):
        print("Tasks:")
        for task in tasks:
            print(f"- {task}")

class TaskController:
    def __init__(self, model, view):
        self.model = model
        self.view = view
    def add_task(self, task):
        self.model.add_task(task)
        self.update_view()
    def update_view(self):
        tasks = self.model.get_tasks()
        self.view.display_tasks(tasks)
```
