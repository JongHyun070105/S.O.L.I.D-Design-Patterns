# Builder Pattern

- 복잡한 객체를 단계별로 구성하여 생성할 수 있게 하는 패턴

## 장점

- 복잡한 객체를 단계별로 구성 가능
- 생성 과정과 표현을 분리 -> 동일한 구성 요소로 다양한 객체 생성 가능
- 클라이언트는 세부 생성 과정을 신경 안써도 됨

## 단점

- 클래스가 많아지고 코드가 길어질 수 있음
- 단순 객체 생성에는 오히려 불필요하게 복잡해짐

<br>

## 클래스 다이어그램

![img](/img/builder.png)

## 코드
```py
from abc import ABC, abstractmethods

class Player:
    def __init__(self):
        self.name = None
        self.position = None

    def info(self):
        print(f"{self.name}의 포지션은 {self.position}")

class Builder(ABC):
    @abstractmethod
    def reset(self): pass

    @abstractmethod
    def set_name(self, name): pass

    @abstractmethod
    def set_position(self, position): pass

    @abstractmethod
    def get_player(self): pass

class PlayerBuilder(Builder):
    def __init__(self):
        self.reset()

    def reset(self):
        self._player = Player()

    def set_name(self, name):
        self._player.name = name
        return self

    def set_position(self, position):
        self._player.position = position
        return self

    def get_player(self):
        product = self._player
        self.reset()
        return product

class Director:
    def __init__(self, builder: Builder):
        self.builder = builder

    def construct_pitcher(self):
        return self.builder.set_name("김민수").set_position("투수").get_player()

    def construct_batter(self):
        return self.builder.set_name("김철수").set_position("유격수").get_player()

builder = PlayerBuilder()
director = Director(builder)

pitcher = director.construct_pitcher()
pitcher.info()

batter = director.construct_batter()
batter.info()
```
