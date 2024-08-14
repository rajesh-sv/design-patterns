# Strategy Pattern

The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

## Diagram

## Example

```python
from abc import ABC, abstractmethod


class FlyBehaviour(ABC):
    @abstractmethod
    def fly(self) -> None:
        raise NotImplementedError

class FlyWithWings(FlyBehaviour):
    def fly(self) -> None:
        print('Flying with wings')

class FlyNoWay(FlyBehaviour):
    def fly(self) -> None:
        print('Cannot fly')


class QuackBehaviour(ABC):
    @abstractmethod
    def quack(self) -> None:
        raise NotImplementedError

class Quack(QuackBehaviour):
    def quack(self) -> None:
        print('Quack!')

class Squeak(QuackBehaviour):
    def quack(self) -> None:
        print('Squeak!')


class Duck(ABC):
    fly_behaviour: FlyBehaviour
    quack_behaviour: QuackBehaviour

    def swim(self) -> None:
        print('Swimming')

    def perform_fly(self) -> None:
        self.fly_behaviour.fly()

    def perform_quack(self) -> None:
        self.quack_behaviour.quack()

    def set_fly_behaviour(self, fly_behaviour: FlyBehaviour) -> None:
        self.fly_behaviour = fly_behaviour

    def set_quack_behaviour(self, quack_behaviour: QuackBehaviour) -> None:
        self.quack_behaviour = quack_behaviour

    @abstractmethod
    def display(self) -> None:
        raise NotImplementedError

class MallardDuck(Duck):
    def __init__(self):
        self.fly_behaviour = FlyWithWings()
        self.quack_behaviour = Quack()

    def display(self) -> None:
        print('Mallard Duck')

class Rubber(Duck):
    def __init__(self):
        self.fly_behaviour = FlyNoWay()
        self.quack_behaviour = Squeak()

    def display(self) -> None:
        print('Rubber Duck')
```
