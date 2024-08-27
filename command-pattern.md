# Command Pattern

The Command Pattern encapsulates a request as an object, thereby letting you parameterize other objects with different requests, queue or log requests, and support undo-able operations.

The Command Pattern decouples an object making a request from the one that knows how to perform it. 

## Diagram

## Example
```python
from abc import ABC, abstractmethod
from typing import List


class Receiver1:
    def on(self) -> None:
        print('ON')
    
    def off(self) -> None:
        print('OFF')

class Receiver2:
    def start(self) -> None:
        print('START')
    
    def stop(self) -> None:
        print('STOP')


class Command(ABC):
    @abstractmethod
    def execute(self) -> None:
        raise NotImplementedError
    
    @abstractmethod
    def undo(self) -> None:
        raise NotImplementedError

class Receiver1OnCommand(Command):
    def __init__(self, receiver1: Receiver1):
        self.receiver1: Receiver1 = receiver1
    
    def execute(self) -> None:
        self.receiver1.on()
    
    def undo(self) -> None:
        self.receiver1.off()

class Receiver1OffCommand(Command):
    def __init__(self, receiver1: Receiver1):
        self.receiver1: Receiver1 = receiver1
    
    def execute(self) -> None:
        self.receiver1.off()
    
    def undo(self) -> None:
        self.receiver1.on()
        
class Receiver2OnCommand(Command):
    def __init__(self, receiver2: Receiver2):
        self.receiver2: Receiver2 = receiver2
    
    def execute(self) -> None:
        self.receiver2.start()
    
    def undo(self) -> None:
        self.receiver2.stop()

class Receiver2OffCommand(Command):
    def __init__(self, receiver2: Receiver2):
        self.receiver2: Receiver2 = receiver2
    
    def execute(self) -> None:
        self.receiver2.stop()
    
    def undo(self) -> None:
        self.receiver2.start()

class MacroCommand(Command):
    def __init__(self, commands: List[Command]):
        self.commands = commands
    
    def execute(self) -> None:
        for command in self.commands:
            command.execute()
    
    def undo(self) -> None:
        for command in reversed(self.commands):
            command.undo()
        
class NoCommand(Command):
    def execute(self) -> None:
        pass
    
    def undo(self) -> None:
        pass
    

class Invoker:
    def __init__(self):
        self.on_slots: List[Command] = [NoCommand() for _ in range(10)]
        self.off_slots: List[Command] = [NoCommand() for _ in range(10)]
        self.stack: List[Command] = []
    
    def set_command(self, i: int, on_command: Command, off_command: Command) -> None:
        self.on_slots[i] = on_command
        self.off_slots[i] = off_command
        
    def invoke_on(self, i: int) -> None:
        self.on_slots[i].execute()
        self.stack.append(self.on_slots[i])
    
    def invoke_off(self, i: int) -> None:
        self.off_slots[i].execute()
        self.stack.append(self.off_slots[i])
    
    def undo(self) -> None:
        self.stack.pop().undo()
```