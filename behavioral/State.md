# State Pattern

- 객체가 특정 상태에 따라 행위를 달리하는 상황에서, 조건문으로 검사하는 것이 아닌 상태를 객체화하여 상태가 행동을 할 수 있도록 위임하는 패턴.

## 장점

-

## 단점

-

<br>

```py
class Door:
    def __init__(self):
        self.state = ClosedState()  # 초기 상태

    def setState(self, state):
        self.state = state

    def open(self):
        self.state.open(self)

    def close(self):
        self.state.close(self)

    def lock(self):
        self.state.lock(self)

    def unlock(self):
        self.state.unlock(self)

class State:
    def open(self, door: Door):
        pass

    def close(self, door: Door):
        pass

    def lock(self, door: Door):
        pass

    def unlock(self, door: Door):
        pass

class OpenState(State):
    def open(self, door: Door):
        print("문은 이미 열려 있습니다.")

    def close(self, door: Door):
        print("문이 닫힙니다.")
        door.setState(ClosedState())

    def lock(self, door: Door):
        print("문이 열려 있어 잠글 수 없습니다.")

    def unlock(self, door: Door):
        print("문이 열려 있어 잠금 상태가 아닙니다.")


class ClosedState(State):
    def open(self, door: Door):
        print("문이 열립니다.")
        door.setState(OpenState())

    def close(self, door: Door):
        print("문은 이미 닫혀 있습니다.")

    def lock(self, door: Door):
        print("문이 잠깁니다.")
        door.setState(LockedState())

    def unlock(self, door: Door):
        print("문은 잠겨 있지 않습니다.")


class LockedState(State):
    def open(self, door: Door):
        print("문이 잠겨 있어 열 수 없습니다.")

    def close(self, door: Door):
        print("문은 이미 닫혀 있습니다.")

    def lock(self, door: Door):
        print("문은 이미 잠겨 있습니다.")

    def unlock(self, door: Door):
        print("문이 잠금 해제됩니다.")
        door.setState(ClosedState())

myRoom = Door()
myRoom.open()
myRoom.lock()
myRoom.close()
myRoom.lock()
myRoom.unlock()
```
