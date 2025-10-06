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

```py
class Player:
  def play_style(self):
    pass

class Batter(Player):
  def play_style(self, style_input):
    print(f'{style_input}타자')

class Pitcher(Player):
  def play_style(self, style_input):
    print(f'{style_input}투수')

class PlayerControl:
  def __init__(self, player: Player):
    self.player = player

  def choose_style(self, style_input):
    self.player.play_style(style_input)

class PlayerStyle(PlayerControl):
  def strategy(self, strategy_input):
    print(f'{strategy_input} 선택')

batter = PlayerStyle(Batter())
batter.choose_style("밸런스")
batter.strategy("공격")

print("")

pitcher = PlayerControl(Pitcher())
pitcher.choose_style("강속구")
```
