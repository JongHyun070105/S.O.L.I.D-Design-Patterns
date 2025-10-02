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

```py
class Player:
  def __init__(self):
    self.name = None
    self.position = None

  def info(self):
    print(f"{self.name}의 포지션은 {self.position}")

class PlayerBuilder:
  def __init__(self):
    self.player = Player()

  def set_name(self,name):
    self.player.name = name
    return self

  def set_position(self, position):
    self.player.position = position
    return self

  def get_player(self):
    return self.player

player = PlayerBuilder()
pitcher = player.set_name("김민수").set_position("투수").get_player()
pitcher.info()

batter = player.set_name("김철수").set_position("유격수").get_player()
batter.info()
```
