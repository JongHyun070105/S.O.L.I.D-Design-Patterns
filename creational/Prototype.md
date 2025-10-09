# Prototype Pattern

- 기존 객체를 복제해서 새로운 객체를 만드는 패턴

## 장점

- 객체 생성 비용이 큰 경우 효율적임
- 동일 설정 객체를 여러 개 만들 때 편리
- 런타임에서 동적 복제 가능

## 단점

- 깊은 복제와 얕은 복제를 구분해야함
- 객체 내부에 참조가 복잡하면 복제 과정에서 실수 할 수 있음
- 모든 객체가 복제를 지원해야하므로 인터페이스가 필요함

<br>

## 클래스 다이어그램

![img](/img/factory.png)

## 코드

```py
import copy

class PrototypeIceCream:
  def __init__(self, taste, decorate):
    self.taste = taste
    self.decorate = decorate

  def info(self):
    return f"{self.taste}맛에 {self.decorate} 추가"

  def clone(self):
    return copy.deepcopy(self)

class MyIceCream(PrototypeIceCream):
  def __init__(self, taste, decorate, cone):
    super().__init__(taste, decorate)
    self.cone = cone

  def info(self):
    print(f"{super().info()}, {self.cone}에 담음")

class YourIceCream(PrototypeIceCream):
  def __init__(self, taste, decorate):
    super().__init__(taste, decorate)

  def info(self):
    print(f"{super().info()}")

me = MyIceCream("초코", "별사탕", "컵")
me.info()

myMom = me.clone()
myMom.cone = "와플콘"
myMom.info()

print("----------------------")

you = YourIceCream("바닐라", "바나나")
you.info()

yourMom = you.clone()
yourMom.taste = "초코"
yourMom.info()
```
