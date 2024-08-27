# Composite Pattern

The Composite Pattern allows you to compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.

## Diagram

## Example

```python
from __future__ import annotations
from abc import ABC, abstractmethod
from typing import Iterator, List


class UnsupportedOperationException(Exception): pass

class MenuComponent(ABC):
    def add(self, menu_component: MenuComponent) -> None:
        raise UnsupportedOperationException
    def remove(self, menu_component: MenuComponent) -> None:
        raise UnsupportedOperationException
    def get_child(self, i: int) -> MenuComponent:
        raise UnsupportedOperationException
    def get_name(self) -> str:
        raise UnsupportedOperationException
    def get_description(self) -> str:
        raise UnsupportedOperationException
    def get_price(self) -> float:
        raise UnsupportedOperationException
    def is_vegetarian(self) -> bool:
        raise UnsupportedOperationException
    def print(self) -> None:
        raise UnsupportedOperationException


class MenuItem(MenuComponent):
    def __init__(self, name: str, description: str, vegetarian: bool, price: float):
        self.name = name
        self.description = description
        self.vegetarian = vegetarian
        self.price = price

    def get_name(self) -> str:
        return self.name

    def get_description(self) -> str:
        return self.description

    def get_price(self) -> float:
        return self.price

    def is_vegetarian(self) -> bool:
        return self.vegetarian

    def create_iterator(self) -> Iterator[MenuComponent]:
        return NullIterator()

    def print(self) -> None:
        print(f"  {self.get_name()}", end="")
        if self.is_vegetarian():
            print(f"(v)", end="")
        print(f", {self.get_price()}")
        print(f"     -- {self.get_description()}")


class Menu(MenuComponent):
    def __init__(self, name: str, description: str):
        self.name = name
        self.description = description
        self.menu_components: List[MenuComponent] = []
        self.iterator: Iterator[MenuComponent] = None

    def add(self, menu_component: MenuComponent) -> None:
        self.menu_components.append(menu_component)

    def remove(self, menu_component: MenuComponent) -> None:
        self.menu_components.remove(menu_component)

    def get_child(self, i: int) -> MenuComponent:
        return self.menu_components[i]

    def get_name(self) -> str:
        return self.name

    def get_description(self) -> str:
        return self.description

    def create_iterator(self) -> Iterator[MenuComponent]:
        if self.iterator == None:
            self.iterator = CompositeIterator(iter(self.menu_components))
        return self.iterator

    def print(self) -> None:
        print(f'\n{self.get_name()}', end="")
        print(f", {self.get_description()}")
        print("---------------------")

        iterator: Iterator[MenuComponent] = iter(self.menu_components)
        menu_component: MenuComponent = next(iterator, None)
        while menu_component:
            menu_component.print()
            menu_component: MenuComponent = next(iterator, None)


class Waitress:
    allMenus: MenuComponent

    def __init__(self, allMenus: MenuComponent):
        self.allMenus = allMenus

    def printMenu(self) -> None:
        self.allMenus.print()
```
