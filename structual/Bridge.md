# Bridge pattern

- 기능과 구현을 분리하여, 서로 독립적으로 확장할 수 있게 하는 구조 패턴

## 장점

- 기능과 구현을 독립적으로 확장할 수 있음
- 클래스 수가 늘어나는 문제를 방지함
- 유지보수가 쉬움

## 단점

- 구조가 복잡해질 수 있음
- 설계 단계에서 추상화와 구현 분리를 잘 해야함.

<br>

## 클래스 다이어그램

![img](/img/bridge.png)

## 파이썬 코드

```py
from abc import ABC, abstractmethod

class Player(ABC):
  @abstractmethod
  def play_style(self, style_input):
    pass


class Batter(Player):
  def play_style(self, style_input):
    print(f"{style_input} 타자 스타일")


class Pitcher(Player):
  def play_style(self, style_input):
    print(f"{style_input} 투수 스타일")


class PlayerControl:
  def __init__(self, player: Player):
    self.player = player

  def show_style(self, style_input):
    self.player.play_style(style_input)


class PlayerMode(PlayerControl):
  def __init__(self, player: Player, mode: str):
    super().__init__(player)
    self.mode = mode

  def activate_mode(self):
    print(f"{self.mode} 모드가 활성화되었습니다.")


batter = PlayerMode(Batter(), "공격")
batter.activate_mode()
batter.show_style("밸런스")

print("")

pitcher = PlayerMode(Pitcher(), "수비")
pitcher.activate_mode()
pitcher.show_style("강속구")
```

## 다트 코드

```dart
abstract class Player {
  void playStyle(String styleInput);
}

class Batter implements Player {
  @override
  void playStyle(String styleInput) {
    print("$styleInput 타자 스타일");
  }
}

class Pitcher implements Player {
  @override
  void playStyle(String styleInput) {
    print("$styleInput 투수 스타일");
  }
}

abstract class PlayerControl {
  Player player;

  PlayerControl(this.player);

  void showStyle(String styleInput) {
    player.playStyle(styleInput);
  }
}

class PlayerMode extends PlayerControl {
  String? mode;
  PlayerMode(super.player, this.mode);

  void activate_mode() {
    print('$mode 모드가 활성화 되었습니다');
  }
}

void main(List<String> args) {
  final batter = PlayerMode(Batter(), "공격");
  batter.activate_mode();
  batter.showStyle("밸런스");

  print("");

  final pticher = PlayerMode(Pitcher(), "수비");
  pticher.activate_mode();
  pticher.showStyle("강속구");
}
```
