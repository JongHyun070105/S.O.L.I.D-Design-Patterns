# Template Method Pattern

- 여러 클래스에서 공통으로 사용하는 메서드를 템플릿화 하여 상위 클래스에서 정의하고, 하위 클래스마다 세부 동작 사항을 다르게 구현하는 패턴.

## 장점

- 중복 코드를 줄이고 공통 로직을 재사용할 수 있음
- 전체 알고리즘 구조를 유지하면서 특정 단계만 재정의 가능
- 코드 변경이 필요한 부분을 명확히 정의하여 유지보수가 용이함
- 자식 클래스가 알고리즘의 특정 단계만 구현하면 되므로 개발이 편리함

## 단점

- 상속을 사용하므로 유연성이 떨어짐
- 알고리즘 구조가 변경되면 모든 하위 클래스에 영향을 줄 수 있음
- 리스코프 치환 원칙을 위반할 수 있음
- 템플릿 메서드의 단계가 많아지면 관리가 어려워질 수 있음

<br>

## 클래스 다이어그램

![img](/img/template_method.png)

## 코드

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
