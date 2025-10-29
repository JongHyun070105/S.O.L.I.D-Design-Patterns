# Decorator pattern

- 객체의 기존 코드를 수정하지 않고, 동적으로 기능을 추가하는 패턴

## 장점

- 기존 코드를 수정 없이 기능 확장 가능 -> OCP 준수
- 다양한 기능 조합이 가능 -> 코드 중복 감소
- 런타임에 기능 추가 가능 -> 유연성 높음

## 단점

- 데코레이터가 많아지면 구조가 복잡해짐
- 객체가 여러 겹으로 호장되면 디버깅이 어려워질 수 있음
- 남용 시 가독성 저하

<br>

## 클래스 다이어그램

![img](/img/decorator.png)

## 파이썬 코드

```py
class Player:
  def wear(self):
    pass

class Batter(Player):
  def wear(self):
    print("착용중인 장비: 야구 방망이, 헬멧", end = "")

class Catcher(Player):
  def wear(self):
    print("착용중인 장비: 포수 마스크", end = "")

class PlayerDecorator(Player):
  def __init__(self, player: Player):
    self.player = player

  def wear(self):
    self.player.wear()

class ElbowGuard(PlayerDecorator):
  def wear(self):
    self.player.wear()
    print(", 팔꿈치 보호대", end = "")

class KneeGuard(PlayerDecorator):
  def wear(self):
    self.player.wear()
    print(", 무릎 보호대", end = "")


class AnkleGuard(PlayerDecorator):
  def wear(self):
    self.player.wear()
    print(", 다리 보호대", end = "")

class PelvicFloorGuard(PlayerDecorator):
  def wear(self):
    self.player.wear()
    print(", 낭심 보호대", end = "")


def makeMyPlayer(player: Player):
    player.wear()
    print("")

batter = Batter()
makeMyPlayer(batter)

batter = ElbowGuard(batter)

batter = KneeGuard(batter)
makeMyPlayer(batter)

print("")

catcher = Catcher()
makeMyPlayer(catcher)

catcher = PelvicFloorGuard(catcher)
makeMyPlayer(catcher)
```

## 다트 코드

```dart
import 'dart:io';

abstract class Player {
  void wear();
}

class Batter extends Player {
  @override
  void wear() {
    stdout.write("착용중인 장비: 야구 방망이, 헬멧");
  }
}

class Catcher extends Player {
  @override
  void wear() {
    stdout.write("착용중인 장비: 포수 마스크");
  }
}

class PlayerDecorator implements Player {
  Player player;

  PlayerDecorator(this.player);

  @override
  void wear() {
    player.wear();
  }
}

class ElbowGuard extends PlayerDecorator {
  ElbowGuard(super.player);

  @override
  void wear() {
    player.wear();
    stdout.write(", 팔꿈치 보호대");
  }
}

class KneeGuard extends PlayerDecorator {
  KneeGuard(super.player);

  @override
  void wear() {
    player.wear();
    stdout.write(", 무릎 보호대");
  }
}

class AnkleGuard extends PlayerDecorator {
  AnkleGuard(super.player);

  @override
  void wear() {
    player.wear();
    stdout.write(", 다리 보호대");
  }
}

class PelvicFloorGuard extends PlayerDecorator {
  PelvicFloorGuard(super.player);

  @override
  void wear() {
    player.wear();
    stdout.write(", 낭심 보호대");
  }
}

void makeMyPlayer(Player player) {
  player.wear();
  print("");
}

void main(List<String> args) {
  Player batter = Batter();
  makeMyPlayer(batter);

  batter = ElbowGuard(batter);
  batter = KneeGuard(batter);

  makeMyPlayer(batter);

  print("");

  Player catcher = Catcher();
  makeMyPlayer(catcher);

  catcher = PelvicFloorGuard(catcher);
  makeMyPlayer(catcher);
}
```
