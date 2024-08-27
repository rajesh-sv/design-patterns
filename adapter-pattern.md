# Adapter Pattern

The Adapter Pattern converts the interface of a class into another interface the clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

## Diagram

## Example

```python
from abc import ABC, abstractmethod


class Duck(ABC):
    @abstractmethod
    def quack(self) -> None:
        raise NotImplementedError
    
    @abstractmethod
    def fly(self) -> None:
        raise NotImplementedError

class MallardDuck(Duck):
    def quack(self) -> None:
        print('Quack!')
    
    def fly(self) -> None:
        print('I\'m flying!')


class Turkey(ABC):
    @abstractmethod
    def gobble(self) -> None:
        raise NotImplementedError
    
    @abstractmethod
    def fly(self) -> None:
        raise NotImplementedError

class WildTurkey(Turkey):
    def gobble(self) -> None:
        print('Gobble!')
    
    def fly(self) -> None:
        print('I\'m flying for a short distance!')
        

class TurkeyAdapter(Duck):
    def __init__(self, turkey: Turkey):
        self.turkey = turkey
        
    def quack(self) -> None:
        self.turkey.gobble()
    
    def fly(self) -> None:
        for _ in range(5):
            self.turkey.fly()
```
