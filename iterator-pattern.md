# Iterator Pattern

The Iterator Pattern provides a way to access the elements of an aggregate object sequentially without exposing the underlying representation. Iterator Pattern places the task of traversal on the iterator object, not on the aggregate.

## Diagram

## Example
```python
from abc import ABC, abstractmethod
from typing import Iterator, List, Dict
from datetime import datetime


class UnsupportedOperationException(Exception): pass


class MenuItem:
    def __init__(self, name: str, description: str, vegetarian: bool, price: float):
        self.name: str = name
        self.description: str = description
        self.vegetarian: bool = vegetarian
        self.price: float = price
        
    def get_name(self) -> str:
        return self.name
    
    def get_description(self) -> str:
        return self.description
    
    def get_price(self) -> float:
        return self.price
    
    def is_vegetarian(self) -> bool:
        return self.vegetarian


class AlternatingDinerMenuIterator(Iterator):
    def __init__(self, items: List[MenuItem]):
        self.items: List[MenuItem] = items
        self.position: int = datetime.weekday(datetime.now()) % 2
        
    def __next__(self) -> MenuItem:
        if self.has_next():
            menu_item: MenuItem = self.items[self.position]
            self.position += 2
            return menu_item
        else:
            raise StopIteration
    
    def has_next(self) -> bool:
        if self.position >= len(self.items) or self.items[self.position] == None:
            return False
        else:
            return True
        
    def remove(self) -> None:
        raise UnsupportedOperationException("Alternating Diner Menu Iterator does not support remove()")


class Menu(ABC):
    @abstractmethod
    def create_iterator(self) -> Iterator[MenuItem]:
        raise NotImplementedError

class DinerMenu(Menu):
    def __init__(self):
        self.menu_items: List[MenuItem] = []
        
        self.add_item("Vegetarian BLT","(Fakin') Bacon with lettuce & tomato on whole wheat", True, 2.99)
        self.add_item("BLT","Bacon with lettuce & tomato on whole wheat", False, 2.99)
        self.add_item("Soup of the day","Soup of the day, with a side of potato salad", False, 3.29)
        self.add_item("Hotdog","A hot dog, with sauerkraut, relish, onions, topped with cheese",False, 3.05)
        self.add_item("Steamed Veggies and Brown Rice","Steamed vegetables over brown rice", True, 3.99)
        self.add_item("Pasta","Spaghetti with Marinara Sauce, and a slice of sourdough bread",True, 3.89)
        
    def add_item(self, name: str, description: str, vegetarian: bool, price: float) -> None:
        menu_item: MenuItem = MenuItem(name, description, vegetarian, price)
        self.menu_items.append(menu_item)
            
    def get_menu_items(self) -> List[MenuItem]:
        return self.menu_items
    
    def create_iterator(self) -> Iterator[MenuItem]:
        return AlternatingDinerMenuIterator(self.menu_items)

class CafeMenu(Menu):
    def __init__(self):
        self.menu_items: Dict[str, MenuItem] = dict()
        
        self.add_item("Veggie Burger and Air Fries",
                     "Veggie burger on a whole wheat bun, lettuce, tomato, and fries",
                     True, 3.99);
        self.add_item("Soup of the day",
                     "A cup of the soup of the day, with a side salad",
                     False, 3.69);
        self.add_item("Burrito",
                     "A large burrito, with whole pinto beans, salsa, guacamole",
                     True, 4.29)
        
    def add_item(self, name: str, description: str, vegetarian: bool, price: float) -> None:
        menu_item: MenuItem = MenuItem(name, description, vegetarian, price)
        self.menu_items[name] = menu_item
        
    def get_items(self) -> Dict[str, MenuItem]:
        return self.menu_items
    
    def create_iterator(self) -> Iterator[MenuItem]:
        return iter(self.menu_items.values())


class Waitress:
    def __init__(self, diner_menu: Menu, cafe_menu: Menu):
        self.diner_menu: Menu = diner_menu
        self.cafe_menu: Menu = cafe_menu
        
    def print_menu(self) -> None:
        diner_iterator: Iterator = self.diner_menu.create_iterator()
        cafe_iterator: Iterator = self.cafe_menu.create_iterator()
            
        print("\nLUNCH")
        self.print_menu_(diner_iterator)
        print("\nDINNER")
        self.print_menu_(cafe_iterator)
        
    def print_menu_(self, iterator: Iterator) -> None:
        menu_item: MenuItem = next(iterator, None) # not all "iterator" has the method "has_next".
        while menu_item != None:
            print(f'{menu_item.get_name()}, ', end="")
            print(f'{menu_item.get_price()} -- ', end="")
            print(menu_item.get_description())
            menu_item: MenuItem = next(iterator, None)
```