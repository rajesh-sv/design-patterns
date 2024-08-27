# State Pattern

The State Pattern allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

State transitions can be controlled by the State classes or by the Context classes.

## Diagram

## Example

```python
from abc import ABC, abstractmethod


class State(ABC):
    @abstractmethod
    def insert_quarter(self) -> None:
        raise NotImplementedError
    @abstractmethod
    def eject_quarter(self) -> None:
        raise NotImplementedError
    @abstractmethod
    def turn_crank(self) -> None:
        raise NotImplementedError
    @abstractmethod
    def dispense(self) -> None:
        raise NotImplementedError
    @abstractmethod
    def refill(self) -> None:
        raise NotImplementedError

class SoldOutState(State):
    def __init__(self, gumball_machine):
        self.gumball_machine = gumball_machine

    def insert_quarter(self) -> None:
        print("You can't insert a quarter, the machine is sold out")

    def eject_quarter(self) -> None:
        print("You can't eject, you haven't inserted a quarter yet")

    def turn_crank(self) -> None:
        print("You turned, but there are no gumballs")

    def dispense(self) -> None:
        print("No gumball dispensed")

    def refill(self) -> None:
        self.gumball_machine.set_state(self.gumball_machine.get_no_quarter_state())

    def __str__(self) -> str:
        return "sold out"

    def __repr__(self) -> str:
        return str(self)

class NoQuarterState(State):
    def __init__(self, gumball_machine):
        self.gumball_machine = gumball_machine

    def insert_quarter(self) -> None:
        print("You inserted a quarter")
        self.gumball_machine.set_state(self.gumball_machine.get_has_quarter_state())

    def eject_quarter(self) -> None:
        print("You haven't inserted a quarter")

    def turn_crank(self) -> None:
        print("You turned, but there's no quarter")

    def dispense(self) -> None:
        print("You need to pay first")

    def refill(self) -> None:
        pass

    def __str__(self) -> str:
        return "waiting for quarter"

    def __repr__(self) -> str:
        return str(self)

class HasQuarterState(State):
    def __init__(self, gumball_machine):
        self.gumball_machine = gumball_machine

    def insert_quarter(self) -> None:
        print("You can't insert another quarter")

    def eject_quarter(self) -> None:
        print("Quarter returned")
        self.gumball_machine.set_state(self.gumball_machine.get_no_quarter_state())

    def turn_crank(self) -> None:
        print("You turned...")
        self.gumball_machine.set_state(self.gumball_machine.getSoldState())

    def dispense(self) -> None:
        print("No gumball dispensed")

    def refill(self) -> None:
        pass

    def __str__(self) -> str:
        return "waiting for turn of crank"

    def __repr__(self) -> str:
        return str(self)

class SoldState(State):

    def __init__(self, gumball_machine):
        self.gumball_machine = gumball_machine

    def insert_quarter(self) -> None:
        print("Please wait, we're already giving you a gumball")

    def eject_quarter(self) -> None:
        print("Sorry, you already turned the crank")

    def turn_crank(self) -> None:
        print("Turning twice doesn't get you another gumball!")

    def dispense(self) -> None:
        self.gumball_machine.releaseBall()
        if self.gumball_machine.getCount() > 0:
            self.gumball_machine.set_state(self.gumball_machine.get_no_quarter_state())
        else:
            print("Oops, out of gumballs!")
            self.gumball_machine.set_state(self.gumball_machine.get_sold_out_state())

    def refill(self) -> None:
        pass

    def __str__(self) -> str:
        return "dispensing a gumball"

    def __repr__(self) -> str:
        return str(self)

class StringBuffer:
    def __init__(self):
        self.ls = []

    def append(self, string: str) -> None:
        self.ls.append(string)

    def __str__(self) -> str:
        return ''.join(self.ls)

class GumballMachine:
    def __init__(self, number_gum_balls: int):
        self.sold_out_state: State = SoldOutState(self)
        self.no_quarter_state: State = NoQuarterState(self)
        self.has_quarter_state: State = HasQuarterState(self)
        self.sold_state: State = SoldState(self)

        self.state: State = None
        self.count: int = number_gum_balls
        if number_gum_balls > 0:
            self.state = self.no_quarter_state
        else:
            self.state = self.sold_out_state

    def insert_quarter(self) -> None:
        self.state.insert_quarter()

    def eject_quarter(self) -> None:
        self.state.eject_quarter()

    def turn_crank(self) -> None:
        self.state.turn_crank()
        self.state.dispense()

    def releaseBall(self) -> None:
        print("A gumball comes rolling out the slot...")
        if self.count > 0:
            self.count -= 1

    def getCount(self) -> int:
        return self.count

    def refill(self, count: int) -> None:
        self.count += count
        print(f"The gumball machine was just refilled; its new count is: {self.count}")
        self.state.refill()

    def set_state(self, state: State) -> None:
        self.state = state

    def get_state(self) -> State:
        return self.state

    def get_sold_out_state(self) -> State:
        return self.sold_out_state

    def get_no_quarter_state(self) -> State:
        return self.no_quarter_state

    def get_has_quarter_state(self) -> State:
        return self.has_quarter_state

    def getSoldState(self) -> State:
        return self.sold_state

    def __str__(self) -> str:
        result: StringBuffer = StringBuffer()
        result.append("\nMighty Gumball, Inc.")
        result.append("\Python-enabled Standing Gumball Model #2004")
        result.append(f"\nInventory: {self.count} gumball")
        if self.count != 1:
            result.append("s")
        result.append("\n")
        result.append(f"Machine is {self.state}\n")
        return str(result)

    def __repr__(self) -> str:
        return str(self)
```
