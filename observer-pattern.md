# Observer Pattern

The Observer Pattern defines a one-to-many dependency between objects so that when one object changes state, all of its dependents are notified and updated automatically.

Publishers (subject) + Subscribers (observers) = Observer Pattern

Whether we push or pull the data to the Observer is an implementation detail, but in a lot of cases it makes sense to let Observers retrieve the data they need rather than passing more and more data to them.

The Observer Pattern is related to the Publish/Subscribe Pattern, which is for more complex situations with multiple Subjects and/or multiple message types.

## Diagram

## Example

```python
from abc import ABC, abstractmethod


class Observer(ABC):
    @abstractmethod
    def update(self) -> None:
        raise NotImplementedError

class Observable(ABC):
    def __init__(self):
        self.observers: set[Observer] = set()

    def add_observer(self, observer: Observer) -> None:
        self.observers.add(observer)

    def remove_observer(self, observer: Observer) -> None:
        self.observers.discard(observer)

    def notify_observers(self) -> None:
        for observer in self.observers:
            observer.update()


class WeatherData(Observable):
    __temp: float
    __pressure: float

    def __init__(self):
        super().__init__()

    def get_temp(self) -> float:
        return self.__temp

    def get_pressure(self) -> float:
        return self.__pressure

    def set_measurements(self, temp: float, pressure: float) -> None:
        self.__temp = temp
        self.__pressure = pressure
        self.measurements_changed()

    def measurements_changed(self) -> None:
        self.notify_observers()


class DisplayElement(ABC):
    @abstractmethod
    def display(self) -> None:
        raise NotImplementedError

class TempDisplay(DisplayElement, Observer):
    __temp: float = 25

    def __init__(self, weather_data: WeatherData):
        self.weather_data = weather_data
        weather_data.add_observer(self)

    def display(self) -> None:
        print(f'Temp: {self.__temp}')

    def update(self) -> None:
        self.__temp = self.weather_data.get_temp()

class PressureDisplay(DisplayElement, Observer):
    __pressure: float = 25

    def __init__(self, weather_data: WeatherData):
        self.weather_data = weather_data
        weather_data.add_observer(self)

    def display(self) -> None:
        print(f'Pressure: {self.__pressure}')

    def update(self) -> None:
        self.__pressure = self.weather_data.get_pressure()
```
