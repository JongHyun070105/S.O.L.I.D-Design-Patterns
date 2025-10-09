# Abstract Factory Pattern

- 관련 있는 객체들을 묶음 단위로 생성할 수 있도록 캡슐화하는 패턴

## 장점

- 관련 있는 객체들은 패밀리 단위로 일관성 있게 생성 가능
- 클라이언트 코드가 구체 클래스에 의존 X -> 유연성 증가
- 새로운 테마나 제품군 추가에도 코드 거의 수정 없이 확장 가능

## 단점

- 클래스 수가 늘어나 구조가 복잡해질 수 있음
- 모든 제품군을 한 번에 정의해야 하기 떄문에 작은 변화에도 팩토리 수정이 필요할 수 있음

<br>

## 클래스 다이어그램

![img](/img/abstract_factory.png)

## 코드

```py
class Weapon:
  def display(self):
    pass

class Armor:
  def display(self):
    pass

class ArcherWeapon(Weapon):
  def display(self):
     print("아처의 무기: 활")

class ArcherArmor(Armor):
  def display(self):
     print("아처의 방어구: 갑옷")

class WarriorWeapon(Weapon):
  def display(self):
     print("전사의 무기: 검")

class WarriorArmor(Armor):
  def display(self):
     print("전사의 방어구: 철갑옷")

class ItemFactory:
  def createWeapon(self):
    pass

  def createArmor(self):
    pass

class ArcherFactory(ItemFactory):
  def createWeapon(self):
     return ArcherWeapon()
  def createArmor(self):
     return ArcherArmor()

class WarriorFactory(ItemFactory):
  def createWeapon(self):
     return WarriorWeapon()
  def createArmor(self):
     return WarriorArmor()

archer = ArcherFactory()

archer_weapon = archer.createWeapon()
archer_armor = archer.createArmor()


archer_weapon.display()
archer_armor.display()

print("")

warrior = WarriorFactory()

warrior_weapon = warrior.createWeapon()
warrior_armor = warrior.createArmor()


warrior_weapon.display()
warrior_armor.display()
```
