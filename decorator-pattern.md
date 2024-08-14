# Decorator Pattern

The Decorator Pattern attaches additional responsibilities to an object dynamically. It provides a flexible alternative to subclassing for extending functionality.

Decorators are not a good choice when some client code depends of specific types.

## Diagram

## Example

```python
from abc import ABC, abstractmethod


class Beverage(ABC):
    description: str = 'Unknown Beverage'

    def get_description(self) -> str:
        return self.description

    @abstractmethod
    def cost(self) -> float:
        raise NotImplementedError

class Espresso(Beverage):
    def __init__(self):
        self.description = 'Espresso'
        self.price = 99.0

    def cost(self) -> float:
        return self.price

class Mocha(Beverage):
    def __init__(self):
        self.description = 'Mocha'
        self.price = 119.0

    def cost(self) -> float:
        return self.price


class CondimentDecorator(Beverage):
    beverage: Beverage

    def get_description(self) -> str:
        return self.beverage.get_description() + ', ' + self.description

    def cost(self) -> float:
        return self.price + self.beverage.cost()

class Milk(CondimentDecorator):
    def __init__(self, beverage: Beverage):
        self.beverage = beverage
        self.description = 'Milk'
        self.price = 19.0

class Whip(CondimentDecorator):
    def __init__(self, beverage: Beverage):
        self.beverage = beverage
        self.description = 'Whip'
        self.price = 29.0
```
