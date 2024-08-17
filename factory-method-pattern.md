# Factory Method Pattern

The Factory Method Pattern defines an interface for creating an object, but lets subclasses decide which class to instantiate.

Factory Method relies on inheritance: object creation is delegated to subclasses, which implement the factory method to create objects.

## Diagram

## Example

```python
from abc import ABC, abstractmethod
from typing import List

class Pizza(ABC):
    name: str
    dough: str
    sauce: str
    toppings: List[str]

    def prepare(self) -> None:
        print(f'Preparing {self.name}...')
        print(f'Tossing {self.dough}...')
        print(f'Spreading {self.sauce}...')
        print('Adding toppings:')
        for topping in self.toppings:
            print(f'    {topping}')

    def bake(self) -> None:
        print('Baking...')

    def cut(self) -> None:
        print('Cutting...')

    def box(self) -> None:
        print('Packing...')

class NYStyleCheesePizza(Pizza):
    def __init__(self):
        self.name = "NY Style Sauce and Cheese Pizza"
        self.dough = "Thin Crust Dough"
        self.sauce = "Marinara Sauce"
        self.toppings = ["Grated Reggiano Cheese"]

class ChicagoStyleVeggiePizza(Pizza):
    def __init__(self):
        self.name = "Chicago Deep Dish Veggie Pizza"
        self.dough = "Extra Thick Crust Dough"
        self.sauce = "Plum Tomato Sauce"
        self.toppings = ["Shredded Mozzarella Cheese", "Black Olives", "Spinach", "Eggplant"]


class PizzaStore(ABC):
    def order_pizza(self, type_: str) -> Pizza:
        pizza: Pizza = self.create_pizza(type_)
        pizza.prepare()
        pizza.bake()
        pizza.cut()
        pizza.box()
        return pizza

    @abstractmethod
    def create_pizza(self, type_: str) -> Pizza:
        raise NotImplementedError

class NYPizzaStore(PizzaStore):
    def create_pizza(self, type_: str) -> Pizza:
        if type_ == "cheese":
            return NYStyleCheesePizza()
        elif type_ == "veggie":
            return NYStyleVeggiePizza()
        elif type_ == "clam":
            return NYStyleClamPizza()
        elif type_ == "pepperoni":
            return NYStylePepperoniPizza()
        else:
            return None

class ChicagoPizzaStore(PizzaStore):
    def create_pizza(self, type_: str) -> Pizza:
        if type_ == "cheese":
            return ChicagoStyleCheesePizza()
        elif type_ == "veggie":
            return ChicagoStyleVeggiePizza()
        elif type_ == "clam":
            return ChicagoStyleClamPizza()
        elif type_ == "pepperoni":
            return ChicagoStylePepperoniPizza()
        else:
            return None
```
