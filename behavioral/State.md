# State Pattern

- 객체가 특정 상태에 따라 행위를 달리하는 상황에서, 조건문으로 검사하는 것이 아닌 상태를 객체화하여 상태가 행동을 할 수 있도록 위임하는 패턴.

## 장점

- 상태에 따른 행동을 개별 클래스로 분리하여 관리가 용이함
- 새로운 상태를 추가해도 기존 코드 수정이 최소화됨 -> OCP 준수
- 각 상태가 독립적이므로 SRP 준수
- 복잡한 조건문을 제거하여 코드 가독성 향상

## 단점

- 상태가 적을 경우 오히려 코드가 복잡해질 수 있음
- 상태 클래스가 많아지면 클래스 수가 증가함
- 상태 전환 로직이 여러 클래스에 분산될 수 있어 전체 흐름 파악이 어려울 수 있음

<br>

## 클래스 다이어그램

![img](/img/state.png)

## 코드

```py
class Door:
    def __init__(self):
        self.state = ClosedState()

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
