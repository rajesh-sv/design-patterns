# Singleton Pattern

The Singleton Pattern ensures a class has only one instance, and provides a global point of access to it.

## Diagram

## Example

```python
# Classic Singleton
class Singleton:
    __unique_instance = None
    def __init__(self):
        pass
    @staticmethod
    def get_instance():
        if Singleton.__unique_instance is None:
            Singleton.__unique_instance = Singleton()
        return Singleton.__unique_instance
    # Other useful variables and methods


# Static Singleton - Not Possible
class Singleton:
    __unique_instance = Singleton()
    def __init__(self):
        pass
    @staticmethod
    def get_instance():
        return Singleton.__unique_instance
    # Other useful variables and methods


# Double Checked Locking Singleton - Efficient threadsafe singleton
import threading
class Singleton:
    __unique_instance = None
    __lock__ = threading.Lock()
    def __init__(self):
        pass
    @staticmethod
    def get_instance():
        if Singleton.__unique_instance is None:
            with Singleton.__lock__:
                if Singleton.__unique_instance is None:
                    Singleton.__unique_instance = Singleton()
        return Singleton.__unique_instance
    # Other useful variables and methods


# Enum Singleton
from enum import auto, Enum
class Singleton(Enum):
    UNIQUE_INSTANCE = auto()
    # Other useful variables and methods

```
