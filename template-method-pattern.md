# Template Method Pattern

The Template Method Pattern defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

## Diagram

## Example
```python
from abc import ABC, abstractmethod


class Beverage(ABC):
    def prepare_recipe(self) -> None:
        self.boil_water()
        self.brew()
        self.pour_in_cup()
        if self.customer_wants_condiments():
            self.add_condiments()
    
    def boil_water(self) -> None:
        print('Boiling water...')
    
    def pour_in_cup(self) -> None:
        print('Pouring into cup...')
        
    def customer_wants_condiments(self) -> bool:
        return False
    
    @abstractmethod
    def brew(self) -> None:
        raise NotImplementedError
    
    @abstractmethod
    def add_condiments(self) -> None:
        raise NotImplementedError

class Tea(Beverage):
    def customer_wants_condiments(self) -> bool:
        ans = input('Would you like a lemon with your tea? (y/n)').lower()
        return ans == 'y'

    def brew(self) -> None:
        print('Steeping the tea...')
    
    def add_condiments(self) -> None:
        print('Adding lemon...')

class Coffee(Beverage):
    def customer_wants_condiments(self) -> bool:
        ans = input('Do you want to add milk and sugar? (y/n)').lower()
        return ans == 'y'

    def brew(self) -> None:
        print('Dripping Coffee through filter...')
    
    def add_condiments(self) -> None:
        print('Adding milk and sugar...')
```