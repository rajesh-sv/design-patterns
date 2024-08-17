# Abstract Factory Pattern

The Abstract Factory Pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes.

Abstract Factory relies on object composition: object creation is implemented in methods exposed in the factory interface.

## Diagram

## Example

```python
from abc import ABC, abstractmethod
from typing import List


class Dough(ABC):
    dough: str
    def get_dough(self) -> str:
        return self.dough
class ThickCrustDough(Dough):
    def __init__(self):
        self.dough = 'Thick Crust Dough'
class ThinCrustDough(Dough):
    def __init__(self):
        self.dough = 'Thin Crust Dough'


class Cheese(ABC):
    cheese: str
    def get_cheese(self) -> str:
        return self.cheese
class ParmesanCheese(Cheese):
    def __init__(self):
        self.cheese = 'Parmesan Cheese'
class MozzarellaCheese(Cheese):
    def __init__(self):
        self.cheese = 'Mozzarella Cheese'


class Veggie(ABC):
    veggie: str
    def get_veggie(self) -> str:
        return self.veggie
class Onion(Veggie):
    def __init__(self):
        self.veggie = 'Onion'
class Mushroom(Veggie):
    def __init__(self):
        self.veggie = 'Mushroom'
class BlackOlivies(Veggie):
    def __init__(self):
        self.veggie = 'Black Olivies'
class RedPepper(Veggie):
    def __init__(self):
        self.veggie = 'Red Pepper'


class Sauce(ABC):
    sauce: str
    def get_sauce(self) -> str:
        return self.sauce
class MarinaraSauce(Sauce):
    def __init__(self):
        self.sauce = 'Marinara Sauce'
class PlumTomatoSauce(Sauce):
    def __init__(self):
        self.sauce = 'Plum Tomato Sauce'


class PizzaIngredientFactory(ABC):
    @abstractmethod
    def create_dough(self) -> Dough:
        raise NotImplementedError
    @abstractmethod
    def create_cheese(self) -> Cheese:
        raise NotImplementedError
    @abstractmethod
    def create_sauce(self) -> Sauce:
        raise NotImplementedError
    @abstractmethod
    def create_veggies(self) -> List[Veggie]:
        raise NotImplementedError

class NYPizzaIngredientFactory(PizzaIngredientFactory):
    def create_dough(self) -> Dough:
        return ThinCrustDough()
    def create_cheese(self) -> Cheese:
        return ParmesanCheese()
    def create_sauce(self) -> Sauce:
        return MarinaraSauce()
    def create_veggies(self) -> List[Veggie]:
        return [Onion(), Mushroom(), RedPepper()]

class ChicagoPizzaIngredientFactory(PizzaIngredientFactory):
    def create_dough(self) -> Dough:
        return ThickCrustDough()
    def create_cheese(self) -> Cheese:
        return MozzarellaCheese()
    def create_sauce(self) -> Sauce:
        return PlumTomatoSauce()
    def create_veggies(self) -> List[Veggie]:
        return [BlackOlivies(), Mushroom(), RedPepper()]


class Pizza(ABC):
    name: str
    dough: Dough
    cheese: Cheese
    sauce: Sauce
    veggies: List[Veggie]
    ingredientFactory: PizzaIngredientFactory

    @abstractmethod
    def prepare(self) -> None:
        raise NotImplementedError

    def bake(self) -> None:
        print('Baking...')

    def cut(self) -> None:
        print('Cutting...')

    def box(self) -> None:
        print('Packing...')

    def set_name(self, name: str) -> None:
        self.name = name

class CheesePizza(Pizza):
    def __init__(self, ingredientFactory: PizzaIngredientFactory):
        self.ingredientFactory = ingredientFactory

    def prepare(self) -> None:
        self.dough = self.ingredientFactory.create_dough()
        self.cheese = self.ingredientFactory.create_cheese()
        self.sauce = self.ingredientFactory.create_sauce()
        self.veggies = []

        print(f'Preparing {self.name}...')
        print(f'Tossing {self.dough.get_dough()}...')
        print(f'Spreading {self.sauce.get_sauce()}...')
        print(f'Adding {self.cheese.get_cheese()}...')

class VeggiePizza(Pizza):
    def __init__(self, ingredientFactory: PizzaIngredientFactory):
        self.ingredientFactory = ingredientFactory

    def prepare(self) -> None:
        self.dough = self.ingredientFactory.create_dough()
        self.cheese = self.ingredientFactory.create_cheese()
        self.sauce = self.ingredientFactory.create_sauce()
        self.veggies = self.ingredientFactory.create_veggies()

        print(f'Preparing {self.name}...')
        print(f'Tossing {self.dough.get_dough()}...')
        print(f'Spreading {self.sauce.get_sauce()}...')
        print(f'Adding {self.cheese.get_cheese()}...')
        print('Adding toppings:')
        for veggie in self.veggies:
            print(f'    {veggie.get_veggie()}')

class PizzaStore(ABC):
    def order_pizza(self, type_: str) -> Pizza:
        pizza: Pizza = self.create_pizza(type_)
        pizza.prepare()
        pizza.bake()
        pizza.cut()
        pizza.box()
        return pizza

    @abstractmethod
    def create_pizza(self) -> Pizza:
        raise NotImplementedError

class NYPizzaStore(PizzaStore):
    def create_pizza(self, type_: str) -> Pizza:
        ingredientFactory: PizzaIngredientFactory = NYPizzaIngredientFactory()
        pizza: Pizza = None

        if type_ == "cheese":
            pizza = CheesePizza(ingredientFactory)
            pizza.set_name('NY Style Cheese Pizza')
        elif type_ == "veggie":
            pizza = VeggiePizza(ingredientFactory)
            pizza.set_name('NY Style Veggie Pizza')
        elif type_ == "clam":
            pizza = ClamPizza(ingredientFactory)
            pizza.set_name('NY Style Clam Pizza')
        elif type_ == "pepperoni":
            pizza = PepperoniPizza(ingredientFactory)
            pizza.set_name('NY Style Pepperoni Pizza')

        return pizza

class ChicagoPizzaStore(PizzaStore):
    def create_pizza(self, type_: str) -> Pizza:
        ingredientFactory: PizzaIngredientFactory = ChicagoPizzaIngredientFactory()
        pizza: Pizza = None

        if type_ == "cheese":
            pizza = CheesePizza(ingredientFactory)
            pizza.set_name('Chicago Style Cheese Pizza')
        elif type_ == "veggie":
            pizza = VeggiePizza(ingredientFactory)
            pizza.set_name('Chicago Style Veggie Pizza')
        elif type_ == "clam":
            pizza = ClamPizza(ingredientFactory)
            pizza.set_name('Chicago Style Clam Pizza')
        elif type_ == "pepperoni":
            pizza = PepperoniPizza(ingredientFactory)
            pizza.set_name('Chicago Style Pepperoni Pizza')

        return pizza
```
