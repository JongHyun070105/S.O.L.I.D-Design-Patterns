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

## 파이썬 코드

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

## 다트 코드

```dart
class Player {
  String? name;
  String? position;

  void info() {
    print("$name의 포지션은 $position");
  }
}

abstract class Builder {
  void reset();

  Builder setName(String name);

  Builder setPosition(String position);

  Player getPlayer();
}

class PlayerBuilder implements Builder {
  late Player _player;

  PlayerBuilder() {
    reset();
  }

  @override
  Player getPlayer() {
    final result = _player;
    reset();
    return result;
  }

  @override
  void reset() {
    _player = Player();
  }

  @override
  PlayerBuilder setName(String name) {
    _player.name = name;
    return this;
  }

  @override
  PlayerBuilder setPosition(String position) {
    _player.position = position;
    return this;
  }
}

class Director {
  Builder builder;

  Director(this.builder);

  Player constuctPitcher() {
    return builder.setName("김민수").setPosition("투수").getPlayer();
  }

  Player constuctBatter() {
    return builder.setName("김철수").setPosition("유격수").getPlayer();
  }
}

void main(List<String> args) {
  final builder = PlayerBuilder();
  final director = Director(builder);

  final pitcher = director.constuctPitcher();
  final batter = director.constuctBatter();

  pitcher.info();
  batter.info();
}
```
