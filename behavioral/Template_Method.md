# Template Method Pattern

- 여러 클래스에서 공통으로 사용하는 메서드를 템플릿화 하여 상위 클래스에서 정의하고, 하위 클래스마다 세부 동작 사항을 다르게 구현하는 패턴.

## 장점

-

## 단점

-

<br>

```py
class BasicUniform:
  def makeUniform(self):
    self.addTeam()
    self.addName()
    self.addBackNum()
    self.addSize()

  def addTeam(self):
    print("팀: 삼성 라이온즈")

  def addName(self):
    print("이름: 오승환")

  def addBackNum(self):
    print("등번호: 21")

  def addSize(self):
    pass

class JongHyunUniform(BasicUniform):
  def addSize(self):
    print("사이즈: 110")

class LgUniform(BasicUniform):
  def addTeam(self):
    print("팀: LG 트윈스")

  def addName(self):
    print("이름: 이병규")

  def addBackNum(self):
    print("등번호: 99")

  def addSize(self):
    print("사이즈: 95")

class KiaUniform(BasicUniform):
  def addTeam(self):
    print("팀: 기아 타이거즈")

  def addName(self):
    print("이름: 이종범")

  def addBackNum(self):
    print("등번호: 7")

  def addSize(self):
    print("사이즈: 100")

jonghyun = JongHyunUniform()
jonghyun.makeUniform()

print("---------------------")

lgFan = LgUniform()
lgFan.makeUniform()
```
