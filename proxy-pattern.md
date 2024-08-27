# Proxy Pattern

The Proxy Pattern provides a surrogate or placeholder for another object to control access to it, is expensive to create, or in need of securing.

The most common applications of the Proxy pattern are lazy loading, caching, controlling the access, logging, etc.

## Diagram

## Example

```python
from abc import ABC, abstractmethod


class Subject(ABC):
    @abstractmethod
    def request(self) -> None:
        pass

class RealSubject(Subject):
    def request(self) -> None:
        print("RealSubject: Handling request.")

class Proxy(Subject):
    def __init__(self, real_subject: RealSubject) -> None:
        self._real_subject = real_subject

    def request(self) -> None:
        if self.check_access():
            self._real_subject.request()
            self.log_access()

    def check_access(self) -> bool:
        print("Proxy: Checking access prior to firing a real request.")
        return True

    def log_access(self) -> None:
        print("Proxy: Logging the time of request.", end="")


def client_code(subject: Subject) -> None:
    subject.request()

print("Client: Executing the client code with a real subject:")
real_subject = RealSubject()
client_code(real_subject)

print("")

print("Client: Executing the same client code with a proxy:")
proxy = Proxy(real_subject)
client_code(proxy)
```
